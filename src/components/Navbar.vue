<template>
  <nav class="indigo darken-1">
      <div class="nav-wrapper">
        <div class="container">
          <router-link  :to="{name: 'AddSmoothie'}" class="btn-floating btn-large halfway-fab waves-effect waves-light pink darken-1 ">
            <i class="material-icons">add</i>
          </router-link>
          <router-link class="brand-logo" :to="{name: 'Index'}">Ninja Smoothies</router-link>
        </div>  
      </div>
  </nav>
</template>

<script>
export default {
  name: "Navbar"
};
</script>
