<!-- J'ai demandé un peu d'aide à ChatGPT pour les classes bootstrap -->
<script setup lang="ts">
import type { Product } from "../scripts/types";

defineProps<{ products: Product[] }>();
defineEmits<{
  (e: "edit", product: Product): void;
  (e: "duplicate", product: Product): void;
  (e: "delete", id: number): void;
  (e: "view", product: Product): void;
}>();
</script>

<template>
  <div class="container mt-4">
    <table class="table table-hover">
      <thead>
        <tr>
          <th>Nom</th>
          <th>Prix</th>
          <th>Stock</th>
          <th>Saveur</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="product in products"
          :key="product.id"
          @click="$emit('view', product)"
          style="cursor: pointer"
        >
          <td>{{ product.name }}</td>
          <td>${{ product.price.toFixed(2) }}</td>
          <td>
            <span
              :class="{
                'text-success': product.stockQuantity > 5,
                'text-warning':
                  product.stockQuantity > 0 && product.stockQuantity <= 5,
                'text-danger': product.stockQuantity === 0,
              }"
            >
              {{ product.stockQuantity }}
            </span>
          </td>
          <td>{{ product.flavor }}</td>
          <td>
            <div class="btn-group">
              <button
                class="btn btn-sm btn-primary"
                @click.stop="$emit('edit', product)"
              >
                Modifier
              </button>
              <button
                class="btn btn-sm btn-success"
                @click.stop="$emit('duplicate', product)"
              >
                Dupliquer
              </button>
              <button
                class="btn btn-sm btn-danger"
                @click.stop="$emit('delete', product.id)"
              >
                Supprimer
              </button>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
