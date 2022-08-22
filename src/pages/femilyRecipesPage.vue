<template>
  <b-container>
    <h3>
      <slot></slot>
    </h3>
    <b-row>
      <b-col v-for="r in recipes" :key="r.id">
        <femilyrecipe class="femilyrecipe" :recipe="r" />
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import femilyrecipe from "../components/femilyrecipe.vue";
export default {
  name: "femilyRecipesPage",
  components: {
    femilyrecipe
  },
  props: {
    title: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      recipes: [
            {
                "title": "Sabich",
                "madeBy": "Grandma Telma",
                "when": "friday morning",
                "readyInMinutes": 60,
                "aggregateLikes": 400,
                "servings": 1,
                "vegetarian": true,
                "vegan": false,
                "glutenFree": false,
                "Ingredients": "eggplant, eggs, potato, tomato, onions",
                "instructions": "bil an egg, chop salad, boil potatoes",
                "image": "src\images\sabich.jpeg"
            },
            {
                "title": "sheep",
                "madeBy": "Grandma Louiza",
                "when": "passover",
                "readyInMinutes": 480,
                "aggregateLikes": 800,
                "servings": 15,
                "vegetarian": false,
                "vegan": false,
                "glutenFree": false,
                "Ingredients": "sheep",
                "instructions": "put in the oven for 3 hours",
                "image": "src\images\sheep.jpeg"
            },
            {
                "title": "cheesecake",
                "madeBy": "Grandma Louiza",
                "when": "every week",
                "readyInMinutes": 40,
                "aggregateLikes": 60,
                "servings": 15,
                "vegetarian": true,
                "vegan": false,
                "glutenFree": false,
                "Ingredients": "cream cheese, eggs, sugar, sour cream, vanilla & salt",
                "instructions": "mix all the ingredients in a big bowl, then move it to baking dish then in the oven at 100 degrees for 25 minutes",
                "image": "src\images\sheep.jpeg"
            }  
        ]
    };
  },
  mounted() {
    this.updateRecipes();
  }
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}
</style>
