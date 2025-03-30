<template>
  <div class="app-container">
    <div class="search-wrapper">
      <h1 class="title">Busca de Cadastros</h1>
      
      <div class="search-bar">
        <input 
          v-model="searchTerm" 
          class="search-input"
          @keyup.enter="searchData"
          placeholder="Digite o termo de busca "
        />
        <button class="search-button" @click="searchData">
          Buscar
        </button>
      </div>

      <div v-if="loading" class="loading-message">
        Carregando...
      </div>

      <div v-if="error" class="error-message">
        {{ error }}
      </div>

      <div v-if="results.length" class="results-container">
        <h2 class="results-title">
          Resultados ({{ results.length }})
        </h2>
        <table class="results-table">
          <thead>
            <tr>
              <th v-for="(value, key) in results[0]" :key="key">
                {{ key }}
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(result, index) in results" :key="index">
              <td v-for="(value, key) in result" :key="key">
                {{ value }}
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';

const searchTerm = ref('');
const results = ref([]);
const loading = ref(false);
const error = ref(null);

const searchData = async () => {
  results.value = [];
  error.value = null;

  if (!searchTerm.value.trim()) {
    error.value = 'Por favor, digite um termo de busca';
    return;
  }

  loading.value = true;

  try {
    const response = await axios.get('http://localhost:5000/buscar', {
      params: { termo: searchTerm.value }
    });
    results.value = response.data;
  } catch (err) {
    error.value = err.response?.data?.erro || 'Erro na busca';
  } finally {
    loading.value = false;
  }
};
</script>

<style scoped>
.app-container {
  min-height: 100vh;
  background-color: #524f4f;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 2rem;
}

.search-wrapper {
  background: white;
  padding: 2rem;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  max-width: 800px;
  width: 100%;
  text-align: center;
}

.title {
  font-size: 1.75rem;
  font-weight: bold;
  margin-bottom: 1.5rem;
  color: #333;
}

.search-bar {
  display: flex;
  gap: 10px;
  margin-bottom: 1rem;
}

.search-input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 1rem;
}

.search-button {
  background: #3b82f6;
  color: white;
  padding: 10px 15px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: 0.3s;
}

.search-button:hover {
  background: #2563eb;
}

.loading-message,
.error-message {
  margin-top: 1rem;
  font-size: 1rem;
}

.error-message {
  color: #b91c1c;
  background: #fee2e2;
  padding: 10px;
  border-radius: 5px;
}

.results-container {
  overflow-x: auto; /* Permite rolagem horizontal */
  max-width: 100%;
}

.results-table {
  width: 100%;
  border-collapse: collapse;
  background: white;
  min-width: 600px; 
}

.results-table th, 
.results-table td {
  border: 1px solid #ddd;
  padding: 10px;
  text-align: left;
  white-space: nowrap; 
  overflow: hidden;
  text-overflow: ellipsis; 
}

.results-table thead {
  background: #f3f4f6;
}

.results-table tbody tr:hover {
  background: #f1f5f9;
}

.results-table tbody tr:hover {
  background: #f1f5f9;
}
</style>
