
<!-- compositoin api with setup function -->

<script>
import { ref } from "vue";

export default {
  setup() {
    const name = ref("John doe");
    const link = ref("https://www.google.com");
    const status = ref("Active");
    const countries = ref([
      "Pakistan",
      "Bangladesh",
      "Afghanistan",
      "Turkey",
      "Azarbaijan",
    ]);

    const updateStatus = () => {
      if (status.value === "Active") {
        status.value = "Inactive";
      } else if (status.value === "Inactive") {
        status.value = "Active";
      }
    };

    const shuffleCountries = () => {
      for (let i = countries.value.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [countries.value[i], countries.value[j]] = [
          countries.value[j],
          countries.value[i],
        ];
      }
    };

    const sortCountries = () => {
      countries.value.sort();
    };

    return {
      name,
      link,
      status,
      countries,
      updateStatus,
      shuffleCountries,
      sortCountries,
    };
  },
};
</script>

<template>
  <h2>{{ name }}</h2>
  <p>
    the current status is : <span>{{ status }}</span>
  </p>

  <a :href="link">click me to to go google</a>

  <br />

  <ul>
    <li v-for="country in countries" :key="country">{{ country }}</li>
  </ul>

  <button @click="updateStatus">CLick to change status</button>

  <br />

  <button @click="shuffleCountries">click to shuffle countries</button>

  <br />

  <button @click="sortCountries">click to sort the countries</button>
</template>


<style scoped>
</style>
