<template>
  <div class="container">
    <h1>Unggah Resep Baru</h1>
    <form @submit.prevent="uploadRecipe">
      <div>
        <label for="title">Judul:</label>
        <input type="text" id="title" v-model="recipe.title">
      </div>
      <div>
        <label for="description">Deskripsi:</label>
        <textarea id="description" v-model="recipe.description"></textarea>
      </div>
      <div>
        <label for="imageUrl">URL Gambar:</label>
        <input type="text" id="imageUrl" v-model="recipe.imageUrl">
      </div>
      <button type="submit">Unggah</button>
    </form>
  </div>
</template>

<script>
import { db } from "~/plugins/firebase.js"; // Import instance Firestore dari plugin Firebase

export default {
  data() {
    return {
      recipe: {
        title: "",
        description: "",
        imageUrl: "",
      },
    };
  },
  methods: {
    async uploadRecipe() {
      try {
        const docRef = await db.collection("recipes").add({
          title: this.recipe.title,
          description: this.recipe.description,
          imageUrl: this.recipe.imageUrl,
          createdAt: new Date(),
          likes: 0,
          // Tambahkan field lain sesuai kebutuhan aplikasi Anda
        });
        
        console.log("Resep berhasil diunggah dengan ID:", docRef.id);
        // Redirect ke halaman detail resep atau ke halaman lain yang sesuai
        this.$router.push("/");
      } catch (error) {
        console.error("Error uploading recipe:", error.message);
        // Tampilkan pesan kesalahan atau berikan respons sesuai kegagalan pengunggahan
      }
    },
  },
};
</script>

<style scoped>
/* CSS khusus untuk halaman ini */
</style>
