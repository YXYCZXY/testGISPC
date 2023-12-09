<template>
    <div class="wrapper">
       <div class="top">
           <el-radio v-model="radio" :label="item.title" v-for="item in arrList" @input="clickTitme(item)">{{item.title}}</el-radio>
       </div>
       <div class="wrapper-item" v-for="item in showArrList">
           <h2 class="left">{{item.title}}</h2>
           <div class="wrapper-content" v-for="val in item.data">
               <div class="secdne">
                   <h3 class="left">{{val.SectionName}}</h3>
                   <el-switch
                       size="mini"
                       v-model="val.Bol"
                       active-color="#13ce66"
                       inactive-color="#ff4949">
                   </el-switch> 
               </div>
               <div>
                   <el-input
                   type="textarea"
                   autosize
                   placeholder="请输入内容"
                   v-model="val.Respond">
                   </el-input>
               </div>
               <el-collapse>
                   <el-collapse-item title="答案" name="1">
                       <article>
                           {{val.Answer}}
                       </article>
                   </el-collapse-item>
               </el-collapse>
   
   
           </div>
       </div>
    </div>
   </template>
   
   <script>
   import axios from 'axios';
   import _ from 'lodash';
    export default {
      name:'',
      mixins:[],
      components: {
   
      },
      props:{},
      data () {
        return {
           arrList:[],
           radio:0,
           showArrList:[]
        }
      },
      computed:{},
      mounted(){
       axios.get('http://localhost:3000/mark').then(res => {
           let arr = res.data.data
           arr = arr.map(item => {
               const correctedStr = item.Answer.replace(/[\\{}"]/g, '')
               item.Answer = correctedStr.split(',')
               return item
           })
           
           arr = arr.map(item => {
            if(!item.Bol){
                return item
            }
           })
           arr = arr.filter(item => item)
           let count = arr.map(item => item.Source)
           count = [... new Set(count)]
   
           let newArr = []
           count.forEach(item => {
               let data = arr.filter(val => val.Source === item)
               let obj = {
                   title:item,
                   data:data
               }
               newArr.push(obj)
           })
   
           newArr = newArr.map(item => {
               item.title = item.title.replace('.md','')
               item.title = Number(item.title)
               return item
           })
           newArr.sort(function(a, b) {
               return a.title - b.title;
           });
           this.arrList = _.cloneDeep(newArr)
           this.showArrList = _.cloneDeep(newArr)
       })
      },
      destroyed:{},
      methods:{
       clickTitme(item){
           this.showArrList = _.cloneDeep([item])
       }
      }
    }
   </script>
   
   <style scoped>
   .wrapper{
       height:100%;
       width:100%
   }
   .left{
       text-align: left;
       margin-right:30px
   }
   .secdne{
       display: flex;
       align-items: center;
   
   }
   </style>
   