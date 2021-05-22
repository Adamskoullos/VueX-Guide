## Updating State

The crux of managing state in VueX is the core process of updating state.

`Dispatch an Action -> Commit a Mutation -> Update the State`

![Screenshot from 2021-05-22 06-35-37](https://user-images.githubusercontent.com/73107656/119215808-f107a380-bac7-11eb-9ea4-3670de788f56.png)

Note:

- We pass in the **context** (`ctx`) when working with `actions`

- We pass in the **state** when working with `mutations`

The **context** property is similar to state and has the following methods that are used via dot notation:

```js
context.commit(); // committing the change to mutations

context.dispatch(); // to trigger another action

context.state(); // gives access to the current state
```

## The core process

The application has loaded, loading any initial data and the user has interacted with the application to update some data:

1. A function is invoked within a component to grab some data from an api and render to the DOM. This first step takes care of connecting to the store and by using the **dispatch** keyword we target the **actions** and invoke the `fetchPosts` function:

![Screenshot from 2021-05-22 06-45-18](https://user-images.githubusercontent.com/73107656/119216067-4d1ef780-bac9-11eb-9c42-a502e48e3c28.png)

2. Within actions in the store the `fetchPosts` aysnc function is called, if there is a successful response returned, context is used with the **commit** keyword to commit the updates to mutations. The mutations function is passed in as the first argument with the data as the second:

![Screenshot from 2021-05-22 06-54-46](https://user-images.githubusercontent.com/73107656/119216251-9d4a8980-baca-11eb-911a-550aa04d0756.png)

3. The returned data is committed to mutations, calling the `setPosts` method, which in turn updates the state, which renders the new state to the DOM.

![Screenshot from 2021-05-22 07-03-07](https://user-images.githubusercontent.com/73107656/119216471-cc152f80-bacb-11eb-9e0d-808130491afc.png)
