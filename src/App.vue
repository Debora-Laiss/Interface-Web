<template>
  <div :class="appContainer">
    <div :class="searchWrapper">
      <h1 :class="title">Busca de Cadastros</h1>
      
      <div :class="searchBar">
        <input 
          v-model="searchTerm" 
          :class="searchInput"
          @keyup.enter="searchData"
          placeholder="Digite o termo de busca" 
        />
        <button :class="searchButton" @click="searchData">
          Buscar
        </button>
      </div>

      <div v-if="loading" :class="loadingMessage">
        Carregando...
      </div>

      <div v-if="error" :class="errorMessage">
        {{ error }}
      </div>

      <div v-if="results.length" class="mt-6">
        <h2 class="text-xl font-semibold mb-4">
          Resultados ({{ results.length }})
        </h2>
        <table :class="resultsTable">
          <thead>
            <tr>
              <th v-for="(value, key) in results[0]" :key="key">
                {{ key }}
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(result, index) in results" :key="index">
              <td 
                v-for="(value, key) in result" 
                :key="key"
              >
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
import { ref } from 'vue'
import axios from 'axios'
import { css } from '@emotion/css'

// Styled with Emotion CSS
const appContainer = css`
  min-height: 100vh;
  background-color: #f4f4f4;
  display: flex;
  justify-content: center;
  padding: 2rem;
`

const searchWrapper = css`
  background-color: white;
  border-radius: 0.5rem;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
  padding: 2rem;
  width: 100%;
  max-width: 800px;
`

const title = css`
  text-align: center;
  font-size: 1.5rem;
  font-weight: bold;
  margin-bottom: 1.5rem;
  color: #333;
`

const searchBar = css`
  display: flex;
  margin-bottom: 1rem;
`

const searchInput = css`
  flex-grow: 1;
  padding: 0.5rem;
  border: 1px solid #ddd;
  border-radius: 0.25rem 0 0 0.25rem;
  &:focus {
    outline: none;
    border-color: #3b82f6;
  }
`

const searchButton = css`
  background-color: #3b82f6;
  color: white;
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 0 0.25rem 0.25rem 0;
  cursor: pointer;
  transition: background-color 0.3s;
  &:hover {
    background-color: #2563eb;
  }
`

const loadingMessage = css`
  text-align: center;
  color: #666;
  margin-top: 1rem;
`

const errorMessage = css`
  background-color: #fee2e2;
  color: #b91c1c;
  padding: 0.75rem;
  border-radius: 0.25rem;
  margin-top: 1rem;
`

const resultsTable = css`
  width: 100%;
  border-collapse: collapse;
  
  thead {
    background-color: #f9fafb;
  }

  th, td {
    border: 1px solid #e5e7eb;
    padding: 0.5rem;
    text-align: left;
  }

  tbody tr:hover {
    background-color: #f3f4f6;
  }
`

// Component Logic
const searchTerm = ref('')
const results = ref([])
const loading = ref(false)
const error = ref(null)

const searchData = async () => {
  results.value = []
  error.value = null
  
  if (!searchTerm.value.trim()) {
    error.value = 'Por favor, digite um termo de busca'
    return
  }

  loading.value = true

  try {
    const response = await axios.get('http://localhost:5000/buscar', {
      params: { termo: searchTerm.value }
    })

    results.value = response.data
  } catch (err) {
    if (err.response) {
      error.value = err.response.data.erro || err.response.data.mensagem || 'Erro na busca'
    } else {
      error.value = 'Erro de conexão com o servidor'
    }
  } finally {
    loading.value = false
  }
}
</script>