<template>
  <div class="hello">
    <h2 v-bind:title="msg" v-if="show" :style="styleObject">Message:{{ msg }}</h2>
    msg:<input v-model="msg"/>     
    <button @click="_clickHandler">msg点击操作</button>
    <hr/>
    <h2>computed:{{ fullMsg }}</h2>
    <button @click="_setFullMsg">computed操作</button>
    <hr/>
    <h2>watch:{{ answer }}</h2>
       watch:<input v-model="watchWord"/>
    <hr/>
    <div @mouseover="_hoverHandler" @mouseleave="_mouseleaveHandler" :class="['animate',isActive ? start : stop]">className变换</div>
     <hr/>
    <template v-if="inputType === 'name'">
      <label>Name</label>
      <input type="text" placeholder="enter UserName" key="Name">
    </template>
    <template v-else-if="inputType === 'email'">
      <label>Email</label>
      <input type="text" placeholder="enter Email" key="Email">
    </template>
    <button @click="_toggleInput">toggleInput</button>
    <hr/>
    <ul> 
      <li v-for="(item,index) in items">
        {{index}} : {{item.message}}
      </li>
    </ul>
    <hr/>
    <label >add todo</label>
    <input type="text" placeholder="add to todosList" v-model="newTodoItem" @keyup.enter="_addTodoItem">  <button @click="_addTodoItem">commit</button>
    <ul>
      <li  v-for="(todoItem,index) in todosList" style="display:block">
        <span  @click="_toggleItem(index)" :style="{textDecoration:todoItem.done ? 'line-through' : 'none'}" :key="'0000'+index">{{todoItem.text}}
        </span>
        <button @click="todosList.splice(index,1)">X</button>
      </li>
    </ul>
    <hr/>
    <label>获取js原生事件</label><button @click="_getEventHandler('a',$event)">click</button>
    <hr/>
    <label>监听enter按键</label><input @keyup.enter.delete="_keyupEnter($event)" placeholder="keyup enter"></input>
    <hr/>
    <input type="checkbox" id="jack" value="jack" v-model ="studentName"><label for="jack">Jack</label>
    <input type="checkbox" id="lucy" value="lucy" v-model ="studentName"><label for="lucy">Jack</label>
    <input type="checkbox" id="tom" value="tom" v-model ="studentName"><label for="tom">Jack</label>
  </div>
</template>

<script>
export default {
  name: 'hello',
  data () {
    return {
      msg: '你好',
      show:true,
      watchWord:'',
      answer:'',
      styleObject:{
        color:"#000"
      },
      isActive:false,
      stop:'animateStop',
      start:'animateStart',
      inputType:'name',
      items:[{message:'v-for'},{message:'的'},{message:'用法'},{message:'v-for'},{message:'item in items'},{message:' {{item.message}}'}],
      todosList:[],
      newTodoItem:'',
      studentName:[]
    }
  },

  created(){
    console.log("create")
  },
  mounted(){
    console.log("mounted")
  },
  updated(){
    console.log("updated")
  },
  destroyed(){
    console.log("destroyed")
  },
  methods:{
    _clickHandler:function(){
      this.msg = new Date().toString();
      this.styleObject.color = this.styleObject.color == "red"?"#000":"red";
    },
    _setFullMsg:function(){
      this.fullMsg = "setFullMsg";
    },
    _requestData:function(){
      setTimeout(()=>{
        this.answer = "请求成功"
      },500);
    },
    _hoverHandler:function(){
      this.isActive = true;
    },
    _mouseleaveHandler:function(){
      this.isActive = false;
    },
    _toggleInput:function(){
      if(this.inputType == "name"){
        this.inputType = "email"
      }else{
        this.inputType = "name"
      }
    },
    _addTodoItem:function(){
      this.todosList.push({text:this.newTodoItem,done:false});
      this.newTodoItem = "";
    },
    _toggleItem:function(index){
      this.todosList[index].done = this.todosList[index].done ? false : true;
    },
    _getEventHandler:function(a,e){
      console.log(a);
      console.log(e);
    },
    _keyupEnter:function(event){
      alert('按下'+event.key);
    }
  },
  computed:{
    fullMsg:{
      get:function(){
        return this.msg + 'getFull';
      },
      set:function(value){
         this.msg = value;
      }
    }
  },
  watch:{
    watchWord:function(value){
      this.answer = "正在请求"+value;
      this._requestData()
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
hr{
  -webkit-margin-before: 50px;
  -webkit-margin-after: 50px;
}
.animate{
  width:50px;
  height: 50px;
  border: 1px solid #000;
  margin:0 auto;
  opacity: 1;
}
.animateStart{
  transform: rotate(720deg) scale(2,2);
  background-color: #000; 
  color:#fff;
  opacity: 0.5;
  transition: all 2s ease-in-out;
}
.animateStop{
  transition: all 2s ease-in;
}
</style>
