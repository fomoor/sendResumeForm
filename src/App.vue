<template>
  <div id="app">
    <div class="container">
      <p class="h1">Отправьте ваше резюме</p>
      <p class="h2">Постройте своё будущее с РусЭлитТрейдинг в городе <ChooseCity v-on:currentCity="newAdress($event)"/></p>
      <div class="main">
        <ResumeForm/>
        <AdressBlock v-bind:currentCity="currentCity" />
      </div>
    </div>
  </div>
</template>

<script>
import './css/styles.css'

import ChooseCity from './components/ChooseCity'
import ResumeForm from './components/ResumeForm'
import AdressBlock from './components/AdressBlock'

export default {
  name: 'App',
  components: {
    ChooseCity,
    ResumeForm,
    AdressBlock
  },
  data () {
    return {
      currentCity: 'nnov'
    }
  },
  methods: {
    newAdress (currentCity) {
      this.currentCity = currentCity
    }
  }
}
</script>
