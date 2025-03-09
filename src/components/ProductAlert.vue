<script setup lang="ts">
import type { Product } from "../scripts/types";

defineProps<{ products: Product[] }>();
</script>

<template>
  <div
    v-if="products.filter((p) => p.stockQuantity === 0).length > 0"
    class="container mt-5 pt-5"
  >
    <div
      v-for="product in products.filter((p) => p.stockQuantity === 0)"
      :key="product.id"
      class="alert alert-danger d-flex align-items-center mb-2"
    >
      Le produit "{{ product.name }}" est en rupture de stock!
    </div>
  </div>
</template>
