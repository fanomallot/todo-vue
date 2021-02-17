<template>
    <div class="box">
        <header >
            <h1>Todos</h1>
            <input type="text" class="input-type" placeholder="add task" v-model="newTodo" required @keyup.enter="addTodo">
        </header>
        <label class="text-center label-input ">
            <input type="checkbox" v-model="allDone">
                All done
        </label>
        <div class="li-div">
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
                {{remain}} unfinished task
            <ul class="filter" >
                <li class="filter-li"><a href="#" :class="{selected: selecttag === 'all'}" @click.prevent="selecttag= 'all'">All</a></li>
                <li class="filter-li"><a href="#" :class="{selected: selecttag === 'unfinish'}" @click.prevent="selecttag= 'unfinish'">Unfinish</a></li>
                <li class="filter-li"><a href="#" :class="{selected: selecttag === 'done'}" @click.prevent="selecttag= 'done'">Done</a></li>
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
.text-center{
    text-align: center;
}
.footer{
    width: 80%;
    margin: 0 auto;
}
.label-input{
    display: flex;
    justify-content: center;
}
.filter{
    list-style: none;
    display: flex;
    justify-content: space-between;
}
.filter-li{
    background: #cccc;
    width: 150px;
    padding: 8px 0;
    text-align: center;
    border-radius: 5px;
}
.filter-li:hover,.filter-li:active{
    outline: none;
    background: transparent;
    box-shadow: 0.5px 0.5px 3px 0.5px rgb(0, 247, 255);
}
.filter-li a{
    padding: 8px 50px;
}
.li-div{
    width: 50%;
    margin: 0 auto;
}
.input-type{
    width: 80%;
    margin-left: 10%;
    padding: 5px;
    border: 1px solid rgba(90, 87, 87, 0.2);
    border-radius: 5px; 
    margin-bottom: 10px;
}
.input-type:focus{
    border: 1px solid rgb(0, 247, 255);
    outline: none;
    box-shadow: 0.5px 0.5px 3px 0.5px rgb(0, 247, 255);
}
h1{
    text-align: center;
    color: rgb(0, 204, 255);
}
.box{
    width: 50%;
    margin: 0 auto;
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
