<script>
export default {
  data() {
    return {
      name: "John doe",
      link: "https://www.google.com",
      status: "Active",
      countries: [
        "Pakistan",
        "Bangladesh",
        "Afghanistan",
        "Turkey",
        "Azarbaijan",
      ],
    };
  },
  methods: {
    updateStatus() {
      if (this.status === "Active") {
        this.status = "Inactive";
      } else if (this.status === "Inactive") {
        this.status = "Active";
      }
    },
    shuffleCountries() {
      for (let i = this.countries.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [this.countries[i], this.countries[j]] = [this.countries[j], this.countries[i]];
      }
    },
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

  <br>

  <button @click="shuffleCountries" >click to shuffle countries</button>


</template>


<style scoped>
</style>
