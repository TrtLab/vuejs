<template>
    <section class="todoapp">
        <header class="header">
           <h1>To do list</h1> 
           <input type="text" class="new-todo" placeholder="Ajouter une tâche" v-model="newTodo" @keyup.enter="addTodo">
        </header>
        <div class="main">
            <input type="checkbox" class="toggle-all" id="toggle-all" v-model="allDone">
            <label for="toggle-all"></label>
            <ul class="todo-list">
                <li class="todo" v-for="todo in filteredTodos" :class="{completed: todo.completed, editing: todo === editing}">
                    <div class="view">
                        <input type="checkbox" v-model="todo.completed" class="toggle">
                        <label @dblclick="edit(todo)">{{ todo.name }}</label>
                        <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
                    </div>
                    <input type="text" class="edit" v-model="todo.name" @keyup.esc="cancelEdit" @blur="doneEdit" @keyup.enter="doneEdit" v-focus=" todo === editing">
                </li>
            </ul>
        </div>
        <footer class="footer" v-show="todos.length > 0">
            <span class="todo-count"><strong>{{ remaining }}</strong> tâche(s) à faire.</span>
            <ul class="filters">
                <li><a href="#" :class="{selected: filter === 'all'}" @click.prevent="filter = 'all'">Toutes les tâches</a></li>
                <li><a href="#" :class="{selected: filter === 'todo'}" @click.prevent="filter = 'todo'">A faire</a></li>
                <li><a href="#" :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'">Faites</a></li>
            </ul>
            <button class="clear-completed" v-show="done" @click.prevent="clearDone">Nettoyer</button>
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
                oldName: ''
            }
        },
        methods: {
            addTodo() {
                this.todos.push({
                    completed: false,
                    name: this.newTodo
                })
                this.newTodo = ''
            },
            deleteTodo(todo){
                this.todos = this.todos.filter(i => i !== todo)
            },
            clearDone(){
                this.todos = this.todos.filter(todo => !todo.completed)
            },
            edit(todo){
                this.editing = todo
                this.oldName = todo.name
            },
            doneEdit(){
                this.editing = null
            },
            cancelEdit(){
                this.editing.name = this.oldName
                this.doneEdit()
            }
        },
        computed: {
            allDone: {
                get(){
                    this.remaining === 0
                },
                set(value){    
                    this.todos.forEach(todo => {
                        todo.completed = value
                    });
                },
            },
            remaining() {
                return this.todos.filter(todo => !todo.completed).length
            },
            done() {
               return this.todos.filter(todo => todo.completed).length
            },
            filteredTodos() {
                if(this.filter === 'todo') {
                    return this.todos.filter(todo => !todo.completed)
                } 
                else if(this.filter === "done") {
                    return this.todos.filter(todo => todo.completed)
                }
                return this.todos    
            }
        },
        directives: {
            focus(el, value) {
                if(value) {
                    el.focus()
                }
            }
        }
    }
</script>

<style src="./todos.css"></style>