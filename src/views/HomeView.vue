<script>
import axios from 'axios'

export default {
  data: function () {
    return {
      message: "Welcome to Fruits!",
      fruits: [],
      newFruit: {},
      currentFruit: {}
    };
  },
  created: function () {
    this.indexFruits();
  },
  methods: {
    indexFruits: function () {
      console.log('index fruits: ')
      axios.get(`/fruits`).then(response => {
        console.log(response.data);
        this.fruits = response.data;
      });
    },
    createFruit: function () {
      console.log('creating fruit: ');
      axios.post(`/fruits`, this.newFruit).then(response => {
        console.log(response.data);
        this.fruits.push(response.data)
        this.newFruit = {}
      });
    },
    showFruit: function (fruit) {
      console.log('show fruit: ');
      this.currentFruit = fruit;
      document.querySelector("#fruit-details").showModal();
    },
    updateFruit: function () {
      console.log('update fruit: ')
      axios.patch(`/fruits/${this.currentFruit.id}`, this.currentFruit).then(response => {
        console.log(response.data)
      })
    },
    destroyFruit: function () {
      console.log('deleting fruit: ')
      axios.delete(`/fruits/${this.currentFruit.id}`, this.currentFruit).then(response => {
        console.log(response);
        var index = this.fruits.indexOf(this.currentFruit)
        this.fruits.splice(index, 1)
      })
    }
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div>
      <p>
        Name:
        <input v-model="newFruit.name" />
      </p>
      <p>
        Color:
        <input v-model="newFruit.color" />
      </p>
      <p>
        Flavor:
        <input v-model="newFruit.flavor" />
      </p>
      <p>
        Image:
        <input v-model="newFruit.image" />
      </p>
      <button v-on:click="createFruit()">Add new fruit</button>
    </div>
    <dialog id="fruit-details">
      <form method="dialog">
        <h3>Fruit info</h3>
        <p>
          Name:
          <input v-model="currentFruit.name" />
        </p>
        <p>
          Color:
          <input v-model="currentFruit.color" />
        </p>
        <p>
          Flavor:
          <input v-model="currentFruit.flavor" />
        </p>
        <p>
          Image:
          <input v-model="currentFruit.image" />
        </p>
        <h3>Update fruit</h3>
        <p>Name: {{ currentFruit.name }}</p>
        <p>Color: {{ currentFruit.color }}</p>
        <p>Flavor: {{ currentFruit.flavor }}</p>
        <p>Image: {{ currentFruit.image }}</p>
        <button v-on:click="updateFruit()">Update fruit</button>
        <button v-on:click="destroyFruit()">Delete fruit</button>
        <button>Close</button>
      </form>
    </dialog>
    <div v-for="fruit in fruits" v-bind:key="fruit.id">
      <p>{{ fruit.name }}</p>
      <p>
        <img v-bind:src="fruit.image" />
      </p>
      <button v-on:click="showFruit(fruit)">More info</button>
    </div>
  </div>
</template>

<style>
img {
  width: 120px;
}
</style>