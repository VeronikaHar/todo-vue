<template>
  <div>
    <h1 class="title">Your to-do list:</h1>
    <md-field>
      <md-button @click="addTodo()" class="md-icon-button addItem">
        <md-icon>add</md-icon>
      </md-button>
      <md-input
        class="add"
        v-model="currentTodo"
        @keyup.enter="addTodo()"
        placeholder="Add a to-do..."
      ></md-input>
    </md-field>
    <md-list class="todos">
      <md-list-item
        v-for="(todo, index) in todos"
        :key="todo.id"
        @dblclick="editTodo(index)"
        class="todo"
        :class="{completed: todo.completed}"
      >
        <md-checkbox v-model="todo.completed" @change="completeTodo(todo)">
          <label v-if="!todo.edit">{{ todo.label }}</label>
          <input
            class="edit"
            type="text"
            v-model="todo.label"
            v-if="todo.edit"
            v-focus
            @blur="doneEdit(index)"
            @keyup.enter="doneEdit(index)"
          >
        </md-checkbox>
        <md-button class="remove" @click="removeTodo(todo)">X</md-button>
      </md-list-item>
    </md-list>
    <footer v-show="todos.length > 0">
      <md-button class="clear" @click="removeAll()">Clear all</md-button>
      <md-button class="clear" @click="removeCompleted()">Clear completed</md-button>
    </footer>
  </div>
</template>

<script>
const focus = {
  inserted(el) {
    el.focus();
  }
};

export default {
  data() {
    return {
      todos: [],
      currentTodo: ""
    };
  },
  directives: { focus },
  methods: {
    addTodo() {
      if (this.currentTodo !== "") {
        this.todos.push({
          id: this.todos.length,
          label: this.currentTodo,
          completed: false,
          edit: false
        });
      }
      this.currentTodo = "";
    },
    removeTodo(todo) {
      var index = this.todos.indexOf(todo);
      this.todos.splice(index, 1);
    },
    editTodo(index) {
      this.todos[index].edit = true;
    },
    doneEdit(index) {
      this.todos[index].edit = false;
    },
    removeAll() {
      this.todos = [];
    },
    removeCompleted() {
      this.todos = this.todos.filter(function(todo) {
        return !todo.completed;
      });
    }
  }
};
</script>

<style>
* {
  font-family: "Karla", sans-serif;
  font-weight: 400;
  font-size: 20px;
  line-height: 1.6;
}

body {
  background-color: #d0ead5;
  margin: auto;
  height: 100%;
}

.clear {
  color: #d0ead5;
  border-radius: 3px;
  width: 50%;
  white-space: nowrap;
  font-family: "Rubik", sans-serif;
  font-size: 18px;
  cursor: pointer;
}

.clear:hover {
  background-color: #d0ead5;
  color: #68a374;
}

div {
  max-width: 80%;
  padding: 2% 5% 5% 10%;
  background-color: white;
  margin: auto;
  margin-top: 5%;
}

:focus {
  outline: #a37c82 auto 5px;
  min-width: 100%;
  bottom: -45%;
  position: absolute;
}

footer {
  max-width: 95%;
  display: flex;
  padding: 5px;
  justify-content: space-between;
  background-color: #68a374;
  border-radius: 3px;
}

h1 {
  color: #a37c82;
  font-family: "Rubik", sans-serif;
  font-weight: 700;
  font-size: 46px;
  text-align: center;
  width: 95%;
}

label {
  position: absolute;
  top: -20%;
}

.md-checkbox,
.md-checkbox-container,
.md-list-item-content > .md-checkbox:first-child {
  margin: 0px;
  padding: 0px;
}

.md-checkbox-container::after {
  border: 2px solid #a37c82;
}

.md-input.add {
  position: absolute;
  top: 30%;
  left: 15%;
  font-size: 18px;
}

.md-field {
  padding: 16px;
  margin: 0px;
  background-color: #68a374;
  text-align: center;
  max-width: 95%;
  border-radius: 3px;
}

.md-input::-webkit-input-placeholder,
.addItem {
  color: #d0ead5;
  padding-left: 2%;
}

.md-input {
  color: #fff;
}

.md-list-item {
  width: 95%;
  display: inline-block;
  border: 1px solid #68a374;
  margin-bottom: 1%;
}

.md-list-item-fake-button.md-list-item-container.md-button-clean {
  max-width: 100%;
}

.md-list-item-content.md-ripple,
.md-ripple,
.md-button-content {
  background: transparent;
  margin: 0px;
  padding: 0px;
  min-width: 100%;
}

.remove {
  color: #b9dabf;
}

.remove:hover {
  color: #a37c82;
}

.todo.completed label {
  text-decoration: line-through;
  color: #be9ba0;
}

@media all and (max-width: 680px) {
  * {
    font-size: 16px;
  }

  .clear {
    width: 100%;
  }

  div {
    padding: 4%;
    padding-bottom: 7%;
    min-width: 97%;
  }
  footer {
    flex-direction: column;
    padding: 0px;
  }

  h1 {
    font-size: 30px;
  }

  .md-button {
    margin: 0px;
  }

  .md-field,
  .md-list-item,
  footer {
    min-width: 100%;
  }

  .md-input.add {
    left: 20%;
  }

  .remove {
    position: absolute;
    left: -25%;
  }
}
</style>
