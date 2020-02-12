<template>
  <div style="width:500px;">

      <div class="box">
          <h3>图书管理增删改</h3>
          <label for="">
            <div class="shuju">
                  id：<input type="number" max="999999999999" min="0" v-model="bookID" :disabled="flag" placeholder="请输入序号">
                  书名: <input type="text" v-model="bookName" placeholder="请输入书名">
                              
            <button @click="add" :disabled="inputTF">提交</button>
            <button @click="cancel" >取消</button>
            </div>

          </label>
          <div class="total">目前图书有{{total}}本</div>
          <table> 
            
              <thead>
                <th>序号</th>
                <th>图书名字</th>
                <th>增加时间</th>  
                <th>操作</th>    
              </thead>
            
            <tr v-for="(item,index) in books" :key="index">
              <td>{{item.id}}</td>
              <td>{{item.name}}</td>
              <td>{{item.time | timeout}}</td>
              <td>  <a href="" @click.prevent="edit(index)">修改</a> <span> | </span><a href="" @click.prevent="del(item.id)">删除</a></td>
            </tr>


          </table>
      </div>


  </div>
</template>

<script>

export default {
  // 局部过滤器 格式化时间
  filters:{
    "timeout":function(date){
          var dt =new Date(date);
          let y = dt.getFullYear();
          let m = (dt.getMonth() + 1).toString().padStart(2,'0');
          let d = dt.getDate().toString().padStart(2,'0');
          let h = dt.getHours().toString().padStart(2,'0');
          let f = dt.getMinutes().toString().padStart(2,'0');
          let s = dt.getSeconds().toString().padStart(2,'0');
          let keep = y + '.' + m + '.' + d + '  ' + h + ':' + f + ':' + s;
          return keep ;
    }
  },
  name: 'HelloWorld',
  data () {
    return { 
            inputTF:false,
            flag:false,
            bookID:'',
            bookName:'',
          books:[
              {id:1,name:"明朝那些事儿",time:Date()},
              {id:2,name:"宋朝那些事儿",time:Date()},
              {id:3,name:"清朝哪些事儿",time:Date()},
              {id:4,name:"民国那些事儿",time:Date()}

          ],
          
    }
  },
  methods:{
    //添加图书
      add(){
        //看ID是否能输入，能输入则是添加，不能输入则是修改
            if(this.flag){
                  this.books.some((item)=>{

                        if(item.id == this.bookID){
                            
                            item.name=this.bookName;
                            this.bookID='';
                            this.bookName="";
                            this.flag=false;
                            return true
                        }


                  })

            } else{
              //查看输入框是否为空，为空的话则不能添加
              if(this.bookID=="" && this.bookName==""){

                  alert("请输入")

              }else if(this.bookName==""){

                  alert("名字不能为空")

              }else if(this.bookID==""){

                  alert("ID不能为空")

              }else{

              this.books.push({id:this.bookID,name:this.bookName,time:Date()})
              this.bookID='';
              this.bookName="";

              }


            }         

         
      },
      //对图书进行编辑
      edit(index){
            this.flag=true;
            this.bookID=this.books[index].id;
            this.bookName=this.books[index].name;

      },
      //删除图书
      del(id){

         var index = this.books.findIndex((item)=>{
                    return item.id == id
            })
           this.books.splice(index,1);

      },
      cancel(){

          this.bookID="";
          this.bookName="";
          this.flag=false;
      }

  },
  //实时监测图书数量
  computed:{

          total(){

          return this.books.length;

          }

    },
    watch:{
        //判断图书是否存在
        bookID(val){
            
            var a=this.books.some((item)=>{

                   return  item.id == val
                   
            })
            if(a){
              this.inputTF=true
            }else{
              this.inputTF=false
            }

        },
         bookName(val){
            
            var a=this.books.some((item)=>{

                   return  item.name == val
                   
            })
            if(a){
              this.inputTF=true
            }else{
              this.inputTF=false
            }

        }

        // name(val){
        //     this.books.some((item)=>{

        //           if(item.id==val){

        //                 this.inputTF=true;

        //           }else{
        //             return false;
        //           }
        //     });

            // if(flag){
            //   this.inputTF=true;
            // }else{

            //   this.inputTF=false;

            // }

        // }

    }
}

    
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
table,tr,td,th{
  border:1px solid #666666;
  text-align:center;
}
table{
  width:500px;
}
.box h3,.shuju,.total,h3{
  text-align:center;
  /* background-color:yellowgreen; */
  border:1px solid #666666;
}
.box h3{
  margin:0px;
  padding:0px;
}
</style>
