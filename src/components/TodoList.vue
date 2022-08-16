<template>
  <div id="TodoList" class="container">
    <h2>Todo List</h2>
    <div class="input">
      <div v-if="!isEditing">
        <input  v-model="message" type="text" placeholder="enter text">
      <button @click="submit(message)">ADD</button>
      </div>
      <div v-else>
            <input type="text" v-model="message">
            <button @click="update(message,id)">UPDATE</button>
        </div>
  </div>
  <div  v-for="(taskk,index) in tasks " :key='taskk' class="list">
    <div class="task"><span>{{index+1}}</span><p>{{taskk.data}}</p></div>
    <button @click="onDelete(index,taskk.id)" class="delete">Delete</button>
    <button @click="edit(taskk.id,taskk.data,index)" class="edit">Edit</button>   
  </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
   isEditing:false,
  name: 'TodoList',
  mounted() {
     axios.get("http://localhost:5000/todolist")
      .then(response=>{
        this.tasks=response.data
      })
  },
  data(){ 
      return{
      tasks:[
      ],
      input:"",
      isEditing:false,
      message:"",
      id:0
    }
  },
  methods:{
    submit(a){
      var data={"data":`${a}`}
      axios.post("http://localhost:5000/todolist",data)
      .then(response=>{
        console.log(response)
      }),
      this.tasks.push({data:a});
      this.message="";
    },
    onDelete(index,id){
     axios.delete(`http://localhost:5000/todolist/${id}`)
     .then(()=>{
        console.log(this.tasks);
     })
     this.tasks.splice(index,1);
    },
    edit(id,message,index){
      this.isEditing=true;
      this.message=message;
      this.id=id
      console.log(id,message);
      this.message=message,
      this.tasks.splice(index,1);
    },
    update(message,id){
      console.log(message,id)
      var data={"data":`${message}`}
      axios.put(`http://localhost:5000/todolist/update/${id}`,data)
      .then(a=>{
        console.log(a);
      })
      this.isEditing=false
      this.message="" ,
       this.tasks.push({data:message});
    }
  }}
</script>

<style>

.input input{
  width: 30rem;
  height: 1.5rem;
  padding: 5px;
}
.input button{
  height: 2rem;
  background-color: green;
  color: white;
  border: 1px white;
  border-radius: 2px;
  margin-left: 0.5rem;
}
.list{
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 2rem;
}
.list .task{
  background-color:white;
  width: 29rem;
  height: 2rem;
  margin-right: 0.5rem;
 text-align: center;
 padding-bottom: 15px;
 box-shadow: 10px 10px 5px rgb(205, 223, 229);
 border:1px solid  rgb(223, 227, 234);
 border-radius: 10px;
}
.delete{
  background-color: red;
  color: white;
  border: 1px white;
  border-radius: 3px;
  height: 2.5rem;
}
.edit{
  background-color:rgb(113, 172, 214);
  color: white;
  border: 1px white;
  border-radius: 3px;
  margin-left: 2px;
  height: 2.5rem;
}

.task {
  padding-left: 1rem;
  display: flex;
  align-items: center;
  font-size: 1rem;
}
.task span{
  color: crimson;
  margin-right: 2rem;
  margin-top: 1rem;
}
.task p{
  margin-top: 1.9rem;
}

</style>