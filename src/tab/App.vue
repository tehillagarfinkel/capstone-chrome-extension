<template>
  <div>
    <img src="ON IT logo.png" alt="logo" />
    <h2>My Tasks</h2>

    <div v-for="task in tasks">
      {{ task.description }}
    </div>
    <h2>Joke of the Day:</h2>
    <div>
      <p class="joke">{{ joke }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      loading: true,
      joke: '',
      categories: [],
      tasks: [],
    };
  },
  mounted() {
    var params = { email: 'tehilla@email.com', password: 'password' };
    axios
      .post('http://localhost:3000/api/sessions', params)
      .then(response => {
        console.log('login', response.data);
        axios.defaults.headers.common['Authorization'] = 'Bearer ' + response.data.jwt;
        return axios.get('http://localhost:3000/api/categories');
      })
      .then(response => {
        console.log(response.data);
        this.categories = response.data;
        this.categories.forEach(category => {
          category.tasks.forEach(task => {
            this.tasks.push(task);
          });
        });
        console.log('categories', this.categories);
        console.log('tasks', this.tasks);
      });
    axios.get('https://icanhazdadjoke.com/', { headers: { Accept: 'application/json' } }).then(res => {
      this.joke = res.data.joke;
      this.loading = false;
    });
  },
};
</script>

<style>
body {
  height: 98vh;
  text-align: center;
  color: #353638;
  font-size: 22px;
  line-height: 30px;
  font-family: Merriweather, Georgia, serif;
  background-size: 200px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.joke {
  max-width: 800px;
}
</style>
