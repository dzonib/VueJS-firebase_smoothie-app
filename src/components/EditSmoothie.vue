<template>
  <div v-if="smoothie">
    <h1>Editing "{{smoothie.title}}" smoothie</h1>
    <form @submit="editSmoothie">
      <div>
        <label for="title">Smoothie Title</label>
        <input type="text" id="title" v-model="smoothie.title"> 
      </div>
      <span class="chip" v-for="(ing, i) in smoothie.ingredients" :key="i">{{ing}}
        <i class="material-icons close" @click.prevent="removeIngredient(i)">close</i>
      </span>
      <div class="input-field">
        <label for="add-ingredient">Add Ingredient</label>
        <input type="text" id="add-ingredient" 
        autocomplete="off" 
        @keydown.tab.prevent="addIngredient" 
        v-model="ingredient">
      </div>
        <p :class="clas">{{feedback}}</p>
      <div class="field center-align">
        <button class="btn pink">Update Smoothie</button>
      </div>
    </form>
  </div>
</template>

<script>
import db from "@/firebase/init";
import slugify from 'slugify';

export default {
  name: "EditSmoothie",
  data() {
    return {
      smoothie: null,
      ingredient: null,
      feedback: null,
      clas: null
    };
  },
  watch: {
    $route: function() {
      this.smoothieSlug = this.$route.params.smoothie_slug;
    }
  },
  methods: {

    removeIngredient(i) {
      this.$delete(this.smoothie.ingredients, i)
      console.log(this.smoothie.ingredients)
    },
    addIngredient() {
      if (this.ingredient) {
        this.smoothie.ingredients.push(this.ingredient);
        this.clas = "green-text";
        this.feedback = "Ingredient Added! :)";
        this.ingredient = null;
      } else {
        this.clas = "red-text";
        this.feedback =
          "You must enter the ingredient in the imput field above, then press TAB key to add it!";
      }
    },
    editSmoothie() {
      if (this.smoothie.title && this.smoothie.ingredients.length !== 0) {
        //create a slug
        this.smoothie.slug = slugify(this.smoothie.title, {
          replacement: "-",
          remove: /[$*_+~.()'"!-:@]/g,
          lower: true
        });
        db.collection("smoothies").doc(this.smoothie.id)
          .update({
            title: this.smoothie.title,
            ingredients: this.smoothie.ingredients,
            slug: this.smoothie.slug
          })
          .then(() => {
            this.$router.push({ name: "Index" });
          })
          .catch(err => console.log(err));
      } else {
        this.feedback = "You must enter a smoothie title and ingredients it was made from"
        this.clas = "red-text"
      }
    }
  },
  created() {
    let ref = db
      .collection("smoothies")
      .where("slug", "==", this.$route.params.smoothie_slug);
    ref.get().then(snapshot => {
      snapshot.forEach(item => {
        this.smoothie = item.data();
        this.smoothie.id = item.id;
      });
    });
  }
};
</script>

