<template>
	<div>
		<div class="m-b-20 ovf-hd">
			<div class="fl" v-show="addShow">
				<router-link class="btn-link-large add-btn" to="add">
					<i class="el-icon-plus"></i>&nbsp;&nbsp;添加用户
				</router-link>
			</div>
			<div class="fl w-200 m-l-30">
				<el-input placeholder="请输入用户名" v-model="realname">
					<el-button slot="append" icon="search" @click="search()"></el-button>
				</el-input>
			</div>
		</div>
    <!--列表-->
		<el-table ref="multipleTable" :data="tableData" :fit="true"   style="width: 100%" @row-click="handleCurrentChangeBox"  @selection-change="selectItem">
			<el-table-column type="selection" width="50" prop='uuid' reserve-selection=""   ></el-table-column>
      <el-table-column type="index" width="60"  ></el-table-column>
			<el-table-column prop="sname" label="所属组织架构" sortable></el-table-column>
			<el-table-column prop="username" label="用户名" width="200" sortable></el-table-column>
      <el-table-column prop="pname" label="岗位" width="200"></el-table-column>
			<el-table-column prop="remark" label="备注"		width="200"></el-table-column>
			<el-table-column label="状态" width="100">
        <template scope="scope">
          <div>
            {{ scope.row.status | status }}
          </div>
        </template>
			</el-table-column>
			<el-table-column	label="操作"	width="200">
        <template scope="scope">
          <div>
            <span>
              <router-link :to="{ name: 'usersEdit', params: { id: scope.row.id }}" class="btn-link edit-btn">
            编辑
              </router-link>
            </span>
            <span>
              <el-button size="small" type="danger" @click="confirmDelete(scope.$index,scope.row)">删除</el-button>
            </span>
          </div>
        </template>
			</el-table-column>
		</el-table>
    <!-- v-bind 'setting' data to config page bar -->
    <!-- bind event 'page-change' to receive page info change -->
    <!--<v-page :setting="pageSet" @page-change="pageChange"></v-page>-->
  <div class="row mt30 pl15">
     <el-button type="warning" @click="delGroup" :disabled="this.multipleSelection.length === 0">批量删除</el-button>
            <!--disabled值动态显示，默认为true,当选中复选框后值为false-->
  </div>
		<div class="pos-rel p-t-20">
			<btnGroup :selectedData="this.multipleSelection" :type="'users'"></btnGroup>
			<div class="block pages">
				<el-pagination
				@current-change="handleCurrentChange"
				layout="prev, pager, next"
				:page-size="limit"
				:current-page="currentPage"
				:total="dataCount">
				</el-pagination>
      </div>
		</div>
	</div>
</template>

<script>
  import btnGroup from '../../../Common/btn-group.vue'
  import http from '../../../../assets/js/http'
  export default {
    data() {
      return {
        tableData: [],
        dataCount: null,
        currentPage: null,
        realname: '',
        multipleSelection: [],
        limit: 10
      }
    },
    methods: {
      search() {
        router.push({ path: this.$route.path, query: { realname: this.realname, page: 1 }})
      },
      selectItem(val) {
        console.info(' ===  ' + _g.j2s(val))
        this.multipleSelection = val
      },
      delGroup() {
        var ids = this.multipleSelection.map(item => item.id).join()
        this.$confirm('确认删除该用户?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          _g.openGlobalLoading()
          let para = { id: row.id }
          this.apiDelete('admin/users/deletes/', para).then((res) => {
            _g.closeGlobalLoading()
            this.handelResponse(res, (data) => {
              _g.toastMsg('success', '删除成功')
              setTimeout(() => {
                _g.shallowRefresh(this.$route.name)
              }, 1500)
            })
          })
        }).catch(() => {
          // catch error
        })
      },
      handleCurrentChangeBox(row, event, column) {
        this.$refs.multipleTable.toggleRowSelection(row)
      },
      handleCurrentChange(page) {
        router.push({ path: this.$route.path, query: { realname: this.realname, page: page }})
      },
      confirmDelete(index, item) {
        this.$confirm('确认删除该用户?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          _g.openGlobalLoading()
          this.apiDelete('admin/users/delete/', item.id).then((res) => {
            _g.closeGlobalLoading()
            this.handelResponse(res, (data) => {
              _g.toastMsg('success', '删除成功')
              setTimeout(() => {
                _g.shallowRefresh(this.$route.name)
              }, 1500)
            })
          })
        }).catch(() => {
          // catch error
        })
      },
      getAllUsers() {
        this.loading = true
        const data = {
          params: {
            realname: this.realname,
            sname: this.sname,
            page: this.currentPage,
            rows: this.limit
          }
        }
        this.apiGet('admin/users', data).then((res) => {
          console.log('res = ', _g.j2s(res))
          this.handelResponse(res, (data) => {
            this.tableData = data.list
            this.dataCount = data.total
          })
        })
      },
      getCurrentPage() {
        let data = this.$route.query
        if (data) {
          if (data.page) {
            this.currentPage = parseInt(data.page)
          } else {
            this.currentPage = 1
          }
        }
      },
      getRealname() {
        let data = this.$route.query
        if (data) {
          if (data.realname) {
            this.realname = data.realname
          } else {
            this.realname = ''
          }
        }
      },
      init() {
        this.getRealname()
        this.getCurrentPage()
        this.getAllUsers()
      }
    },
    created() {
      this.init()
    },
    computed: {
      addShow() {
        return _g.getHasRule('users-save')
      },
      editShow() {
        return _g.getHasRule('users-update')
      },
      deleteShow() {
        return _g.getHasRule('users-delete')
      }
    },
    watch: {
      '$route' (to, from) {
        this.init()
      }
    },
    components: {
      btnGroup
    },
    mixins: [http]
  }
</script>