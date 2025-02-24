<script setup>
import { ref, onMounted } from "vue";

// Data properties
const name = ref("John Doe");
const link = ref("https://www.google.com");
const status = ref("Active");
const countries = ref([
  "Pakistan",
  "Bangladesh",
  "Afghanistan",
  "Turkey",
  "Azerbaijan",
]);

const newCountry = ref("");
const errorMessage = ref("");
const validCountries = ref([]);

// Function to validate and add a new country
const addNewCountry = () => {
  const isValidName = validateCountryName(newCountry.value);
  if (isValidName) {
    const isRealCountry = validCountries.value.includes(newCountry.value);
    const isDuplicate = countries.value.includes(newCountry.value);

    if (isRealCountry) {
      if (!isDuplicate) {
        countries.value.push(newCountry.value);
        newCountry.value = "";
        errorMessage.value = "";
      } else {
        errorMessage.value = "This country is already in the list.";
      }
    } else {
      errorMessage.value = "This is not a valid country name.";
    }
  } else {
    errorMessage.value = "Country name can only contain letters and spaces.";
  }
};

const deleteCountry = (index) => {
  countries.value.splice(index, 1);
};

// Function to validate the country name (letters and spaces only)
const validateCountryName = (country) => {
  const regex = /^[A-Za-z\s]+$/;
  return regex.test(country.trim());
};

// Fetch the list of valid countries on component mount
onMounted(async () => {
  try {
    const response = await fetch(
      "https://restcountries.com/v3.1/all?fields=name"
    );
    const data = await response.json();
    validCountries.value = data.map((country) => country.name.common);
  } catch (error) {
    console.error("Failed to fetch countries:", error);
  }
});

// Toggle status between 'Active' and 'Inactive'
const updateStatus = () => {
  status.value = status.value === "Active" ? "Inactive" : "Active";
};

// Shuffle the list of countries
const shuffleCountries = () => {
  for (let i = countries.value.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [countries.value[i], countries.value[j]] = [
      countries.value[j],
      countries.value[i],
    ];
  }
};

// Sort the list of countries alphabetically
const sortCountries = () => {
  countries.value.sort();
};
</script>

<template>
  <div class="app-container">
    <h2>{{ name }}</h2>
    <p class="status">
      The current status is: <span :class="status">{{ status }}</span>
    </p>
    <a class="google-link" :href="link" target="_blank">Click me to go to Google</a>

    <ul class="countries-list">
      <li v-for="(country, index) in countries" :key="country" class="country-item">
        <span>{{ country }}</span>
        <button @click="deleteCountry(index)" class="delete-button">X</button>
      </li>
    </ul>

    <form @submit.prevent="addNewCountry" class="country-form">
      <label for="country">Add a new country</label>
      <input type="text" id="country" name="country" v-model="newCountry" />
      <button type="submit">Submit</button>
      <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>
    </form>

    <div class="buttons">
      <button @click="updateStatus">Change Status</button>
      <button @click="shuffleCountries">Shuffle Countries</button>
      <button @click="sortCountries">Sort Countries</button>
    </div>
  </div>
</template>

<style scoped>
/* General Styling */
* {
  box-sizing: border-box;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  margin: 0;
  padding: 0;
}

/* Container */
.app-container {
  max-width: 600px;
  margin: 50px auto;
  padding: 20px;
  background: #f1f1f1;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  border-radius: 10px;
  text-align: center;
}

/* Header */
h2 {
  font-size: 2em;
  margin-bottom: 20px;
  color: #2c3e50;
}

/* Status Styling */
.status {
  margin-bottom: 20px;
  font-size: 1.2em;
}

.status span.Active {
  color: #27ae60;
  font-weight: bold;
}

.status span.Inactive {
  color: #e74c3c;
  font-weight: bold;
}

/* Link Styling */
.google-link {
  display: inline-block;
  margin-bottom: 20px;
  color: #2980b9;
  text-decoration: none;
  font-weight: bold;
  transition: color 0.3s;
}

.google-link:hover {
  color: #1b4f72;
}

/* Country List Styling */
.countries-list {
  list-style: none;
  margin-bottom: 20px;
  padding: 0;
}

.country-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #f39c12;
  padding: 10px 15px;
  color: #fff;
  font-weight: bold;
  margin: 5px 0;
  border-radius: 5px;
  transition: transform 0.3s, background 0.3s;
}

.country-item:hover {
  transform: scale(1.05);
  background: #e67e22;
}

.delete-button {
  background: #c0392b;
  border: none;
  color: #fff;
  padding: 5px 10px;
  font-weight: bold;
  border-radius: 5px;
  cursor: pointer;
  transition: background 0.3s;
}

.delete-button:hover {
  background: #a93226;
}

/* Form Styling */
.country-form {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 20px;
}

.country-form label {
  margin-bottom: 8px;
  font-weight: bold;
}

.country-form input {
  padding: 10px;
  border: 2px solid #3498db;
  border-radius: 5px;
  margin-bottom: 10px;
  width: 80%;
  font-size: 1em;
  outline: none;
}

.country-form input:focus {
  border-color: #1abc9c;
}

.country-form button {
  padding: 10px 20px;
  border: none;
  background: #2980b9;
  color: white;
  font-size: 1em;
  cursor: pointer;
  border-radius: 5px;
  transition: background 0.3s, transform 0.3s;
}

.country-form button:hover {
  background: #1b4f72;
  transform: scale(1.05);
}

/* Error Message Styling */
.error-message {
  color: #e74c3c;
  margin-top: 10px;
  font-weight: bold;
}

/* Buttons Styling */
.buttons {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.buttons button {
  padding: 10px 20px;
  margin: 5px 0;
  border: none;
  background: #27ae60;
  color: white;
  font-size: 1em;
  cursor: pointer;
  border-radius: 5px;
  transition: background 0.3s, transform 0.3s;
  width: 80%;
}

.buttons button:hover {
  background: #1d8348;
  transform: scale(1.05);
}
</style>
