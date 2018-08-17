<template>
  <div>
      <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo">

      <div class="filter">
        <div>
          <button :class="{ active : filter == 'all' }" @click="filter = 'all'">All : {{ todos.length }}</button>
          <button :class="{ active : filter == 'active' }" @click="filter = 'active'">Active : {{ todos.filter(todo => !todo.completed).length }}</button>
          <button :class="{ active : filter == 'completed' }" @click="filter = 'completed'">Completed : {{ todos.filter(todo => todo.completed).length }}</button>
        </div>

        <div>
          <transition name="fade">
            <button v-if="showClearCompletedButton" @click="clearCompleted">Clear completed</button>
          </transition>
        </div>      
      </div>
      
      <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
        
        <div v-for="(todo, index) in todosFiltered" :key="todo.id" class="todo-item">
          <div class="todo-item-left">
            <input type="checkbox" v-model="todo.completed">
            <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label" :class="{ completed : todo.completed }">
              {{ todo.title }}
            </div>
            <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
          </div>         
          <div class="remove-item" @click="removeTodo(index)">
            &times;
          </div>
        </div>
      </transition-group>

      <div class="extra-container">
        <div class="check-all">
          <input id="check-all" type="checkbox" :checked="!anyRemaining" @change="checkAllTodos">
          <label for="check-all">Check all</label>
        </div>
        <div>
          {{ remaining }} item(s) left
        </div>
      </div>

  </div>
</template>

<script>
export default {
  name: "todo-list",
  data() {
    return {
      newTodo: "",
      idForTodo: 3,
      beforeEditCache: "",
      filter: "all",
      todos: []
    };
  },
  computed: {
    remaining() {
      return this.todos.filter(todo => !todo.completed).length
    },
    anyRemaining() {
      return this.remaining != 0
    },
    todosFiltered() {
      if(this.filter == 'all') {
        return this.todos
      } else if(this.filter == 'active') {
        return this.todos.filter(todo => !todo.completed)
      } else if(this.filter == 'completed') {
        return this.todos.filter(todo => todo.completed)
      }

      return this.todos
    },
    showClearCompletedButton() {
      return this.todos.filter(todo => todo.completed).length > 0
    },
  },
  directives: {
    focus: {
      inserted: function(el) {
        el.focus()
      }
    }
  },
  methods: {
    addTodo() {
      if(this.newTodo.trim().length == 0) {
        return
      }

      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false,
        editing: false,
      })
      
      this.newTodo = ''
      this.idForTodo++
    },
    removeTodo(index) {
      this.todos.splice(index, 1)
    },
    editTodo(todo) {
      if(todo.completed) {
        return
      }

      this.beforeEditCache = todo.title
      todo.editing = true
    },
    cancelEdit(todo) {
      todo.title = this.beforeEditCache
      todo.editing = false
    },
    doneEdit(todo) {
      if(todo.title.trim().length == 0) {
        todo.title = this.beforeEditCache
      }
      todo.editing = false
    },
    checkAllTodos() {
      this.todos.forEach(todo => todo.completed = event.target.checked)
    },
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed)
    }
  }
};
</script>

<style lang="scss">
@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");

.todo-input {
  width: 100%;
  padding: 10px 0;
  font-size: 18px;
  padding: 16px;
  border: none;
  border-bottom: 1px solid lightgrey;

  &:focus {
    outline: 0;
  }
}

.todo-item {
  padding: 0 16px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  animation-duration: 0.3s;
  border-top: 1px solid lightgrey;
}

.remove-item {
  cursor: pointer;
  margin-left: 14px;

  &:hover {
    color: black;
  }
}

.todo-item-left {
  display: flex;
  align-items: center;
}

.todo-item-label {
  padding: 10px;
  margin-left: 12px;
}

.todo-item-edit {
  font-size: 20px;
  color: #2c3e50;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: none;
  font-family: 'Avenir', Arial, Helvetica, sans-serif;

  &:focus {
    outline: none;
  }
}

.completed {
  text-decoration: line-through;
  color: lightgray;
}

.extra-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 14px;
  border-top: 1px solid lightgrey;
  padding: 16px;

  .check-all label {
    margin-left: 18px;
  }
}

.filter {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 12px;
  padding: 16px;

  button {
    font-size: 12px;
    background-color: white;
    border: none;
    padding: 5px 10px;
    border-radius: 4px;
    font-weight: bold;

    &:hover {
      background: #e4e5e6;
    }

    &:focus {
      outline: none;
    }
  }

  .active {
    background: #2771F8;
    color: white;

    &:hover {
      background: #2771F8;
    }

    .item-amounts {
      background: white;
      color: #2771F8;
    }
  }
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .3s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
