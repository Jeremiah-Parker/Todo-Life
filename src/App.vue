<template>
  <div id="app" class ="container">
    
    <h1 class = "title">TodoLife</h1>
    <h3 class ="read">Next? Do you want to todo something</h3> 
    
    <Todo></Todo>
    
  </div>
</template>

<script>


import Todo from './components/Todo.vue'


export default { 
  name: 'App',
  components: {
    Todo,

  }  
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@300&family=Roboto+Condensed&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Dosis&display=swap');


* {

  background-color: rgb(8,107,186);
}
 



.container{
  max-width:  600px;
  margin:  0 auto;
  margin-bottom:  20px;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*text-align: center;*/
  color: #2c3e50;
  margin-top: 123px;
  font-size: 30px;
  margin-bottom: 1px;
}

.logo{
  display: block;
  margin: 20px auto;
  height:75px;
}

.title{
  text-align: center;
  color: aliceblue;
  margin-bottom: 50px;
  font-size: 110px;
font-family: 'Montserrat', sans-serif;
font-family: 'Roboto Condensed', sans-serif;
}

.read{
  color: white;
  font-size: 30px;
  text-align: center;
  font-family: 'Dosis', sans-serif;
}

</style>
