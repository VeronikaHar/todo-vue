<template>
  <div>
  <h1 class="title">Your to-do list:</h1>
  <md-field>
    <md-input class="add" v-model="currentTodo" @keyup.enter="addTodo()" placeholder="Add a to-do..."></md-input>
    <md-button @click="addTodo()" class="md-icon-button addItem">
    <md-icon>add </md-icon>
    </md-button>
  </md-field>  
     <md-list class="todos">
        <md-list-item v-for="(todo, index) in todos" :key="todo.id" @dblclick="editTodo(index)" class="todo" :class="{completed: todo.completed}">
        <md-checkbox v-model="todo.completed" value="completed">
        <label v-if="!todo.edit"> {{ todo.label }} </label>
        <input class="edit" type="text" v-model="todo.label" v-if="todo.edit" v-focus @blur="doneEdit(index)"  @keyup.enter="doneEdit(index)">
        </md-checkbox>
        <md-button class="remove" @click="removeTodo(todo)">X</md-button>
      </md-list-item> 
    </md-list>
    <footer>
    <button class="clear" @click="removeAll()" v-show="todos.length > 0">Clear all</button>
    <button class="clear" @click="removeCompleted()" v-show="todos.length > 0">Clear completed</button>
    </footer>
  </div>
</template>

<script>
 const focus = {
    inserted(el) {
      el.focus()
    },
  }

export default {
  data() {
    return {
      todos: [],
      currentTodo: ''
    };
  },
  directives: { focus },
  methods: {
    addTodo() {
      if(this.currentTodo!=='') { this.todos.push({id: this.todos.length, label: this.currentTodo, completed: false, edit: false})
      }
      this.currentTodo = '';
    },
    removeTodo(todo) {
      var index = this.todos.indexOf(todo);
      this.todos.splice(index, 1);
    },
    editTodo(index) {
      this.todos[index].edit=true;
    },
    doneEdit(index){
      this.todos[index].edit=false;
    },
    removeAll(){
      this.todos=[];
    },
    removeCompleted() {
     this.todos=this.todos.filter(function (todo) {
        return !todo.completed
      })
    }
  }
};
</script>

<style>
* {
    font-family: 'Karla', sans-serif;
    font-weight: 400;
    font-size: 20px;
    line-height: 1.6;
}

body {
    background-color: #D0EAD5;
    margin: auto;
    height: 100%;
}

.clear {
    background-color: #68A374;
    color: #D0EAD5;
    border: none;
    padding: 5px;
    border-radius: 3px;
    width: 48%;
    white-space: nowrap;
    font-family: 'Rubik', sans-serif;
    font-size: 18px;
    cursor: pointer;
    margin-top: 3%
}

.clear:hover {
    background-color: #A37C82;
    color: #fff;
}

div {
    max-width: 80%;
    padding: 2% 5% 5% 10%;
    background-color: white;
    margin: auto;
    margin-top: 5%;
}

:focus {
    outline: #68A374 auto 5px;
}

footer {
    max-width: 90%;
    display: flex;
    justify-content: space-between;
}

h1 {
    color: #A37C82;
    font-family: 'Rubik', sans-serif;
    font-weight: 700;
    font-size: 46px;
    text-align: center;
    width: 90%;
}

.md-field {
    padding: 16px;
    margin: 0px;
    background-color: #68A374;
    text-align: center;
    max-width: 90%;
    border-radius: 3px;
}

.md-input::-webkit-input-placeholder,
.addItem {
    color: #D0EAD5;
    padding-left: 2%;
}

.md-input {
    color: #fff;
}

.md-list-item {
    width: 90%;
    display: inline-block;
    border: 1px solid #68A374;
    margin-bottom: 1%;
}

.md-list-item-container {
    min-width: 135%;
}

.md-ripple,
.md-button-content {
    background: transparent;
    margin: 0px;
    padding: 0px;
}

.remove {
    position: relative;
    color: #D0EAD5;
}

.remove:hover {
    color: #A37C82;
}

@media all and (max-width: 610px) {
    * {
        font-size: 18px;
    }

    .clear {
        width: 100%;
        font-size: 16px;
    }

    footer {
        flex-direction: column;
    }

    h1 {
        font-size: 30px;
    }

    .md-field,
    div,
    footer {
        min-width: 95%;
    }
}
</style>