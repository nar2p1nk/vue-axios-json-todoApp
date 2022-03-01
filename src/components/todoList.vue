<template>
    <div>
        <h1>Todos</h1>
        <input
            type="text"
            v-model="todoName"
            @keyup.enter="addTodo"
            aria-label="Add a new Todo"
            placeholder="Add a new todo"
        />
        <ul>
            <li
                v-for='todo of todos'
                :class="{done:todo.done}"
                :key='todo.id'
                @click='doneTodo(todo.id)'
            >
                {{todo.name}}
            </li>
        </ul>
    </div>
</template>

<script>
import axios from 'axios';

const baseURL = 'http://localhost:8000/todos';

export default {
    data(){
        return{
            todos:[],
            todoName:''
        };
    },
    async created(){
        try{
            const res = await axios.get(baseURL)

            this.todos = res.data
        }catch(e){
            console.error(e);
        }
    },
    methods:{
        async addTodo(){
            try{
                const res = await axios.post(baseURL,{name:this.todoName})

                this.todos = [...this.todos,res.data];
                console.log(this.todos)
                this.todoName = '';
            }catch(e){console.error(e)}
        },
        async doneTodo(id){
            try{
                await axios.patch(`${baseURL}/${id}`,{
                    done:true
                });

                this.todos = this.todos.map(todo =>{
                    if(todo.id === id){todo.done = true;}
                    return todo
                })
            }catch(e){console.error(e)}
        }
    }
};
</script>

<style scoped>
h1{
    text-decoration: underline;
}

li{
    color:white;
    cursor: pointer;
}


input {
  width: 100%;
  padding: 1rem;
  border-radius: 0.4rem;
  border: 1px solid #fd9644;
  margin-bottom: 2rem;
  font-size: 1.5rem;
}

.done {
    text-decoration: line-through;
}

</style>
