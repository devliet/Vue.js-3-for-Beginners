
<template>
  <div>
    <ul>
      <li v-for="item in items.posts" :key="item.userId">{{ item.reactions }}</li>
    </ul>
    <p v-if="loading">Loading...</p>
    <p v-if="error">{{ error }}</p>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';

export default {
  setup() {
    const items = ref([]);
    const loading = ref(true);
    const error = ref(null);

    const fields1 = ref([]);
  const fields2 =  ref([]);
  const fields3 =ref([]);






  const fetchData = async () => {
      try {
        const response = await fetch('https://dummyjson.com/posts?limit=5&select=userId,body,reactions,views');

        const [fields1Response, fields2Response, fields3Response] = await Promise.all([
        fetch('https://dummyjson.com/posts?limit=5&select=userId,body,reactions,views'),
        fetch('https://randomuser.me/api/?inc=login,picture&results=5'),
        fetch('https://dummyjson.com/comments?limit=5&select=body'),
      ]);

        if (!fields1Response.ok || !fields2Response.ok || !fields3Response.ok) {
          throw new Error('Network response was not ok');
        }
     //   const data = await response.json();
      const fields1 = await fields1Response.json();
      const fields2= await fields2Response.json();
      const fields3= await fields3Response.json();

      fields1.value =fields1;
      fields2.value = fields2;
      fields3.value =fields3;


     //   items.value = data;
      } catch (err) {
        error.value = err.message;
      } finally {
        loading.value = false;
      }
    };


    const fetchData = async () => {
      try {
        const response = await fetch('https://dummyjson.com/posts?limit=5&select=userId,body,reactions,views');
        if (!response.ok) {
          throw new Error('Network response was not ok');
        }
        const data = await response.json();
        items.value = data;
      } catch (err) {
        error.value = err.message;
      } finally {
        loading.value = false;
      }
    };

    onMounted(fetchData);

    return {
      items,
      loading,
      error,
    };
  },
};
</script>

<style scoped>
/* Add your styles here */
</style>

Explanation

Template:

The v-for directive iterates over the items array and renders each item.
Conditional rendering is used to display loading and error messages.

Script:

ref is used to create reactive variables for items, loading, and error.
fetchData is an asynchronous function that fetches data from an API and updates the items array.
onMounted ensures that fetchData is called when the component is mounted.

This approach ensures that your Vue component handles asynchronous data fetching gracefully and renders the data efficiently. If you have any more questions or need further assistance, feel free to ask!