<!-- J'ai demandé un peu d'aide à ChatGPT pour les classes bootstrap -->
<script setup lang="ts">
import { ref, watch } from "vue";
import type { Product } from "../scripts/types";

const props = defineProps<{
  productToEdit: Product | null;
  mode: string;
}>();

const emit = defineEmits(["save"]);

const supplement = ref<Product>({
  id: 0,
  name: "",
  price: 0,
  stockQuantity: 0,
  description: "",
  flavor: "",
});

const errors = ref({
  name: "",
  price: "",
  stockQuantity: "",
  description: "",
  flavor: "",
});

watch(
  () => props.productToEdit,
  (newProduct) => {
    if (newProduct) {
      supplement.value = { ...newProduct };
    } else {
      supplement.value = {
        id: 0,
        name: "",
        price: 0,
        stockQuantity: 0,
        description: "",
        flavor: "",
      };
    }
  },
  { immediate: true }
);

const validateForm = () => {
  errors.value = {
    name: supplement.value.name.trim()
      ? ""
      : "Le nom du supplément est requis.",
    price: supplement.value.price > 0 ? "" : "Le prix doit être supérieur à 0.",
    stockQuantity:
      supplement.value.stockQuantity < 0
        ? "La quantité ne peut pas être négative."
        : "",
    description: supplement.value.description.trim()
      ? ""
      : "La description est requise.",
    flavor: supplement.value.flavor.trim()
      ? ""
      : "La saveur du supplément est requise.",
  };

  return !Object.values(errors.value).some((error) => error !== "");
};

const handleSubmit = () => {
  if (!validateForm()) return;

  emit("save", { ...supplement.value });

  if (props.mode === "add" || props.mode === "duplicate") {
    supplement.value = {
      id: 0,
      name: "",
      price: 0,
      stockQuantity: 0,
      description: "",
      flavor: "",
    };
  }
};
</script>

<template>
  <div class="container mt-4 mb-5">
    <h2>
      {{
        props.mode === "edit"
          ? "Modifier un supplément"
          : props.mode === "duplicate"
          ? "Dupliquer un supplément"
          : "Ajouter un nouveau supplément"
      }}
    </h2>
    <form @submit.prevent="handleSubmit" novalidate class="border p-4 rounded">
      <div class="mb-3">
        <label class="form-label">Nom du supplément</label>
        <input
          v-model="supplement.name"
          type="text"
          class="form-control"
          :class="{ 'is-invalid': errors.name }"
        />
        <div v-if="errors.name" class="invalid-feedback">{{ errors.name }}</div>
      </div>
      <div class="mb-3">
        <label class="form-label">Prix ($)</label>
        <input
          v-model.number="supplement.price"
          type="number"
          class="form-control"
          :class="{ 'is-invalid': errors.price }"
          min="0.01"
          step="0.01"
          placeholder="29.99"
        />
        <div v-if="errors.price" class="invalid-feedback">
          {{ errors.price }}
        </div>
      </div>
      <div class="mb-3">
        <label class="form-label">Quantité en stock</label>
        <input
          v-model.number="supplement.stockQuantity"
          type="number"
          class="form-control"
          :class="{ 'is-invalid': errors.stockQuantity }"
          min="0"
          placeholder="10"
        />
        <div v-if="errors.stockQuantity" class="invalid-feedback">
          {{ errors.stockQuantity }}
        </div>
      </div>
      <div class="mb-3">
        <label class="form-label">Saveur</label>
        <input
          v-model="supplement.flavor"
          type="text"
          class="form-control"
          :class="{ 'is-invalid': errors.flavor }"
        />
        <div v-if="errors.flavor" class="invalid-feedback">
          {{ errors.flavor }}
        </div>
      </div>
      <div class="mb-3">
        <label class="form-label">Description</label>
        <textarea
          v-model="supplement.description"
          class="form-control"
          :class="{ 'is-invalid': errors.description }"
          rows="3"
        ></textarea>
        <div v-if="errors.description" class="invalid-feedback">
          {{ errors.description }}
        </div>
      </div>
      <div class="d-flex gap-2">
        <button type="submit" class="btn btn-primary">
          {{
            props.mode === "edit"
              ? "Enregistrer"
              : props.mode === "duplicate"
              ? "Dupliquer"
              : "Ajouter"
          }}
        </button>
      </div>
    </form>
  </div>
</template>
