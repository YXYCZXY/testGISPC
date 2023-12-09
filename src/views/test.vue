<template>
    <div class="wrapper" v-if="showArrList.length > 0">
        <el-switch
            size="mini"
            v-model="da"
            active-color="#13ce66"
            inactive-color="#ff4949">
        </el-switch> 
        <el-button type="text" @click="dialogVisible = true">点击打开 Dialog</el-button>
        <div class="wrapper-content" v-for="val in showArrList">
            <div class="secdne">
                <h3 class="left">{{val.SectionName}}--{{val.Source}}</h3>
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
            <el-collapse v-show="da">
                <el-collapse-item title="答案" name="1">
                    <article>
                        {{val.Answer}}
                    </article>
                </el-collapse-item>
            </el-collapse>
        </div>
        <el-dialog
        title="提示"
        :visible.sync="dialogVisible"
        width="30%">
        <span>这是一段信息</span>
        <span slot="footer" class="dialog-footer">
            <el-button @click="dialogVisible = false">取 消</el-button>
            <el-button type="primary" @click="submit">确 定</el-button>
        </span>
        </el-dialog>
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
            dialogVisible:false,
           arrList:[],
           radio:0,
           showArrList:[],
           da:false
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

           this.arrList = _.cloneDeep(arr)
           const randomSample = _.sampleSize(arr, Math.min(20, arr.length));
           this.showArrList = _.cloneDeep(randomSample)
       })
      },
      destroyed:{},
      methods:{
        async submit() {
            console.log(this.showArrList);
            let arr = _.cloneDeep(this.showArrList);
            // 定义一个内部的 async 函数来发送 PUT 请求
            async function updateItem(item) {
                try {
                const response = await axios.put(`http://localhost:3000/mark/${item.id}`, item);
                    // 更新成功后的处理
                    console.log('用户已更新', response.data);
                } catch (error) {
                    // 更新失败时的处理
                    console.error('更新用户失败', error);
                }
            }

            // 在 forEach 中调用 async 函数
            for (const item of arr) {
                await updateItem(item);
            }
            this.dialogVisible = false
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
   