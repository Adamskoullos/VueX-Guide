# Structure

## src / store / store.js

1. Import Vue & VueX
2. Tell Vue to use VueX
3. Create new VueX store:
4. Import store into **main.js** to make it globally available within the app

Installing VueX from the Vue CLI (image from VueX 3)

![Screenshot from 2021-05-21 12-30-05](https://user-images.githubusercontent.com/73107656/119130882-b43c9d80-ba30-11eb-96d7-3aa401c95ede.png)

## Pulling data into the store

**Actions** are used to connect with data api's (async code) and pull the data in. This inturn **dispatches** the returned data to **mutations**. At this point the mutations directly **commit** changes to the **store** which maintains the current state of all data.

## Updating the store via user input

The component calls a function that passes in the user input and **dispatching** the changes to `Actions` which **commits** the changes to `mutations` which updates the store:

![Screenshot from 2021-05-21 13-00-12](https://user-images.githubusercontent.com/73107656/119134240-ce787a80-ba34-11eb-8f5d-f9bf8f64ef09.png)

## Using data within components

**Computed properties** are used within components to pull the data in via the store and use within the template.

## Store Modules

Data stores for different parts of an application can be split into modules and pulled into main `store.js`:

![Screenshot from 2021-05-21 13-07-22](https://user-images.githubusercontent.com/73107656/119134787-7db55180-ba35-11eb-9ecf-f01dccaf2537.png)
