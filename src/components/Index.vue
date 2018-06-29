<template>
  <div class="row ">
      <div class="card col l4 m6 s12 hoverable center" v-for="smoothie in smoothies" :key="smoothie.id">
        <div class="card-content">
          <router-link
          title="Edit Smoothie" 
          :to='{name: "EditSmoothie", params: {smoothie_slug: smoothie.slug}}'
          class="btn-floating halfway-fab">
            <i class="material-icons">edit</i>
          </router-link>
          <a href="#" class="material-icons right" title="Remove Smoothie" @click="deleteCrap(smoothie.id)">delete</a>
          <span class="card-title brown-text">{{smoothie.title}}</span>
          <span class="chip" v-for="(ingreedient, i) in smoothie.ingredients" :key="i">{{ingreedient}}</span>
        </div>
      </div>
  </div>
</template>

<script>
import db from "@/firebase/init";

export default {
  name: "Index",
  data() {
    return {
      smoothies: []
    };
  },
  created() {
    db
      .collection("smoothies")
      .get()
      .then(snapshot => {
        snapshot.forEach(item => {
          let smoothie = item.data();
          smoothie.id = item.id;
          this.smoothies.push(smoothie);
        });
      });
  },
  methods: {
    deleteCrap(id) {
      db
        .collection("smoothies")
        .doc(id)
        .delete()
        .then(() => {
          this.smoothies = this.smoothies.filter(smuti => smuti.id !== id);
        });
    }
  }
};
</script>
