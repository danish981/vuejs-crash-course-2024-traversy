# vuejs-crash-course-2024-travery

Learning vuejs 3 by watching brad traversy vuejs 3 2024 crash course

here is the video link
https://youtu.be/VeNfHj6MhgA

#### the topic the currently im working on

deleting a country from the reactive array list

the video link with current timestamp
https://youtu.be/VeNfHj6MhgA?t=2798

**How you can use the fetch**
_Here is how you can use fetch and get result on API response_

```js
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
```
