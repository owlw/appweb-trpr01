<script setup lang="ts">
import type { Product } from "../scripts/types";

const props = defineProps<{ products: Product[] }>();

// Inspiré du site : https://www.clearpeople.com/blog/export-data-to-csv-with-typescript-without-format-issues
// J'ai aussi demandé de l'aide à ChatGPT car j'avais beaucoup de problèmes même avec l'aide du site
const exportToCSV = () => {
  const filename = "supplements-de-mathieu.csv";
  const rows = props.products;
  const headers = [
    "ID",
    "Nom",
    "Prix",
    "Quantité en stock",
    "Description",
    "Saveur",
  ];

  if (!rows || !rows.length) {
    return;
  }

  const separator = ",";
  const keys: (keyof Product)[] = [
    "id",
    "name",
    "price",
    "stockQuantity",
    "description",
    "flavor",
  ];

  const csvContent =
    "sep=,\n" +
    headers.join(separator) +
    "\n" +
    rows
      .map((row) => {
        return keys
          .map((k) => {
            let cell = row[k] === null || row[k] === undefined ? "" : row[k];
            const cellValue = String(cell).replace(/"/g, '""');

            if (cellValue.search(/("|,|\n)/g) >= 0) {
              return `"${cellValue}"`;
            }
            return cellValue;
          })
          .join(separator);
      })
      .join("\n");

  const blob = new Blob([csvContent], { type: "text/csv;charset=utf-8;" });
  const link = document.createElement("a");
  const url = URL.createObjectURL(blob);

  link.setAttribute("href", url);
  link.setAttribute("download", filename);
  link.style.visibility = "hidden";
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
};
</script>

<template>
  <button @click="exportToCSV" class="btn btn-secondary">
    <i class="bi bi-download me-1"></i> Exporter en CSV
  </button>
</template>
