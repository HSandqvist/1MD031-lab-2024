<template>
  <div>
    <h3>{{ burger.name }}</h3>
    <img :src="burger.imageUrl" :alt="burger.name">
    <ul>
      <li><span class="ingredient" v-if="!burger.containsGluten">Contains no gluten</span><span class="ingredient" v-else>Contains gluten</span></li>
      <li><span class="ingredient" v-if="!burger.containsLactose">Contains no lactose</span><span class="ingredient" v-else>Contains lactose</span></li>
      <li>{{ burger.kCal }} kCal</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

#burger-selection {
    background-color: black;
    color: white;
    border-color: white;
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); /* Increased the minimum size */
    gap: 20px;
}

#burger-selection div {
    padding: 10px;
    margin: 10px;
}
#burger-selection img {
    width: 100%;
    height: 150px; /* Set a fixed height for the images */
    object-fit: cover; /* Ensure the images maintain their aspect ratio */
}

.ingredient {
    font-weight: bold;
}

</style>