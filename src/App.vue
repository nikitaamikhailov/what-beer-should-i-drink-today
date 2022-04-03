<template>
  <div class="bg">
    <div class="container">
      <div class="header">
        <div class="profile">
          <img class="avatar" :src="user.avatar" alt="avatar">
          <div class="info">
            <p class="text"><b>Name:</b> {{ user.first_name }} {{ user.last_name }}</p>
            <p class="text"><b>Age:</b> {{ getAge() }}</p>
            <p class="text"><b>Employment:</b> {{ user.employment.title }}</p>
          </div>
        </div>
        <button class="text button" @click="getBeer()">{{ buttonText }}</button>
      </div>
      <div v-if="beer" class="description">
        <p class="text">Today I will advise you beer "{{ beer.name }}" brand "{{ beer.brand }}"</p>
      </div>
    </div>
  </div>
</template>

<script>
import "./main.css"

export default {
  name: 'App',
  data() {
    return {
      user: null,
      beer: null,
      buttonText: 'What beer should i drink today?',
    }
  },
  beforeCreate() {
    fetch('https://random-data-api.com/api/users/random_user')
        .then((response) => {
          return response.json();
        })
        .then((data) => {
          this.user = data
        })
  },
  methods: {
    async getBeer() {
      await fetch('https://random-data-api.com/api/beer/random_beer')
          .then((response) => {
            return response.json();
          })
          .then((data) => {
            this.beer = data;
            this.buttonText = 'another beer?';
          })
    },
    getAge() {
      let date = this.user.date_of_birth.split('-')
      date = Math.floor((new Date().getTime() - new Date(date)) / (24 * 3600 * 365.25 * 1000))
      return date
    }
  }
}
</script>
