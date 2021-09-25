<template>
  <Header />
  <Search @searchedMeals="searchedMeals($event)" @selectedMeals="selectedMeals($event)" @selectedAreas="selectedAreas($event)"/>
  <div class="container-md d-flex flex-wrap justify-content-center my-5">
    <Meals :meals="meals" />
  </div>
</template>

<script>

import Header from './components/Header.vue'
import Search from './components/Search.vue'
import Meals from './components/Meals.vue'

export default {
  name: 'App',
  components: { Header, Search, Meals },
  data(){
    return{
      meals: []
    }
  },
  methods:{
    async getMeals(){
      const res = await fetch(`https://www.themealdb.com/api/json/v1/1/filter.php?c=Seafood`)
      const data = await res.json()

      this.meals = data.meals
    },
    searchedMeals(meal){
      this.meals = meal
    },
    selectedMeals(meal){
      this.meals = meal
    },
    selectedAreas(meal){
      this.meals = meal
    }
  },
  created(){
    this.getMeals()
  }
}
</script>

<style>
</style>
