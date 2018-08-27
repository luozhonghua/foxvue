

<template>

      <el-tree  ref="tree"
                :data="nodes"
                show-checkbox
                default-expand-all
                node-key="id"
                highlight-current
                :default-checked-keys="resourceCheckedKey"
                :props="defaultProps">
        </el-tree>

</template>



<script>

  export default {
    data() {
      return {
        isLoading: false,
        nodes: [
            {
            id: 111,
            title: '一级 1',
            child: [{
              id: 114,
              title: '二级 1-1',
              child: [{
                id: 9,
                title: '三级 1-1-1'
              }, {
                id: 10,
                title: '三级 1-1-2'
              }]
            }]
           }
        ],
        resourceDialogisShow: false,
        resourceCheckedKey:[],
        defaultProps: {
          children: 'child',
          label: 'title'
        } 
      }
    },
    methods: {
     getGroupInfo(){
          let that = this;
          //that.resourceCheckedKey = [111,114,9,10]
          that.$refs.tree.setCheckedKeys([111,114,9,10]);//获取已经设置的资源后渲染
     },
     created() {
       getGroupInfo()
     }
    }
  }
</script>