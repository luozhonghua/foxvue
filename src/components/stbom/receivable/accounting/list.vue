 
<template>
  <div  >
      <div   >
        <h1>Hello Ztree(非异步)</h1>
        <div   v-if='ztreeDataSource.length>0'>
           <vue-ztree :list.sync='ztreeDataSource' :func='nodeClick' :is-open='true' :is-check='true' ></vue-ztree>
        </div>
      </div>
  </div>
</template>

<script>
import vueZtree from '../../../../components/vue-ztree.vue'
export default {
  data () {
    return {
      msg: 'Hello Vue-Ztree-2.0!',
      ztreeDataSource:[],
      dataList:[],
      treeDeepCopy:[],
      parentNodeModel:[],//当前点击节点父亲对象
      nodeModel:null // 当前点击节点对象
    }
  },
  methods:{
    navigateClick:function(index,item){
        // 递归
        var recurFunc = (data,list) => {
            data.forEach((i)=>{
                if(i.id==item.id){
                  i.clickNode = true;
                  this.nodeModel = i;
                  this.parentNodeModel = data;
                }else {
                  i.clickNode = false;
                }

                if(i.children){
                   recurFunc(i.children,i);
                }
            });
        }
        recurFunc(this.treeDeepCopy, this.treeDeepCopy);
        
        
        // 导航
        var self = this;
        for(var i=0;i<self.dataList.length;i++){
            if(index == i){
                self.dataList[i].clickNode = true
            }else{
                self.dataList[i].clickNode = false
            }
        }
        self.dataList.splice(index+1,self.dataList.length - (index+1))
       this.ztreeDataSource = this.treeDeepCopy
     
     console.info("aaaaaaaaaaaaa")

    },
    findById:function(data,parentId) {
        var vm =this;
        for(var i = 0;i<data.length;i++){
            if (parentId == data[i].id){
                console.log(data[i])
                vm.dataList.push(data[i]);
                vm.findById(vm.ztreeDataSource, data[i].parentId)
                return data[i]
            }
            if (data[i].hasOwnProperty('children')){
                vm.findById(data[i].children,parentId)
            }
        }
    },
    // 点击节点
    nodeClick:function(m, parent, trees){
       this.treeDeepCopy = trees;
       this.nodeModel = m;  // 当前点击节点对象
       this.parentNodeModel = parent; //当前点击节点父亲对象
       console.log(m);
       console.log(parent);
       // 导航菜单
       this.dataList=[]
       this.findById(this.ztreeDataSource,m.parentId)
       this.dataList= this.dataList.reverse();
       this.dataList.push(m);
    },
    // 右击事件
    contextmenuClick:function(m){
       console.log(m);
       console.log(event.target);
       console.log("触发了自定义的contextmenuClick事件");
       alert("触发了自定义");
    },
    // 点击展开收起
    expandClick:function(m){
       console.log(JSON.parse(JSON.stringify(m)));
       // 点击异步加载
       if(m.isExpand) {
          // 动态加载子节点, 模拟ajax请求数据
         // 请注意 id 不能重复哦。
         if(m.hasOwnProperty("children")){
            m.loadNode = 1; // 正在加载节点
            setTimeout(()=>{
              m.loadNode = 2; // 节点加载完毕
              m.isFolder = !m.isFolder; 
              m.children.push({
                  id:+new Date(),
                  parentId:m.id,
                  name:"动态加载节点1",
                  path:"",
                  clickNode:false,
                  isFolder:false,
                  isExpand:false,
                  hover:false,
                  loadNode:0,
                  children:[{
                        id:+new Date()+1,
                        parentId:m.id,
                        name:"动态加载末节点",
                        path:"",
                        clickNode:false,
                        isExpand:false,
                        isFolder:false,
                        hover:false,
                        loadNode:0
                  }]
              })
            },1000);
         }
       }
    }
  },
  components:{
    vueZtree
  },
  created() {
       console.info("aaaaaaaaaaaaa")
  },
  mounted (){
     
      // 异步获取数据操作
      setTimeout(()=>{ 
         this.ztreeDataSource = [{
            id:220,
            parentId:0,
            name:"游戏1",
            children:[{
              id:221,
              parentId:220,
              name:"游戏2",
              path:"",
              children:[{
                  id:222,
                  parentId:221,
                  name:"游戏3",
                  path:"",
                  children:[{
                      id:223,
                      parentId:222,
                      name:"游戏4",
                      path:"",
                      children:[{
                          id:224,
                          parentId:223,
                          name:"游戏5",
                          path:"",
                          children:[{
                              id:225,
                              parentId:224,
                              name:"游戏6",
                              path:"",
                              children:[{
                                  id:226,
                                  parentId:224,
                                  name:"游戏末节点",
                                  path:""
                              }],
                          }],
                      }],
                  }],
              }],
           }],
            path:"http://www.baidu.com"
         },{
            id:1,
            parentId:0,
            name:"音乐",
            children:[],
            path:"http://www.baidu.com"
         },{
            id:2,
            parentId:0,
            name:"视频",
            children:[{
               id:3,
               parentId:2,
               name:"电影",
               children:[{
                  id:4,
                  parentId:3,
                  name:"国产电影",
                  path:""
               },{
                  id:5,
                   parentId:3,
                  name:"好莱坞电影",
                  path:""
               },{
                  id:6,
                   parentId:3,
                  name:"小语种电影",
                  path:""
               }]
            },{
               id:7,
               parentId:2,
               name:"短片",
               children:[{
                  id:9,
                  parentId:7,
                  name:"电视剧",
                  path:""
               },{
                  id:10,
                   parentId:7,
                  name:"短片",
                  path:""
               }]
            }],
            path:""
         }]
      },1000);
  }
}
</script>
