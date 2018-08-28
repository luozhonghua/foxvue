<template>
<div  >
				 test
 </div>
</template>


<script>
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
       
      handleCurrentChangeBox(row, event, column) {
        this.$refs.multipleTable.toggleRowSelection(row)
      },
      handleCurrentChange(page) {
        router.push({ path: this.$route.path, query: { realname: this.realname, page: page }})
      }
      
      
    },
    
    computed: {
      
    },
    watch: {
      '$route' (to, from) {
         
      }
    },
    components: {
    },
    mixins: [http]
  }
</script>
 