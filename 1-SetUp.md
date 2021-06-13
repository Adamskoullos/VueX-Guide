# Set Up Process

**Relevant for Vue 3 + VueX 4**

Using the Vue CLI and including VueX in the scaffolding provides a **store** folder with an **index.js** file containing the store:

![Screenshot from 2021-06-13 07-13-37](https://user-images.githubusercontent.com/73107656/121797243-f6639400-cc16-11eb-9076-4de8c7888177.png)

It is good practice to add a **modules** folder here to split the store up into separated modules. When we do this the main store (**index.js**) is altered in order to pull in all the modules. All modules are first imported and then added to `modules` within the store:

```js
import { createStore } from "vuex";
import { todoOne } from "./modules/todoOne.js";
import { todoTwo } from "./modules/todoTwo.js";

const store = createStore({
  modules: {
    todoOne,
    todoTwo,
  },
});

export default store;
```

Each module can then be accessed via the `store` access point. Each module contains:

- State
- Mutations
- Actions
- Getters

```js
import axios from "axios";

export const todoOne = {
  namespaced: true,

  state() {
    return {
      todos: [],
      isLoading: false,
      error: "",
      filter: "all",
    };
  },

```

```js
mutations: {
  setTodosData(state, data) {
    state.todos = data;
  },
  setIsLoading(state, boolean) {
    state.isLoading = boolean;
  },
  setError(state, err) {
    state.error = err;
  },
  setUpdateTodo(state, boolean) {
    state.updateTodo = boolean;
  },
  setFilter(state, input) {
    state.filter = input;
  },
},

```

```js
actions: {
  // This first action is the core of all workflows and updates the store.state every time there has been a change
  async fetchTodo(ctx) {
    ctx.commit("setIsLoading", true);
    ctx.commit("setError", "");
    try {
      const res = await axios.get(
        "https://dev-test-api-one.herokuapp.com/todos"
      );
      ctx.commit("setTodosData", res.data);
      ctx.commit("setIsLoading", false);
    } catch (error) {
      console.log(error.message);
      if (error.request) {
        // Code to run...
        console.log(error.request);
      } else if (error.response) {
        // Code to run...
        console.log(error.response.data);
        console.log(error.response.status);
        console.log(error.response.statusText);
        console.log(error.response.headers);
        console.log(error.toJSON);
      } else {
        // Code to run...
        console.log(error.toJSON);
      }
      ctx.commit("setError", "Sorry, unable to fetch todo list at this time");
      ctx.commit("setIsLoading", false);
    }
  },
  async fetchSingleTodo(ctx, todo) {
    try {
      const res = await axios(
        "https://dev-test-api-one.herokuapp.com/todos/" + todo.id
      );

      const newArr = ctx.state.todos.map((todo) => {
        if (todo.id == res.data.id) {
          return res.data;
        }
        return todo;
      });
      ctx.commit("setTodosData", newArr);
    } catch (error) {
      console.log(error.message);
      if (error.request) {
        // Code to run...
        console.log(error.request);
      } else if (error.response) {
        // Code to run...
        console.log(error.response.data);
        console.log(error.response.status);
        console.log(error.response.statusText);
        console.log(error.response.headers);
        console.log(error.toJSON);
      } else {
        // Code to run...
        console.log(error.toJSON);
      }
      ctx.commit("setError", "Unable to access the data base at this time");
    }
  },
  async toggleComplete(ctx, todo) {
    try {
      await axios.patch(
        "https://dev-test-api-one.herokuapp.com/todos/" + todo.id,
        {
          complete: !todo.complete,
        }
      );
      ctx.dispatch("fetchSingleTodo", todo);
    } catch (error) {
      console.log(error.message);
      if (error.request) {
        // Code to run...
        console.log(error.request);
      } else if (error.response) {
        // Code to run...
        console.log(error.response.data);
        console.log(error.response.status);
        console.log(error.response.statusText);
        console.log(error.response.headers);
        console.log(error.toJSON);
      } else {
        // Code to run...
        console.log(error.toJSON);
      }
      ctx.commit("setError", "Unable to access the data base at this time");
    }
  },
  async addTodo(ctx, newTodo) {
    try {
      await axios.post(
        "https://dev-test-api-one.herokuapp.com/todos",
        newTodo
      );
      const res = await axios(
        "https://dev-test-api-one.herokuapp.com/todos/" + newTodo.id
      );
      const newArr = [...ctx.state.todos, res.data];
      ctx.commit("setTodosData", newArr);
    } catch (error) {
      console.log(error.message);
      if (error.request) {
        // Code to run...
        console.log(error.request);
      } else if (error.response) {
        // Code to run...
        console.log(error.response.data);
        console.log(error.response.status);
        console.log(error.response.statusText);
        console.log(error.response.headers);
        console.log(error.toJSON);
      } else {
        // Code to run...
        console.log(error.toJSON);
      }
      ctx.commit("setError", "Unable to access the data base at this time");
    }
  },
  async deleteTodo(ctx, todo) {
    try {
      await axios.delete(
        "https://dev-test-api-one.herokuapp.com/todos/" + todo.id
      );

      const newArr = ctx.state.todos.filter((task) => task.id != todo.id);
      ctx.commit("setTodosData", newArr);
    } catch (error) {
      console.log(error.message);
      if (error.request) {
        // Code to run...
        console.log(error.request);
      } else if (error.response) {
        // Code to run...
        console.log(error.response.data);
        console.log(error.response.status);
        console.log(error.response.statusText);
        console.log(error.response.headers);
        console.log(error.toJSON);
      } else {
        // Code to run...
        console.log(error.toJSON);
      }
    }
  },
  updateTodo(ctx, todo) {
    const newArr = ctx.state.todos.map((task) => {
      if (task.id == todo.id) {
        task.update = !task.update;
        return task;
      }
      return task;
    });
    ctx.commit("setTodosData", newArr);
  },
  async updateTodoText(ctx, todo) {
    try {
      await axios.patch(
        "https://dev-test-api-one.herokuapp.com/todos/" + todo.id,
        {
          update: !todo.update,
          text: todo.text,
          complete: false,
        }
      );
      ctx.dispatch("fetchSingleTodo", todo);
    } catch (error) {
      console.log(error.message);
      if (error.request) {
        // Code to run...
        console.log(error.request);
      } else if (error.response) {
        // Code to run...
        console.log(error.response.data);
        console.log(error.response.status);
        console.log(error.response.statusText);
        console.log(error.response.headers);
        console.log(error.toJSON);
      } else {
        // Code to run...
        console.log(error.toJSON);
      }
    }
  },
  filterTodoList(ctx, input) {
    ctx.commit("setFilter", input);
  },
},
```

```js
getters: {
  filterTodos(state) {
    if (state.filter == "all") {
      return state.todos;
    }
    if (state.filter == "complete") {
      return state.todos.filter((todo) => todo.complete);
    }
    if (state.filter == "incomplete") {
      return state.todos.filter((todo) => todo.complete == false);
    }
  },
},
};
```
