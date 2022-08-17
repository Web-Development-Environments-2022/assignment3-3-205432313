<template>
  <b-container>
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
    <b-row>
      <b-col v-for="r in recipes" :key="r.id">
        <FavoritesPreview class="FavoritesPreview" :recipe="r" />
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import FavoritesPreview from "../components/FavoritesPreview.vue";
export default {
  name: "FavroritesPage",
  components: {
    FavoritesPreview
  },
  props: {
    title: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      recipes: []
    // {
    //     "id": 638808,
    //     "title": "Chocolate Almond Bark",
    //     "readyInMinutes": 45,
    //     "aggregateLikes": 34,
    //     "vegetarian": false,
    //     "vegan": false,
    //     "glutenFree": true,
    //     "image": "https://spoonacular.com/recipeImages/638808-556x370.jpg"
    // },
    // {
    //     "id": 665779,
    //     "title": "Zucchini Ribbon and Ricotta Pizza",
    //     "readyInMinutes": 45,
    //     "aggregateLikes": 6,
    //     "vegetarian": false,
    //     "vegan": false,
    //     "glutenFree": false,
    //     "image": "https://spoonacular.com/recipeImages/665779-556x370.jpg"
    // },
    // {
    //     "id": 637624,
    //     "title": "Cheesecake Ice-Cream With Mango Syrup",
    //     "readyInMinutes": 45,
    //     "aggregateLikes": 18,
    //     "vegetarian": true,
    //     "vegan": false,
    //     "glutenFree": true,
    //     "image": "https://spoonacular.com/recipeImages/637624-556x370.jpg"
    // }  
    //   ]
    };
  },
  mounted() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      try {
        const response = await this.axios.get(
          // this.$root.store.server_domain + "/recipes/random",
          "http://localhost:3000" + "/favorites",
          // "https://test-for-3-2.herokuapp.com/recipes/random"
        );

        // console.log(response);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        // console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}
</style>
