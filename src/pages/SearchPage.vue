<template>
  <div class="container">
    <b-form @submit.prevent="onSearch" @reset.prevent="onReset"></b-form>
      <h1 class="title">Search Page</h1>
      <b-form-group
        id="input-group-query"
        label-cols-sm="3"
        label="query:"
        label-for="query"
      >
        <b-form-input
          id="query"
          v-model="$v.form.query.$model"
          type="text"
        ></b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.query.required">
          query is required in order to search
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-if="!$v.form.query.alpha">
          Username alpha
        </b-form-invalid-feedback>
      </b-form-group>


        <b-form-group
          id="input-group-number-result_number"
          label-cols-sm="3"
          label="number of recipes:"
          label-for="result_number"
        >
          <b-form-select
            id="result_number"
            v-model="$v.form.result_number.$model"
            :options="result_options"
            :state="validateState('result_number')"
          ></b-form-select>
          <b-form-invalid-feedback v-if="!$v.form.result_number.numeric">
            Got to be a number
          </b-form-invalid-feedback>
          <b-form-invalid-feedback>
            number of recipes is required
          </b-form-invalid-feedback>
        </b-form-group>


        <b-form-group
          id="input-group-cuisine"
          label-cols-sm="3"
          label="cuisine:"
          label-for="cuisine"
        >
          <b-form-select
            id="cuisine"
            v-model="$v.form.cuisine.$model"
            :options="cuisines"
            :state="validateState('cuisine')"
          ></b-form-select>
        </b-form-group>



          <b-form-group
          id="input-group-Searchdiet"
          label-cols-sm="3"
          label="Diet:"
          label-for="Searchdiet"
        >
          <b-form-select
            id="Searchdiet"
            v-model="$v.form.Searchdiet.$model"
            :options="Diets"
            :state="validateState('Searchdiet')"
          ></b-form-select>

        </b-form-group>

        <b-form-group
          id="input-group-number-MySort"
          label-cols-sm="3"
          label="MySort:"
          label-for="MySort"
        >
          <b-form-select
            id="MySort"
            v-model="$v.form.MySort.$model"
            :options="sortings"
            :state="validateState('MySort')"
          ></b-form-select>
        </b-form-group>

        <b-form-group
          id="input-group-number-intolerance"
          label-cols-sm="3"
          label="intolerance:"
          label-for="intolerance"
        >
          <b-form-select
            id="intolerance"
            v-model="$v.form.intolerance.$model"
            :options="Intolerances"
            :state="validateState('intolerance')"
          ></b-form-select>
        </b-form-group>
        
        
        
        <b-button type="reset" variant="danger">Reset</b-button>
        <b-button
          type="submit"
          variant="primary"
          style="width:250px;"
          class="ml-5 w-75"
          >Search</b-button
        >
        <div class="mt-2">
          You have an account already?
          <router-link to="login"> Log in here</router-link>
        </div>
      <b-alert
        class="mt-2"
        v-if="form.submitError"
        variant="warning"
        dismissible
        show
      >
        Search failed: {{ form.submitError }}
      </b-alert>
      <!-- <b-card class="mt-3 md-3" header="Form Data Result">
        <pre class="m-0"><strong>form:</strong> {{ form }}</pre>
        <pre class="m-0"><strong>$v.form:</strong> {{ $v.form }}</pre>
      </b-card> -->

      <b-container>
        <h3>
          <slot></slot>
        </h3>
        <b-row>
          <b-col v-for="r in recipes" :key="r.id">
            <RecipePreview class="recipePreview" :recipe="r" />
          </b-col>
        </b-row>
      </b-container>



  </div>
</template>

<script>
import RecipePreview from "../components/RecipePreview.vue";
import { defineComponent } from '@vue/composition-api'
import {
  required,
  minLength,
  maxLength,
  alpha,
  sameAs,
  numeric,
  email
} from "vuelidate/lib/validators";
import cuisines from "../assets/cuisines";
import sortings from "../assets/sortings";
import Diets from "../assets/Diets";
import Intolerances from "../assets/Intolerances";

export default defineComponent({
  name: "Search",
  components: {
    RecipePreview
  },
  data() {
    return {
      recipes: [],
      form: {
        query: "",
        result_number: "5",
        SearchType: "",
        cuisine: "",
        MySort: "",
        intolerance: "",
        Searchdiet: ""
      },
      result_options: ["5","10","15"],
      types: ['title', ],
      cuisines: [{ value: null, text: "", disabled: true }],
      sortings:  [{ value: null, text: "", disabled: true }],
      Diets: [{ value: null, text: "", disabled: true }],
      Intolerances: [{ value: null, text: "", disabled: true }],
      errors: [],
      validated: false
    };
  },
  validations: {
    form: {
      query: {
        required,
        alpha
      },
      result_number: {
        required,
        numeric
      },
      SearchType:{
        required,
        alpha
      },
      cuisine: {},
      Searchdiet: {},
      MySort: {},
      intolerance: {}
    }
  },
  mounted() {
    // console.log("mounted");
    this.cuisines.push(...cuisines);
    this.sortings.push(...sortings);
    this.Diets.push(...Diets);
    this.Intolerances.push(...Intolerances)
  },
  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },
    async Search() {
      try {
        const response = await this.axios.get(
          // "https://test-for-3-2.herokuapp.com/user/Register",
          "http://localhost:3000" + "/recipes/search" + "/" + this.query+ "/" + this.result_number+ "/"+  this.MySort+ "/"+ this.cuisine+ "/" +this.intolerance+ "/" +this.Searchdiet 

          
        );
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        // console.log(response);
      } catch (err) {
        console.log(error);
      }
    },
    onSearch() {
      // console.log("register method called");
      this.$v.form.$touch();
      if (this.$v.form.$anyError) {
        return;
      }
      // console.log("register method go");
      this.Search();
    },
    onReset() {
      this.form = {
        query: "",
        result_number: "5",
        SearchType: "",
        cuisine: "",
        MySort: "",
        intolerance: "",
        Searchdiet: ""
      };
      this.$nextTick(() => {
        this.$v.$reset();
      });
    }
  }
})
</script>

