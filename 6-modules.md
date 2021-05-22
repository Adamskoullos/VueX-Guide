## Modules

Using modules is the best practice even for small applications as this puts the provisions in place to manage highly scalable growth moving forward.

To do this the main entry point: `store / index.js` is used to pull all modules in and make them accessible to the application:

```js
import { createStore } from "vuex";
import { posts } from "./posts.js";

export const store = createStore({
  modules: {
    posts,
  },
});
```

The above example shows that each module is first imported, from the `store` folder and then each module is added to the modules object which in turn is exported ready to be imported within application components.

Following on from there, below is the pattern used to structure each individual module. Notice that `namespace` is set to `true`, this scopes each module meaning that (within a component) we access module properties by using the namespace entry point, ex: `store.state.`**posts**`.posts`:

```js
import { testPosts } from "../microblog/testPosts.js";

const delay = () =>
  new Promise((res) => {
    setTimeout(res, 1000);
  });

export const posts = {
  namespaced: true,

  state() {
    return {
      post: null,
    };
  },

  mutations: {
    setPostData(state, post) {
      state.post = post;
    },
  },

  actions: {
    async fetchPostData(ctx, id) {
      await delay();
      const post = testPosts.find((x) => x.id === id);
      ctx.commit("setPostData", post);
    },
  },

  getters: {
    postTitle(state) {
      if (!state.post) {
        return "";
      }
      return `#${state.post.id} ${state.post.title}`;
    },
  },
};
```

The name spacing also has some further nuances that need to be considered when working with a module from within a component.
To access a module and its properties we first import and save `useStore()` to a const as normal as going first through the store is always the entry point.

1. To access a modules property value directly: `store.state.`**moduleName**`.propertyName`
2. When using a computed property with a getter to we use bracket notation to access the module first then the property. Or when dispatching to actions the first string argument `module/method`:

```js
const store = useStore();

const fetchPost = (post) => {
  store.dispatch("posts/fetchPostData", post.id);
};
const post = computed(() => {
  return store.state.posts.post;
});
const postTitle = computed(() => {
  return store.getters["posts/postTitle"];
});
return {
  postTitle,
  post,
  fetchPost,
  posts,
};
```
