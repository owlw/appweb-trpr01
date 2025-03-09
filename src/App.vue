<script setup lang="ts">
import "bootstrap/dist/css/bootstrap.min.css";
import "bootstrap";
import { ref } from "vue";
import type { Product } from "./scripts/types";
import StoreHeader from "./components/Header.vue";
import StoreFooter from "./components/Footer.vue";
import ProductList from "./components/ProductList.vue";
import ProductSearch from "./components/ProductSearch.vue";
import ProductAlert from "./components/ProductAlert.vue";
import ProductForm from "./components/ProductForm.vue";
import ProductDetails from "./components/ProductDetails.vue";

const products = ref<Product[]>([
  {
    id: 1,
    name: "Whey Protéine",
    price: 29.99,
    stockQuantity: 10,
    description: "La meilleure protéine pour prendre du muscle.",
    flavor: "Chocolat",
  },
  {
    id: 2,
    name: "Créatine",
    price: 19.99,
    stockQuantity: 5,
    description: "La meilleure créatine pour prendre du muscle.",
    flavor: "Nature",
  },
  {
    id: 3,
    name: "BCAA",
    price: 24.99,
    stockQuantity: 3,
    description: "Acides aminés essentiels pour la récupération musculaire.",
    flavor: "Punch aux fruits",
  },
  {
    id: 4,
    name: "Iso Protéine",
    price: 34.99,
    stockQuantity: 0,
    description:
      "La meilleure protéine pour prendre du muscle et rester en forme.",
    flavor: "Vanille",
  },
  {
    id: 5,
    name: "Pre-Workout",
    price: 27.99,
    stockQuantity: 8,
    description: "Boostez votre énergie avant l'entraînement.",
    flavor: "Fraise",
  },
]);

const showForm = ref(false);
const productToEdit = ref<Product | null>(null);
const formMode = ref("add");
const selectedProduct = ref<Product | null>(null);
const searchQuery = ref("");

const filteredProducts = () => {
  if (!searchQuery.value.trim()) {
    return products.value;
  }

  const searchLower = searchQuery.value.toLowerCase();
  return products.value.filter(
    (product) =>
      product.name.toLowerCase().includes(searchLower) ||
      product.description.toLowerCase().includes(searchLower) ||
      product.flavor.toLowerCase().includes(searchLower)
  );
};

const handleSearch = (query: string) => {
  searchQuery.value = query;
};

const addProduct = (product: Product) => {
  let nextId = 1;
  if (products.value.length > 0) {
    nextId = products.value[products.value.length - 1].id + 1;
  }

  product.id = nextId;
  products.value.push(product);
};

const editProduct = (product: Product) => {
  const index = products.value.findIndex((p) => p.id === product.id);
  if (index !== -1) {
    products.value[index] = product;
  }
};

const deleteProduct = (id: number) => {
  const userConfirmed = confirm(
    "Êtes-vous sûr de vouloir supprimer ce produit?"
  );

  if (userConfirmed) {
    const productToDelete = products.value.find((product) => product.id === id);
    if (productToDelete) {
      const productIndex = products.value.findIndex(
        (product) => product.id === id
      );
      products.value.splice(productIndex, 1);
    }
  }
};

const duplicateProduct = (product: Product) => {
  productToEdit.value = { ...product, id: 0 };
  formMode.value = "duplicate";
  showForm.value = true;
};

const handleSave = (product: Product) => {
  if (formMode.value === "edit") {
    editProduct(product);
  } else {
    addProduct(product);
  }
  closeForm();
};

const closeForm = () => {
  showForm.value = false;
  productToEdit.value = null;
};

const handleEdit = (product: Product) => {
  productToEdit.value = product;
  formMode.value = "edit";
  showForm.value = true;
};

const toggleForm = () => {
  if (showForm.value) {
    showForm.value = false;
    productToEdit.value = null;
  } else {
    showForm.value = true;
    formMode.value = "add";
    productToEdit.value = null;
  }
};
</script>

<template>
  <StoreHeader />
  <ProductAlert :products="products" />

  <ProductSearch @search="handleSearch" />
  <div class="text-center mt-3 mb-4">
    <!-- J'ai demandé un peu d'aide à ChatGPT pour le toggle ici -->
    <button class="btn btn-success" @click="toggleForm">
      {{ showForm ? "Fermer le formulaire" : "Ajouter un supplément" }}
    </button>
  </div>
  <ProductForm
    v-if="showForm"
    :productToEdit="productToEdit"
    :mode="formMode"
    @save="handleSave"
    @cancel="closeForm"
  />
  <ProductDetails
    v-if="selectedProduct"
    :product="selectedProduct"
    @close="selectedProduct = null"
  />
  <ProductList
    :products="filteredProducts()"
    @edit="handleEdit"
    @duplicate="duplicateProduct"
    @delete="deleteProduct"
    @view="(product) => (selectedProduct = product)"
  />
  <StoreFooter />
</template>
