<template>
	<div>
		<div class="m-b-20">
  	  
		</div>
		
		<el-table
		:data="tableData"
		style="width: 100%"
		@selection-change="selectItem">

			<el-table-column
			type="selection"
			:context="_self"
			width="50">
			</el-table-column>

			<el-table-column
			prop="p_title"
			label="上级菜单"
			width="150">
			</el-table-column>

			<el-table-column
			prop="title"
			label="菜单">
			</el-table-column>
		<!-- <el-table-column
		prop="menu_type"
		label="类型"
		width="200">
		</el-table-column> -->


		  <!-- <el-table-column
			prop="title"
			label="title">
			</el-table-column>   -->
		 
			<el-table-column
			inline-template
			label="状态"
			width="100">
				<div>
					{{ row.status | status}}
				</div>
			</el-table-column>

		 

		</el-table>
	 
	</div>
</template>

<script>
  import btnGroup from '../../../Common/btn-group.vue'
  import http from '../../../../assets/js/http'

  export default {
    data() {
      return {
        tableData: [],
        multipleSelection: []
      }
    },
    methods: {
      selectItem(val) {
        this.multipleSelection = val
      },
      confirmDelete(item) {
        this.$confirm('确认删除该菜单?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          _g.openGlobalLoading()
          this.apiDelete('admin/menus/delete/', item.id).then((res) => {
            _g.closeGlobalLoading()
            this.handelResponse(res, (data) => {
              _g.toastMsg('success', '删除成功')
              setTimeout(() => {
                _g.shallowRefresh(this.$route.name)
              }, 1500)
            })
          })
        }).catch(() => {
          // handel error
        })
      }
    },
    created() {
      this.apiGet('admin/menus').then((res) => {

        this.handelResponse(res, (data) => {
			 
        })
      })
    },
    computed: {
      addShow() {
        return _g.getHasRule('menus-save')
      },
      editShow() {
        return _g.getHasRule('menus-update')
      },
      deleteShow() {
        return _g.getHasRule('menus-delete')
      }
    },
    components: {
      btnGroup
    },
    mixins: [http]
  }
</script>