<template>
    <div>
        <h1>Todos</h1>
        <ul>
            <li v-for='todo of todos' :key='todo.id'>
                {{todo.name}}
            </li>
        </ul>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data(){
        return{
            todos:[]
        };
    },
    async created(){
        try{
            const res = await axios.get('http://localhost:8000/todos')

            this.todos = res.data
        }catch(e){
            console.error(e);
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
}

</style>
