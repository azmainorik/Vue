<template>
  <div>
    <h1>A Todo List </h1>
      <input
      type="text"
      v-model="todoName"
      @keyup.enter="addTodo"
      autocomplete="off"     
      aria-label="Add a new Todo"
      placeholder="Add a new Todo"
      />
        <span class="input-group-button">
          <button @click="addTodo()" 
                  class="button"
        >
            
            <i class="fa fa-plus"></i> Add
          </button>
        </span>
    
      <ul>
       <li
        v-for="todo of todos"
        :class="{ done: todo.done }"
        :key="todo.id"
        @click="doneTodo(todo.id)"
      >
        {{ todo.name }}
        </li>
    </ul>

  </div>
</template>

<script>
import axios from "axios";

const baseURL = "http://localhost:3001/todos";

export default {
  name: "TodoList",
  data() {
    return {
      todos: [],
      todoName: "",
      
    };
  },
  async created() {
    try {
      const res = await axios.get(baseURL);

      this.todos = res.data;
    } catch (e) {
      console.error(e);
    }
  },
  methods: 
  {

    async doneTodo(id){

      try{
         
         await axios.patch('${baseURL}/${id}',{
         done:true ,

         });

         this.todos = this.todos.map(todo => {

           if(todo.id === id)
           {
             todo.done = true;
           }    

           return todo;

         });

       }  catch(e) {

           console.error(e);

      }

    },


    async addTodo() 
    {

      try {

        if (this.todoName) {
        const res = await axios.post(baseURL, { name: this.todoName });

        this.todos = [...this.todos, res.data];

        this.todoName = "";

         }
      } catch (e) {
        console.error(e);
      }
    }
  }


};

</script>

<style scoped>
h1 {
  text-decoration: underline;
  margin-left: 8rem;
  margin-right: 6rem;
}

li {
  color: white;
}

input {
  width: 100%;
  padding: 1rem;
  border-radius: 50px;
  border: 1px solid #fd9644;  
  font-size: 1.5rem;
}


.button
{

  width: 120px;
  height: 60px;
  font-size: 1.1rem;
  margin-left:320px;
  
  color: #FFFFFF;
  background-color: #1E90FF;
  transition: .3s; 
  cursor:pointer;

}

.button:hover {
  animation: pulse 1s infinite;
  transition: .3s;
  box-shadow: 0 0 10px 0 blue inset, 0 0 10px 4px blue;
}


@keyframes pulse {
  0% {
    transform: scale(1);
  }
  70% {
    transform: scale(.9);
  }
    100% {
    transform: scale(1);
  }
}


</style>

