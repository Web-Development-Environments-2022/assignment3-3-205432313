<template>
  <div class="container">
    <h1 class="title">Create Youre Recipe</h1>
        <b-form @submit.prevent="onCreation" @reset.prevent="onReset">
            <b-form-group
                id="input-group-title"
                label-cols-sm="3"
                label="title:"
                label-for="title"
            >
                <b-form-input
                id="title"
                v-model="$v.form.title.$model"
                type="text"
                :state="validateState('title')"
                ></b-form-input>
                <b-form-invalid-feedback v-if="!$v.form.title.required">
                    title is required
                </b-form-invalid-feedback>
            </b-form-group>



            <b-form-group
                id="input-group-InMinutes"
                label-cols-sm="3"
                label="How Does it take to make the recipe?"
                label-for="InMinutes"
            >
                <b-form-input
                id="InMinutes"
                v-model="$v.form.InMinutes.$model"
                type="numeric"
                :state="validateState('InMinutes')"
                ></b-form-input>
                <b-form-invalid-feedback v-if="!$v.form.InMinutes.required">
                firstname is required
                </b-form-invalid-feedback>
                <b-form-invalid-feedback v-if="!$v.form.InMinutes.numeric">
                Has to be a number
                </b-form-invalid-feedback>
            </b-form-group>


            <b-form-group
                id="input-group-image"
                label-cols-sm="3"
                label="image:"
                label-for="image"
            >
                <b-form-input
                id="image"
                v-model="$v.form.image.$model"
                type="text"
                :state="validateState('image')"
                ></b-form-input>
                <b-form-invalid-feedback v-if="!$v.form.image.required">
                    image is required
                </b-form-invalid-feedback>
            </b-form-group> 








            <b-form-group
                id="input-group-vegan"
                label-cols-sm="3"
                label="Is it vegan?"
                label-for="vegan"
            >
                <b-form-select
                id="vegan"
                v-model="$v.form.vegan.$model"
                :options="TrueOrFalse"
                :state="validateState('vegan')"
                ></b-form-select>
                <b-form-invalid-feedback>
                    vegan or not is required
                </b-form-invalid-feedback>
            </b-form-group>
        

            <b-form-group
                id="input-group-vegetarian"
                label-cols-sm="3"
                label="Is it vegetarian?"
                label-for="vegetarian"
            >
                <b-form-select
                id="vegetarian"
                v-model="$v.form.vegetarian.$model"
                :options="TrueOrFalse"
                :state="validateState('vegetarian')"
                ></b-form-select>
                <b-form-invalid-feedback>
                    vegetarian or not is required
                </b-form-invalid-feedback>
            </b-form-group>

        

            <b-form-group
                id="input-group-glutenFree"
                label-cols-sm="3"
                label="Is it gluten Free?"
                label-for="glutenFree"
            >
                <b-form-select
                id="glutenFree"
                v-model="$v.form.glutenFree.$model"
                :options="TrueOrFalse"
                :state="validateState('glutenFree')"
                ></b-form-select>
                <b-form-invalid-feedback>
                    gluten Free or not is required
                </b-form-invalid-feedback>
            </b-form-group>



            <b-form-group
                id="input-group-Ingredients"
                label-cols-sm="3"
                label="Ingredients:"
                label-for="Ingredients"
            >
                <b-form-input
                id="Ingredients"
                v-model="$v.form.Ingredients.$model"
                type="text"
                :state="validateState('Ingredients')"
                ></b-form-input>
                <b-form-invalid-feedback v-if="!$v.form.Ingredients.required">
                email is required
                </b-form-invalid-feedback>
            </b-form-group> 

            <b-form-group
                id="input-group-DishesNumber"
                label-cols-sm="3"
                label="Number of diches:"
                label-for="DishesNumber"
            >
                <b-form-input
                id="DishesNumber"
                type="numeric"
                v-model="$v.form.DishesNumber.$model"
                :state="validateState('DishesNumber')"
                ></b-form-input>
                <b-form-invalid-feedback v-if="!$v.form.DishesNumber.required">
                    DishesNumber is required
                </b-form-invalid-feedback>
                <b-form-invalid-feedback v-if="!$v.form.DishesNumber.numeric">
                    Dishes Number should be a number
                </b-form-invalid-feedback>
               

            </b-form-group>


            <b-button type="reset" variant="danger">Reset</b-button>
            <b-button
                type="submit"
                variant="primary"
                style="width:250px;"
                class="ml-5 w-75"
                >save it</b-button
            >
        </b-form>
        <b-alert
        class="mt-2"
        v-if="form.submitError"
        variant="warning"
        dismissible
        show
        >
        Saving failed: {{ form.submitError }}
        </b-alert>
        <!-- <b-card class="mt-3 md-3" header="Form Data Result">
        <pre class="m-0"><strong>form:</strong> {{ form }}</pre>
        <pre class="m-0"><strong>$v.form:</strong> {{ $v.form }}</pre>
        </b-card> -->
  </div>
</template>

<script>
import {
  required,
  minLength,
  maxLength,
  numeric,
  sameAs,
  email
} from "vuelidate/lib/validators";

export default {
  name: "CreateRecipe",
  data() {
    return {
      form: {
        title: "",
        InMinutes: "",
        vegan: "",
        vegetarian: null,
        glutenFree: "",
        Ingredients: "",
        DishesNumber: "",
        image: "",
        submitError: undefined
      },
      TrueOrFalse: [true, false],
      errors: [],
      validated: false
    };
  },
  validations: {
    form: {
      title: {
        required
      },
      InMinutes: {
        required,
        numeric
      },
      image:{
        required
      },
      vegan: {
        required
      },
      vegetarian: {
        required
      },
      glutenFree:{
        required
      },
      Ingredients: {
        required
      },
      DishesNumber:{
        required,
        numeric
      }
    }
  },
  mounted() {
    // console.log("mounted");
    // console.log($v);
  },
  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },
    async CreateRecipe() {
      try {
        const response = await this.axios.post(
          "http://localhost:3000" + "/CreateReceipes",

          {
            title: this.form.title,
            InMinutes: this.form.InMinutes,
            vegan: this.form.vegan,
            vegetarian: this.form.vegetarian,
            glutenFree: this.form.glutenFree,
            Ingredients: this.form.Ingredients,
            DishesNumber: this.form.DishesNumber,
            image: this.form.image
          }
        );
        // console.log(response);
      } catch (err) {
        console.log(err.response);
        this.form.submitError = err.response.data.message;
      }
    },
    onCreation() {
      this.$v.form.$touch();
      if (this.$v.form.$anyError) {
        return;
      }
      this.CreateRecipe();
    },
    onReset() {
      this.form = {
        title: "",
        InMinutes: "",
        vegan: "",
        vegetarian: null,
        glutenFree: "",
        Ingredients: "",
        image: "",
        DishesNumber: ""
      };
      this.$nextTick(() => {
        this.$v.$reset();
      });
    }
  }
};
</script>
<style lang="scss" scoped>
.container {
  max-width: 500px;
}
</style>
