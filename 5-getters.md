## getters

Getters are another property within the store that house what would be computed properties within components. They allow us to extract complexity out of components and make it easier to share reusable code.

The below example shows how a user can click on a post and then render that specific article by accessing the data from the store. First store is imported into the component.

**Note**: On page load the posts data was loaded and a `v-for` has rendered a list of buttons each of which is showing the relevant post title. This is where the `post.id` is coming from:

1. The user clicks on the post button to see the article and the store property `postId` is set to the post id:

![Screenshot from 2021-05-22 07-27-49](https://user-images.githubusercontent.com/73107656/119217012-3aa7bc80-bacf-11eb-974d-f3b0102b1298.png)

The above actually commits directly to mutations, this can easily be funnelled via actions using the same `setPostId` and changing this to **dispatch**.

Below is the **getter** within the store that sets the property `currentPost` to the post that has the same id as the button clicked by the user.

![Screenshot from 2021-05-22 07-33-17](https://user-images.githubusercontent.com/73107656/119217132-fff25400-bacf-11eb-8825-1b694bbc331f.png)

**Note**: In this example, `currentPost` is initially set in the store as **null**. In the component template there is a `div` with a `v-if` that only shows if `currentPost` has a value and dynamically shows this value.

Once the getter is in place we can change this dynamic value from `store.currentPost` to `store.getters.currentPost`
