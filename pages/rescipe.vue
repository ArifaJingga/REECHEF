<template>
    <div>
      <h1>Resep</h1>
      <div v-if="loading">Loading...</div>
      <div v-else>
        <div v-for="recipe in recipes" :key="recipe.id" class="recipe-card">
          <h2>{{ recipe.title }}</h2>
          <p><strong>Ingredients:</strong> {{ recipe.ingredients.join(', ') }}</p>
          <p><strong>Steps:</strong> {{ recipe.steps.join(', ') }}</p>
          <p><strong>Author:</strong> {{ recipe.author }}</p>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { db } from '~/plugins/firebase.js'
  
  export default {
    data() {
      return {
        recipes: [],
        loading: true
      }
    },
    async fetch() {
      try {
        const snapshot = await db.collection('recipes').get()
        this.recipes = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }))
      } catch (error) {
        console.error("Error fetching recipes:", error)
      } finally {
        this.loading = false
      }
    }
  }
  </script>
  
  <style>
  .recipe-card {
    border: 1px solid #ccc;
    padding: 16px;
    margin-bottom: 16px;
    border-radius: 8px;
  }
  </style>
  