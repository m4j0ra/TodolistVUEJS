
<template>
    <section class="todoapp">
        <header class="header">
            <h1>Todo-List</h1>
            <input type="text" class="new-todo" placeholder="Ajouter une tache" v-model="newTodo" @keyup.enter="addTodo">
        </header>
        <div class="main">
            <label v-show="todos.length > 0">
            <input type="checkbox" class="toggle-all" v-model="allDone" value="Tout cocher" v-show="todos.length > 0" >
            Tout cocher/décocher ici
            </label>
            <ul class="todo-list">
                <li class="todo" v-for="todo in filteredTodos" :class="{completed : todo.completed}">
                    <div class="view" v-show="todo.name != ''">
                        <input type="checkbox" v-model="todo.completed" v-show="todo.name != ''" class="toggle">
                        <label >{{ todo.name }}</label>
                        <button class="destroy" @click.prevent="deleteTodo(todo)">&#10006;</button>

                    </div>
                    
                </li>
            </ul>
        </div>
        <footer class="footer" v-show="todos.length > 0">
            <span class="todo-count"><strong>{{ remaining }}</strong> Tâches à faire</span>
            <ul class="filters">
                <li>
                    <a href="#" class="{selected : filter === 'all'}" @click.prevent="filter = 'all'">Toutes</a>
                </li>
                <li>
                    <a href="#" class="{selected : filter === 'todo'}" @click.prevent="filter = 'todo'">A faire</a>
                </li>
                <li>
                    <a href="#" class="{selected : filter === 'done'}" @click.prevent="filter = 'done'">Faites</a>
                </li>
            </ul>
            <button class="clear-completed"  @click.prevent="deleteCompleted" >Supprimer les tâches complétés</button>
        </footer>
    </section>
</template>

<script>
export default {
    data() {
        return {
            allDone: false, 
            todos: [{
                name: '',
                completed: false
            }],
            newTodo: '',
            filter: 'all'
        }
    },
    methods: {
        addTodo() {
            this.todos.push({
                completed: false,
                name: this.newTodo
            });
            this.newTodo = ''+'';
        },
        deleteTodo (todo) {
            this.todos = this.todos.filter(i => i !== todo)
        },
        deleteCompleted() {
            this.todos = this.todos.filter(todo => !todo.completed)
        }
    },
    computed: {

        allDone: {
            get() {
                return this.remaining === 0

            },
            set(value) {
                this.todos.forEach(todo => {
                    todo.completed = value
                })
            }
        },
        remaining() {
            return this.todos.filter(todo => !todo.completed).length;
        },
        completed () {
            return this.todos.filter(todo => todo.completed).length;
        },

        filteredTodos() {
            if (this.filter === 'todo') {
                return this.todos.filter(todo => !todo.completed); // Utilise !todo.completed pour le filtre 'à faire'
            } else if (this.filter === 'done') {
                return this.todos.filter(todo => todo.completed); // Utilise todo.completed pour le filtre 'fait'
            }
            return this.todos;
        }
    }
}
</script>

<style src="./todos.css"></style>