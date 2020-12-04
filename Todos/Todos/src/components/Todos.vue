<template lang="html">
  <section class="todoapp">
    <header class="header">
      <h1>Todos</h1>
      <input @keyup.enter="addTodo" class="new-todo" placeholder="Ajouter une tache" type="text" v-model="newTodo">
    </header>
    <div class="main">
      <input class="toggle-all" type="checkbox" v-model="allDone">
      <ul class="todo-list">
        <li :class="{completed: todo.completed, editing: todo === editing}" class="todo" v-bind:key="todo.index"
            v-for="todo in filteredTodos">
          <div class="view">
            <input class="toggle" type="checkbox" v-model="todo.completed">
            <label @dblclick="editTodo(todo)">{{todo.name}}</label>
            <button @click.prevent="deleteTodo(todo)" class="destroy"></button>
          </div>
          <input @blur="doneEdit" @keyup.enter="doneEdit" @keyup.esc="cancelEdit" class="edit" type="text"
                 v-focus="todo === editing" v-model="todo.name">
        </li>
      </ul>
    </div>
    <footer class="footer" v-show="hasTodos">
      <span class="todo-count"><strong>{{ remaining }}</strong> taches à faire</span>
      <ul class="filters">
        <li><a :class="{selected: filter === 'all'}" @click.prevent="filter = 'all'" href="#">Toutes</a></li>
        <li><a :class="{selected: filter === 'todo'}" @click.prevent="filter = 'todo'" href="#">A faire</a></li>
        <li><a :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'" href="#">Faites</a></li>
      </ul>
      <button @click.prevent="deleteCompleted" class="clear-completed" v-show="completed">Supprimer les tâches finies
      </button>
    </footer>
  </section>
</template>

<script>
  export default {
    data() {
      return {
        todos: [],
        newTodo: '',
        filter: 'all',
        editing: null,
        oldTodo: ''
      }
    },
    methods: {
      addTodo() {
        this.todos.push({
          completed: false,
          name: this.newTodo
        });
        this.newTodo = ''
      },
      deleteTodo(todo) {
        this.todos = this.todos.filter(i => i !== todo)
      },
      deleteCompleted(todo) {
        this.todos = this.todos.filter(todo => !todo.completed)
      },
      editTodo(todo) {
        this.editing = todo;
        this.oldTodo = todo.name
      },
      doneEdit() {
        this.editing = null
      },
      cancelEdit() {
        this.editing.name = this.oldTodo;
        this.doneEdit()
      }
    },
    computed: {
      allDone: {
        get() {
          return this.remaining === 0
        },
        set(value) {
          this.todos.forEach(function (todo) {
            todo.completed = value
          })
        }
      },
      remaining() {
        return this.todos.filter(todo => !todo.completed).length
      },
      completed() {
        return this.todos.filter(todo => todo.completed).length
      },
      filteredTodos() {
        if (this.filter === 'todo') {
          return this.todos.filter(todo => !todo.completed)
        } else if (this.filter === 'done') {
          return this.todos.filter(todo => todo.completed)
        }
        return this.todos
      },
      hasTodos() {
        return this.todos.length > 0
      }
    },
    directives: {
      focus(el, value) {
        if (value) {
          el.focus()
        }
      }
    }
  }
</script>

<style src="./todos.css"></style>
