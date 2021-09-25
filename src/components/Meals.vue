<template>
  <div class="meal-container m-3" v-for="(meal,index) in meals" :key="index" data-bs-toggle="modal" data-bs-target="#showMealModal" @click="openMealModal(index)">
      <img :src="meal.strMealThumb" :alt="meal.strMeal">
      <h4>{{meal.strMeal}}</h4>
  </div>



  <!-- Meal Modal -->
    <div class="modal fade" id="showMealModal" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
    <div class="modal-dialog modal-dialog-scrollable modal-lg">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="staticBackdropLabel">Meal Details</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                <div class="row">
                    <div class="col-sm-6">
                        <h5 class="text-decoration-underline">Meal Name</h5> 
                        <p>{{mealName}}</p>
                        <h5 class="text-decoration-underline">Area </h5> 
                        <p>{{area}}</p>
                        <h5 class="text-decoration-underline">Category</h5> 
                        <p>{{category}}</p>
                        <p>If it is required for more info, please
                            <strong>
                                <a :href="source" target="_blank" class="link-dark">visit</a>
                            </strong>
                        </p>
                    </div>
                    <div class="col-sm-6">
                        <img :src="image" :alt="mealName" class="img-fluid rounded modal-img">
                    </div>
                </div>
                <div class="row">
                    <div class="col-sm-12">
                        <h4 class="text-decoration-underline">Instructions</h4> 
                        <p>{{instructions}}</p>
                    </div>
                </div>
                <div class="row mt-1">
                    <div class="col-sm-12">
                        <h4 class="text-decoration-underline">Ingredients</h4> 
                        <span class="badge rounded-pill m-1 px-3 py-2 bg-secondary text-capitalize" v-for="(ingredient,index) in ingredients" :key="index">
                            {{ingredient}}
                        </span>
                    </div>
                </div>
                <div class="row mt-4">
                    <div class="col-sm-12">
                        <h4 class="text-decoration-underline mb-3">Recipe Video</h4> 
                        <iframe class="w-100" height="350" allowfullscreen = 'true'
                        :src="videoURL">
                        </iframe>
                    </div>
                </div>

            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-outline-secondary" data-bs-dismiss="modal">Close</button>
            </div>
        </div>
    </div>
    </div>
</template>

<script>
export default {
    name: "Meals",
    props: ["meals"],
    data(){
        return{
            mealName: "",
            area: "",
            category: "",
            image: "",
            instructions: "",
            source: "",
            ingredients: [],
            video: "",
            videoURL: ""
        }
    },
    methods:{
        async openMealModal(index){
            const res = await fetch(`https://www.themealdb.com/api/json/v1/1/lookup.php?i=${this.meals[index].idMeal}`)
            const data = await res.json()

            this.mealName = data.meals[0].strMeal
            this.area = data.meals[0].strArea
            this.category = data.meals[0].strCategory
            this.image = data.meals[0].strMealThumb
            this.instructions= data.meals[0].strInstructions
            this.source= data.meals[0].strSource
            this.video = data.meals[0].strYoutube
            this.videoURL = `https://www.youtube.com/embed/${this.video.slice(-11)}`
            
  
            for (let i = 1; i <= 20; i++) {
                if (data.meals[0][`strIngredient${i}`]) {
                    this.ingredients.push(
                    `${data.meals[0][`strIngredient${i}`]} - ${data.meals[0][`strMeasure${i}`]}`
                    );
                } else {
                    break;
                }
            }
  
        }
    }
}
</script>

<style>

</style>