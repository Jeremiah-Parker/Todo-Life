<template>
  <div>
     
      <div class="input-group mb-3">
  <input type="text" class="form-control" placeholder="your daily  task" aria-label="Recipient's username" aria-describedby="basic-addon2" v-model="newTodo"  @keyup.enter="addTodo">

  <div class="input-group-append" @click="clickTodo">
    <button class="btn btn-outline-secondary" type="button" color="green" >Add</button>
  </div>
</div>
<transition-group name="fade" enter-active class="animated fadeInDown" leave-active-class="animated fadeOutUp">
 <div v-for= " (todo,index) in todosFiltered" :key="todo.id" class="todo-item">
    
    <div class = "todo-item-left">
      
      <input type="checkbox" v-model="todo.completed">
      
        <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label" :class="{completed: todo.completed}"> {{todo.title}}</div>
     <input v-else  class ="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" 
     @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
    </div>


    <div class="remove-item" @click="removeTodo(index)">
   
<svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" fill="currentColor" class="bi bi-trash" viewBox="0 0 16 16">
  <path d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6z"/>
  <path fill-rule="evenodd" d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1zM4.118 4L4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118zM2.5 3V2h11v1h-11z"/>
</svg>
      </div>
 </div>
</transition-group>
      <div class ="extra-container" v-if="quantityTodo" >
        <div><label><input type="checkbox" :checked="!anyRemaining" @change="checkAll">Check All</label></div>
        <div>{{remaining}} items left</div>
        
        
  </div>
<div class = "extra-container">
<div class="btn-group">
  <button type="button" class="btn btn-success dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
    Action
  </button>
  <div class="dropdown-menu">
    <a class="dropdown-item" :class="{ active: filter == 'all'}" @click="filter = 'all' ">All</a>
    <a class="dropdown-item" :class="{ active: filter == 'active'}" @click="filter = 'active' ">Active</a>
    <a class="dropdown-item" :class="{ active: filter == 'completed'}" @click="filter  = 'completed' " >Done</a>
   
  </div>
</div>

<div>
  <transition name = "fade">
<button type="button" class="btn btn-success" v-if="showCompletedButton" @click="clearCompleted">Complete</button>
  </transition>
</div>

  </div>


  </div>

  


</template>

<script>
 
export default {
  name: 'todo-list',
  data() {

      return {
    newTodo: '',
    idForTodo: 4,
    beforeEditCache:'',
    filter: 'all',
    todos: [
        {

        'id': this.idForTodo,
        'title': this.newTodo,
         'completed' : false,
         'editing': false,
    },



    
    ]
      }
  },

computed:{
  remaining(){
     return this.todos.filter(todo => !todo.completed).length
  },
  anyRemaining(){
    return this.remaining != 0
  },
  todosFiltered(){
    if(this.filter == 'all'){
      return this.todos
    }
    else if(this.filter == 'active'){
        return this.todos.filter(todo => !todo.completed)
    }
    else if(this.filter == 'completed'){
       return this.todos.filter(todo => todo.completed)
    
    
    }

    return this.todos
  },
  showCompletedButton(){
     return this.todos.filter(todo => todo.completed).length > 0
     
  },
  quantityTodo(){
    return this.todos.filter(todo => !todo.completed).length > 0
    
  }

  
},
 
  directives:{
    focus:{
      //directive definition
      inserted: function(el){
        el.focus()
      }
    }
  },
  methods:{
      addTodo(){
        if(this.newTodo.trim().length == 0){
          return
        }
      this.todos.push({
          id: this.idForTodo,
          title: this.newTodo,
          completed: false,
          editing:false
      })
      this.newTodo = ''
      this.idForTodo++
  },
  clickTodo(){
 if(this.newTodo.trim().length == 0){
          return
        }
      this.todos.push({
          id: this.idForTodo,
          title: this.newTodo,
          completed: false,
          editing:false
      })
      this.newTodo = ''
      this.idForTodo++
  },
  removeTodo(index){
   this.todos.splice(index ,1)
  },

editTodo(todo){
  this.beforeEditCache = todo.title
todo.editing = true

if(todo.completed == true){
todo.editing = false
}
},
doneEdit(todo){
   if(todo.title.trim()== ''){
          todo.title = this.beforeEditCache
   }
todo.editing = false


},
cancelEdit(todo){
  todo.title = this.beforeEditCache
  todo.editing =  false
},

checkAll(){
  this.todos.forEach((todo) => todo.completed = event.target.checked )
},

clearCompleted(){
this.todos = this.todos.filter(todo => !todo.completed)
}
  }

}
</script>


<style lang="scss">
@import url("https://gitcdn.github.io/bootstrap-toggle/2.2.2/css/bootstrap-toggle.min.css");

@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css");
@import url('https://fonts.googleapis.com/css2?family=Dosis&family=Karla&family=Quicksand&display=swap');

.input-group-append{
  font-size:10px;
   font: white;
}


 .todo-input{
    width: 100%;
    padding: 10px 18px;
    font-size: 30px;
    margin-bottom: 16px;
    color: navy;



    &:focus{
        outline: 0;
    }
}

.todo-item{
margin-bottom: 12px;
display: flex;
align-items: center;
justify-content: space-between;
animation-duration: 0.3s;

}

.remove-item.btn icon-btn btn-danger{
  cursor: pointer;
  margin-left: 14px;
  &hover{
    color: green;
  }
}


.btn-glyphicon {
    padding:8px;
    background:#ffffff;
    margin-right:4px;
}
.icon-btn {
    padding: 1px 15px 3px 2px;
    border-radius:50px;
}

.todo-item-left{
  display:flex;
  align-items:center;
  color: white;
  text-align: center;
  font-size: 35px;
  font-family: 'Dosis', sans-serif;
font-family: 'Karla', sans-serif;
font-family: 'Quicksand', sans-serif;
}


.todo-item-label{
  padding: 10px;
  border:1px solid none;
  margin-left: 12px;
}

.todo-item-edit{
  font-size: 24px;
  color: #2c3e50;
  margin-left:12px;
  width:100%;
  padding:10px;
  border:1px solid #ccc;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;


  &:focus{
    outline:none;
  }

}

.completed{
  text-decoration: line-through ;
  color:grey; 
}

.extra-container{
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size:16px;
  border-top:1px solid lightgrey;
  padding-top:14px;
  margin-bottom:14px;
}

button{
font-size: 14px;
background-color: white;
appearance: none;

&:hover{
  background: lightgreen;
}

&:focus{
  outline:none;
}

}

.active {
  background: lightgreen;
}

.fade-enter-active, fade-leave-active{
  transition: opacity .2s;
}

.fade-enter, fade-leave-to{
  opacity: 0;
}



</style>


















































































































































































































































