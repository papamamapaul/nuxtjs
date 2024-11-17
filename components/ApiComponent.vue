<template>
    <div>
        <h1 class="text-3xl font-bold">
            <button class="btn btn-active btn-secondary">Secondary</button>
    Hello world!
  </h1>
      <div v-if="loading">Lade Daten...</div>
      <div v-else-if="error">Fehler: {{ error }}</div>
      <ul v-else>
        <li v-for="item in data" :key="item.id">{{ item.title }}</li>
      </ul>
    </div>
  </template>
  
  <script>
import { ref, onMounted } from 'vue';

export default {
  setup() {
    const config = useRuntimeConfig()
    const data = ref(null); // Reactive data
    const loading = ref(true);
    const error = ref(null);

    const fetchData = async () => {
      try {
        console.log(config.public.apiUrl)
        const response = await fetch(config.public.apiUrl+"/api/gifts");
        if (!response.ok) {
          throw new Error(`HTTP-Error: ${response.status}`);
        }
    

        const result = await response.json();

    // Access the `data` property of the response
    data.value = result.data; 
      } catch (err) {
        error.value = err.message;
      } finally {
        loading.value = false;
      }
    };

    onMounted(fetchData);

    return {
      data,
      loading,
      error,
    };
  },
};
</script>
  
  <style scoped>
  h1 {
    font-size: 24px;
    margin-bottom: 10px;
  }
  </style>
  