<template>
  <div id="app" v-bind:class="{'day-background': this.dayOrNot, 'night-background': !this.dayOrNot}">
    <HeaderComponent />
    <div id="site-content">
      <MainComponent v-bind:isItDay="dayOrNot" />
    </div>
    <FooterComponent />
  </div>
</template>

<script>
import HeaderComponent from './components/layout/Header'
import MainComponent from './components/layout/Main'
import FooterComponent from './components/layout/Footer'

export default {
  name: 'App',
  components: {
    HeaderComponent,
    MainComponent,
    FooterComponent
  },
  data() {
    return {
      dayOrNot: this.isItDay()
    }
  },
  methods: {
    // Determine if it is day or night
    isItDay() {
      let hours = new Date().getHours()
      let isItDay = true

      // If hours is between 5 and 20 (Both exclusive) it is day
      if (hours > 5 && hours < 20) {
          isItDay = true
      } else {
          isItDay = false
      }

      return isItDay
    },
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Baloo+Bhaina+2&display=swap');

* {
  margin: 0;
  padding: 0;
}

#app {
  /* font-family: Avenir, Helvetica, Arial, sans-serif; */
  font-family: 'Baloo Bhaina 2', cursive;
  display: flex;
  min-height: 100vh;
  flex-direction: column;
}

#site-content {
  flex: 1;
}

.day-background {
  background-color: #e8f4ff;
  color: #2c3e50;
}

.night-background {
  background-color: #1c1f38;
  color: white;
}
</style>
