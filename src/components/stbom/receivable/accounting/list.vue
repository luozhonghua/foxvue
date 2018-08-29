<template>
	<div>
		accounting 会计日
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
      
    },
    created() {
      this.apiGet('admin/rules').then((res) => {
        this.handelResponse(res, (data) => {
          
        })
      })
    },
    computed: {
     
    },
    components: {
      
    },
    mixins: [http]
  }
</script>
  