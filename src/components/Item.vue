<!--<template> TodoItem Parents(will use in the future)
  <div>
     
      <div class="input-group mb-3">
  <input type="text" class="form-control" placeholder="your daily  task" aria-label="Recipient's username" aria-describedby="basic-addon2" v-model="newTodo"  @keyup.enter="addTodo">

  <div class="input-group-append" @click="clickTodo">
    <button class="btn btn-outline-secondary" type="button" color="green" >Add</button>
  </div>
</div>
<transition-group name="fade" enter-active class="animated fadeInDown" leave-active-class="animated fadeOutUp">
 <todo-item v-for="todo in todosFiltered" :key="todo.id" :todo="todo" :checkAll="!anyRemaining" @removedTodo="removeTodo" @finishedEdit="finishedEdit">
    
    <!--<div class = "todo-item-left">
      <input type="checkbox" v-model="todo.completed">
        <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label" :class="{completed: todo.completed}"> {{todo.title}}</div>
     <input v-else  class ="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" 
     @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
    </div>
    <div class="remove-item" @click="removeTodo(index)">
   <a class="btn icon-btn btn-danger" href="#">
<span class="glyphicon btn-glyphicon glyphicon-trash img-circle text-danger"></span>
Delete
</a>
      </div>
      <Item></Item>
 </todo-item>
 
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
    <a class="dropdown-item" :class="{ active: filter == 'completed'}" @click="filter  = 'completed' " >Completed</a>
   
  </div>
</div>

<div>
  <transition name = "fade">
<button type="button" class="btn btn-success" v-if="showCompletedButton" @click="clearCompleted">Clear Completed</button>
  </transition>
</div>

  </div>
 

  </div>

  


</template>

<script>
import Item from './Item'
export default {
  name: 'todo-list',
  components:{
    Item,
  },
  data() {
   
      return {
    newTodo: '',
    idForTodo: 3,
    beforeEditCache:'',
    filter: 'all',
    todos: [
        {

        'id':1,
        'title': "Burger",
         'completed' : false,
         'editing': false,
    },
    {

        'id':2,
        'title': "Burger",
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
  removeTodo(id){
   const index = this.todos.findIndex((item) => item.id == id)
      this.todos.splice(index, 1)	    
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
},

finishedEdit(data){
const index = this.todos.findIndex((item) => item.id == data.id)
      this.todos.splice(index, 1, data)
}
  }

}
</script>


<style lang="scss">

@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css");

.input-group-append{
  font-size:90px;
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

-->









<!--<template>
<div class = "todo-item">
    <div class = "todo-item-left">
      <input type="checkbox" v-model="completed" @change="doneEdit">
        <div v-if="!editing" @dblclick="editTodo" class="todo-item-label" :class="{completed: completed}"> {{title}}</div>
     <input v-else  class ="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit" 
     @keyup.enter="doneEdit" @keyup.esc="cancelEdit" v-focus>
    </div>
    <div class="remove-item" @click="removeTodo(todo.id)">
   <a class="btn icon-btn btn-danger" href="#">
<span class="glyphicon btn-glyphicon glyphicon-trash img-circle text-danger"></span>
Delete
</a>
      </div>

</div>

</template>

<script>
export default {
    name: 'todo-item',
    props: {
        todo:{
            type: Object,
            required: true,
        },
       
       checkAll:{
          type:Boolean,
          required:true,
       }
        },

        data() {
            return{
              'id':this.todo.id,
              'title': this.todo.title,
              'completed': this.todo.completed,
              'editing': this.todo.editing,
              'beforeEditCache': '',
            }
        },

        watch: {
            checkAll(){
                //if(this.check){
                 //this.completed = true
                 
                 //}
                // else{
                   //  this.completed = this.todo.completed
                // }
                this.completed = this.checkAll ? true: this.todo.completed
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
        methods: {
            removeTodo(id){
              this.$emit('removedTodo', id)
            },

            editTodo(){
              this.beforeEditCache = this.title
               this.editing = true

          if(this.completed == true){
             this.editing = false
                }
},
  doneEdit(){
       if(this.title.trim()== ''){
          this.title = this.beforeEditCache
       }
      this.editing = false
      this.$emit('finishedEdit', {
           
                'id': this.id,
                'title': this.title,
                'completed': this.completed,
                'editing': this.editing,
            


      })
        },
cancelEdit(){
      this.title = this.beforeEditCache
      this.editing =  false
     },

    

     },
  

        }


    
</script>

-->





