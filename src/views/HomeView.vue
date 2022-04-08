<script>
import axios from 'axios'

export default {
  data: function () {
    return {
      message: "Welcome to Fruits!",
      fruits: [],
      newFruit: {}
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
      <button v-on:click="createFruit()">Add new fruit</button>
    </div>

    <div v-for="fruit in fruits" v-bind:key="fruit.id">
      <p>{{ fruit.name }}</p>
    </div>
  </div>
</template>

<style></style>