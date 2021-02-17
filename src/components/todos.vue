<template>
    <div>
        <header>
            <h1>Todos</h1>
            <input type="text" placeholder="add task" v-model="newTodo" required @keyup.enter="addTodo">
        </header>
        <label >
                All done
            <input type="checkbox" v-model="allDone">
            </label>
        <div>
            <ul>
                <li :class="{completed: todo.completed}" v-for="(todo, index) in filtered" :key="index">
                    <input type="checkbox" v-model="todo.completed">
                    <label @dblclick.prevent="editData(todo)"> {{todo.name}} </label>
                    <button @click.prevent="remove(todo)">delete</button>
                    <input type="text" v-show="editCheck == todo" @blur="closeEdit" v-model="todo.name" @keyup.enter="closeEdit" v-focus="editCheck == todo" @keyup.esc="cancelEdit(todo)">
                </li>
            </ul>
        </div>
        <div class="footer" v-if="todos.length > 0 ">
            <ul class="filter" >
                {{remain}} unfinished task
                <li><a href="#" :class="{selected: selecttag === 'all'}" @click.prevent="selecttag= 'all'">All</a></li>
                <li><a href="#" :class="{selected: selecttag === 'unfinish'}" @click.prevent="selecttag= 'unfinish'">Unfinish</a></li>
                <li><a href="#" :class="{selected: selecttag === 'done'}" @click.prevent="selecttag= 'done'">Done</a></li>
            </ul>
            <button class="deleteall" v-show="checked > 0 " @click.prevent="destroy_all">
                delete all
            </button>
        </div>
        <input type="text" v-show="editChec" v-model="editTodo">
    </div>
</template>

<script>

export default {
    props: {
        value: {type: Array, default () {return []}}
    },
    data () {
    return { todos: this.value,
    newTodo: '',
    selecttag: 'all',
    editCheck: false,
    oldName: ''
    } },
    watch:{
        value(value){
            this.todos = value
        }
    },
    methods: {
        addTodo () {
            this.todos.push({
                name: this.newTodo,
                completed: false
            })
            this.newTodo = ''
        },
        remove (todo) {
            this.todos = this.todos.filter(i => i !== todo)
            this.$emit('input',this.todos)
        },
        destroy_all () {
            this.todos = this.todos.filter(i => i.completed !== true)
            this.$emit('input',this.todos)
        },
        editData (todo) {
            this.oldName = todo.name
            this.editCheck = todo
        },
        closeEdit () {
            this.editCheck = false
        },
        cancelEdit (todo) {
            this.editCheck.name = this.oldName  
            this.closeEdit()
        }
    },
    computed: {
        remain () {
            return this.todos.filter(todo => !todo.completed).length
        },
        filtered () {
            if (this.selecttag === 'all'){
                return this.todos
            }
            else if(this.selecttag === 'unfinish'){
                return this.todos.filter(todo => !todo.completed)
            }
            else{
                return this.todos.filter(todo => todo.completed)
            }
        },
        allDone: {
            get () {
                return this.remain === 0
            },
            set (value) {
                this.todos.forEach(todo => {
                    todo.completed = value
                })
            }
        },
        checked () {
            return this.todos.filter(todo => todo.completed).length
        }
    },
    directives: {
        focus (el) {
            el.focus()
        }
    }
}

</script>

<style>
h1{
    color: red;
}
.completed{
    color: rgba(0, 0, 0, 0.466);
    text-decoration: line-through;
}
a{
    text-decoration: none;
    color: black;
}
.selected{
    color: aqua;
}
</style>
