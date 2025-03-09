<!-- J'ai demandé un peu d'aide à ChatGPT pour les classes bootstrap ainsi que pour les icônes -->
<script setup lang="ts">
import { ref } from "vue";

const searchInput = ref("");

const emit = defineEmits<{
  (e: "search", searchText: string): void;
}>();

const updateSearch = () => {
  emit("search", searchInput.value);
};
</script>

<template>
  <div class="container my-4 mt-3">
    <div class="input-group">
      <input
        v-model="searchInput"
        type="text"
        class="form-control rounded-3"
        placeholder="Rechercher un supplément..."
        @input="updateSearch"
      />
    </div>
  </div>
</template>
