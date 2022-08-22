<template>
  <div class="container">
    <h1 class="title">Main Page</h1>
    <RecipePreviewList title="Randome Recipes" class="RandomRecipes center" />
    <router-link v-if="IsLogged" to="/login" tag="button">You need to Login to vue this</router-link>
    <!-- {{ !$root.store.username }} -->
    <div v-else>

      </div><RecipePreviewListLastSeen
        title="Last Viewed Recipes"
        :class="{
          RandomRecipes: true,
          blur: !$root.store.username,
          center: true
        }"
        disabled
      ></RecipePreviewListLastSeen>
    </div>
    <!-- <div
      style="position: absolute;top: 70%;left: 50%;transform: translate(-50%, -50%);"
    >
      Centeredasdasdad
    </div>-->
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";
import RecipePreviewListLastSeen from "../components/RecipePreviewListLastSeen";
export default {
  components: {
    RecipePreviewList,
    RecipePreviewListLastSeen
  },
  methods: {
    IsLogged(){
      console.log(this.$root.store)
      return this.$root.store.login !==undefined
    }
  }
  
};
</script>

<style lang="scss" scoped>
.RandomRecipes {
  margin: 10px 0 10px;
}
.blur {
  -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  filter: blur(2px);
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}
</style>
