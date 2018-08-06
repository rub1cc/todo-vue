<template>
  <div>
      <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo">
      <div v-for="(todo, index) in todos" :key="todo.id" class="todo-item">
        <div class="todo-item-left">
          <input type="checkbox" v-model="todo.completed">
          <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label" :class="{ completed : todo.completed }">{{ todo.title }}</div>
          <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
        </div>         
        <div class="remove-item" @click="removeTodo(index)">
          &times;
        </div>
      </div>

      <div class="extra-container">
        <div>
          <label>
            <input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos"> Check All
          </label>
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
      todos: [
        {
          'id': 1,
          'title': "Learn vue.js",
          'completed': false,
          'editing': false,
        },
        {
          'id': 2,
          'title': "Create Laravel API",
          'completed': false,
          'editing': false,
        }
      ]
    };
  },
  computed: {
    remaining() {
      return this.todos.filter(todo => !todo.completed).length
    },
    anyRemaining() {
      return this.remaining != 0
    }
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
      })
      
      this.newTodo = ''
      this.idForTodo++
    },
    removeTodo(index) {
      this.todos.splice(index, 1)
    },
    editTodo(todo) {
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
    }
  }
};
</script>

<style lang="scss">
.todo-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;

  &:focus {
    outline: 0;
  }
}

.todo-item {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
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
  border: 1px solid white;
  margin-left: 12px;
}

.todo-item-edit {
  font-size: 20px;
  color: #2c3e50;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  font-family: 'Avenir', Arial, Helvetica, sans-serif;

  &:focus {
    outline: none;
  }
}

.completed {
  text-decoration: line-through;
  color: gray;
}

.extra-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 12px;
  border-top: 1px solid lightgrey;
  padding-top: 20px;
  margin-bottom: 14px;
}

button {
  font-size: 12px;
  background-color: white;
  appearance: none;

  &:hover {
    background: lightgrey;
  }

  &:focus {
    outline: none;
  }
}

.active {
  background: lightgreen;
}
</style>
