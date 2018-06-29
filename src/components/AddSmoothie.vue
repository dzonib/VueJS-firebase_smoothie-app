<template>
  <div class="container">
    <h2 class="center-align indigo-text">Add New Smoothie Recepie</h2>
    <form @submit="handleForm">
      <div class="input-field">
        <label for="title">Smoothie Title</label>
        <input type="text" id="title" v-model="title"> 
      </div>
      <span class="chip" v-for="(ing, i) in ingredients" :key="i">{{ing}}
        <i class="material-icons close" @click="removeIngredient(i)">close</i>
      </span>
      <div class="input-field">
        <label for="add-ingredient">Add Ingredient</label>
        <input type="text" id="add-ingredient" autocomplete="off" @keydown.tab.prevent="addIngredient" 
        v-model="ingredient">
        <p :class="clas">{{feedback}}</p>
      </div>

      <div class="field center-align">
        <button class="btn pink">Add Smoothie</button>
      </div>
    </form>
  </div>
</template>

<script>
import slugify from "slugify";
import db from "@/firebase/init";

export default {
  name: "AddSmoothie",
  data() {
    return {
      title: null,
      ingredient: null,
      ingredients: [],
      feedback: null,
      clas: null,
      slug: null
    };
  },
  methods: {
    handleForm() {
      if (this.title && this.ingredients.length !== 0) {
        //create a slug
        this.slug = slugify(this.title, {
          replacement: "-",
          remove: /[$*_+~.()'"!-:@]/g,
          lower: true
        });
        console.log(this.slug);

        db
          .collection("smoothies")
          .add({
            title: this.title,
            ingredients: this.ingredients,
            slug: this.slug
          })
          .then(() => {
            this.$router.push({ name: "Index" });
          })
          .catch(err => console.log(err));
      }
    },

    addIngredient() {
      if (this.ingredient) {
        this.ingredients.push(this.ingredient);
        this.clas = "green-text";
        this.feedback = "Ingredient Added! :)";
        this.ingredient = null;
      } else {
        this.clas = "red-text";
        this.feedback =
          "You must enter the ingredient in the imput field above, then press TAB key to add it!";
      }
    },
    removeIngredient(i) {
      this.$delete(this.ingredients, i);
      setTimeout(() => {
        console.log(this.ingredients);
      }, 1000);
    }
  }
};
</script>
