<template>
    <div class="row container-md d-flex justify-content-between mt-3 mx-auto">
        <form class="col-lg-6 searchForm" @keyup="searchMeal">
            <i class="fas fa-search"></i>
            <input type="text" placeholder="Search any meals..." v-model="search">
        </form>
        <form class="col-lg-3 text-center mt-lg-0 mt-4">
            <select v-model="select_Meal" @change="selectCategory()">
                <option value="" disabled>Select Category</option>
                <option :value="category.strCategory" v-for="(category,index) in categories" :key="index">
                    {{category.strCategory}}
                </option>
            </select>
        </form>
        <form class="col-lg-3 text-center mt-lg-0 mt-4">
            <select v-model="select_Area" @change="selectArea()">
                <option value="" disabled>Select Area</option>
                <option :value="area.strArea" v-for="(area,index) in areas" :key="index">
                    {{area.strArea}}
                </option>
            </select>
        </form>
    </div>
</template>

<script>
export default {
    name: "Search",
    data(){
        return{
            search: "",
            searchedMeals: [],
            select_Meal: "",
            select_Area: "",
            categories: [],
            selectedMeals: [],
            areas: [],
            selectedAreas: []
        }
    },
    methods:{
        async searchMeal(){
            this.select_Area = ""
            this.select_Meal = ""
            const res = await fetch(`https://www.themealdb.com/api/json/v1/1/search.php?s=${this.search}`)
            const data = await res.json()

            this.searchedMeals = data.meals

            this.$emit('searchedMeals', this.searchedMeals)
        },
        async getCategory(){
            const res = await fetch(`https://www.themealdb.com/api/json/v1/1/categories.php`)
            const data = await res.json()

            this.categories = data.categories
        },
        async getArea(){
            const res = await fetch(`https://www.themealdb.com/api/json/v1/1/list.php?a=list`)
            const data = await res.json()

            this.areas = data.meals
        },
        async selectCategory(){
            this.search = ""
            this.select_Area = ""
            const res = await fetch(`https://www.themealdb.com/api/json/v1/1/filter.php?c=${this.select_Meal}`)
            const data = await res.json()

            this.selectedMeals = data.meals
            
            this.$emit('selectedMeals', this.selectedMeals)
        },
        async selectArea(){
            this.search = ""
            this.select_Meal = ""
            const res = await fetch(`https://www.themealdb.com/api/json/v1/1/filter.php?a=${this.select_Area}`)
            const data = await res.json()

            this.selectedAreas = data.meals
            
            this.$emit('selectedAreas', this.selectedAreas)
        }
    },
    created(){
        this.getCategory()
        this.getArea()
    }
}
</script>

<style>

</style>