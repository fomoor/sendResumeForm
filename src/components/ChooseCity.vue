<template>
  <label class="city-select" v-on:click="changeCity" ref="selectCity">
    <span @click="showCityList = !showCityList" style="white-space: nowrap;">
      <span class="current-city"> {{ currentCityName }} </span>
      <span class="red-arrows material-symbols-outlined"> {{ showCityList ? 'expand_more' : 'expand_less' }} </span>
    </span>
    <div class="city-list" :class="{ 'hidden' : showCityList}" @click="setCurrentCity($event.target, $event.target.id)">
      <p id="msc">Москва</p>
      <p id="ksdr">Краснодар</p>
      <p id="kzn">Казань</p>
      <p id="nnov" class="selected">Нижний Новгород</p>
    </div>
  </label>
</template>

<script>
export default {
  name: 'ChooseCity',
  data () {
    return {
      currentCity: 'nnov',
      currentCityName: 'Нижний Новгород',
      showCityList: true
    }
  },
  created(){ 
    document.addEventListener('click', this.hideCityList)
  },
  destroyed () {
    document.removeEventListener('click', this.hideCityList)
  },
  methods: {
    changeCity() {
      this.$emit('currentCity', this.currentCity);
    },
    hideCityList(e) {
      let el = this.$refs.selectCity;
      let target = e.target;
      if (el !== target && !el.contains(target)){
        this.showCityList = true;
      }
    },
    setCurrentCity(target, id) {
      document.getElementById(this.currentCity).classList.remove("selected");
      target.classList.add("selected");

      this.currentCity = id;
      this.currentCityName = id === 'nnov' ? 'Нижний Новгород' : id === 'msc' ? 'Москва' : id === 'ksdr' ? 'Краснодар' : 'Казань';
      this.showCityList = true;
    }
  }
}
</script>
