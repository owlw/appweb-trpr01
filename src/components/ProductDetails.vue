<script setup lang="ts">
import type { Product } from "../scripts/types";

defineProps<{
  product: Product;
}>();

defineEmits<{
  (e: "close"): void;
}>();
</script>

<template>
  <!-- J'ai demandé de l'aide à ChatGPT un peu pour html et surtout pour l'utilisation des classes bootstrap -->
  <div class="container mt-4">
    <div
      class="product-detail card bg-light text-dark mx-auto"
      style="max-width: 800px"
    >
      <div class="card-header bg-light text-dark">
        <div class="d-flex justify-content-between align-items-center">
          <h3 class="mb-0">Informations du supplément</h3>
          <button
            type="button"
            class="btn btn-sm btn-outline-secondary"
            @click="$emit('close')"
          >
            X
          </button>
        </div>
      </div>
      <div class="card-body">
        <h4>{{ product.name }}</h4>

        <div class="my-3">
          <p><strong>Saveur:</strong> {{ product.flavor }}</p>
          <p><strong>Prix:</strong> {{ product.price }}$</p>
          <p>
            <strong>Disponibilité: </strong>
            <span
              :class="{
                'text-success': product.stockQuantity > 5,
                'text-warning':
                  product.stockQuantity > 0 && product.stockQuantity <= 5,
                'text-danger': product.stockQuantity === 0,
              }"
            >
              {{ product.stockQuantity }} en stock
            </span>
          </p>
        </div>

        <div class="card-text">
          <h5>Description</h5>
          <p>{{ product.description }}</p>
        </div>
      </div>
    </div>
  </div>
</template>
