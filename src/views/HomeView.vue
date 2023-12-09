<template>
  <div class="home">
    <el-container>
      <el-aside width="200px">
        <el-radio class="radio" v-model="radio" label="1" @input="clickTitme('1')">测试</el-radio>
        <el-radio class="radio" v-model="radio" label="2" @input="clickTitme('2')">全部</el-radio>
        <el-radio class="radio" v-model="radio" label="3" @input="clickTitme('3')">其他</el-radio>
      </el-aside>
      <el-main>
        <KeepAlive>
        <component :is="activeComponent" />
        </KeepAlive>
      </el-main>
    </el-container>
  </div>
</template>

<script>
import Main from './main';
import Test from './test';
import Other from './other';
 export default {
   name:'HomeView',
   mixins:[],
   components: {
    Main,
    Test,
    Other
   },
   props:{},
   data () {
     return {
      radio:'1',
      activeComponent:'Test'
     }
   },
   computed:{},
   mounted(){},
   destroyed:{},
   methods:{
    clickTitme(val){
      this.radio = val
      if(val === '2'){
        this.activeComponent = 'Main'
      } else if(val === '1') {
        this.activeComponent = 'Test'
      } else {
        this.activeComponent = 'Other'
      }
    }
   }
 }
</script>

<style scoped>
.home{
  width:100%;
  height:100%
}
::v-deep .el-container{
  height:100%
}
::v-deep .el-aside{
  border-right:1px solid #2c3e50;
  padding:10px
}
.radio{
  display: block;
  margin-right:0px;
  margin-bottom:40px
}
 
</style>
