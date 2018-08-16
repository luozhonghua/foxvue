<template>
	<el-dialog ref="dialog" custom-class="w-400 h-300" title="修改密码">
		<div class="ovf-auto">
			<el-form ref="formpass" :model="formpass" :rules="rules" label-width="80px" >
				<el-form-item label="旧密码" prop="oldpwd">
					<el-input v-model.trim="formpass.oldpwd"></el-input>
				</el-form-item>
				<el-form-item label="新密码" prop="newpwd">
					<el-input v-model.trim="formpass.newpwd"></el-input>
				</el-form-item>
        <div class="p-t-20">
			    <el-button type="primary" class="fl m-l-20"   @click="edit('formpass')" :loading="isLoading"  >提交</el-button>
		   </div>
			</el-form>
		</div>
	</el-dialog>
</template>
<style>

</style>
<script>
  import http from '../../assets/js/http'
  import fomrMixin from '../../assets/js/form_com'

  export default {
    data() {
      return {
        isLoading: false,
        formpass: {
          auth_key: '',
          oldpwd: '',
          newpwd: ''
        },
        rules: {
          oldpwd: [
            { required: true, message: '请输入旧密码', trigger: 'blur' },
            { min: 6, max: 12, message: '长度在 6 到 12 个字符', trigger: 'blur' }
          ],
          newpwd: [
            { required: true, message: '请输入新密码', trigger: 'blur' },
            { min: 6, max: 12, message: '长度在 6 到 12 个字符', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      open() {
        this.$refs.dialog.open()
      },
      close() {
        this.$refs.dialog.close()
      },
      edit(formName) {
        this.$refs[formName].validate((pass) => {
          if (pass) {
            console.info(pass)
            console.info(this.formpass.oldpwd)
            console.info(this.formpass.newpwd)
            this.isLoading = !this.isLoading
            this.apiPost('admin/setInfo', this.formpass).then((res) => {
              this.handelResponse(res, (data) => {
                _g.toastMsg('success', '修改成功')
                Lockr.rm('authKey')
                Lockr.rm('authList')
                Lockr.rm('sessionId')
                console.info(data.oldpwd)
                setTimeout(() => {
                  router.replace('/')
                }, 1500)
              }, () => {
                this.isLoading = !this.isLoading
              })
            })
          }
        })
      }
    },
    created() {
      this.form.auth_key = Lockr.get('authKey')
    },
    mixins: [http, fomrMixin]
  }
</script>