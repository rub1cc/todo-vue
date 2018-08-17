<template>
  <div id="app">
    <h1 class="logo">VUE Todos</h1>
    <div class="container">
      <todo-list></todo-list>
    </div>
  </div>
</template>

<script>
import TodoList from "./components/TodoList";

export default {
  name: "App",
  components: {
    TodoList
  }
};
</script>

<style>
* {
  box-sizing: border-box;
}

body {
  background-color: #2771F8;
}

.container {
  max-width: 600px;
  margin: 0 auto;
  background-color: white;
  box-shadow: 0px 20px 20px rgba(0,0,0, .3);
  border-bottom-right-radius: 10px;
  border-bottom-left-radius: 10px;
}

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
  font-size: 20px;
}

.logo {
  text-align: center;
  color: white;
}
</style>
