<template>
  <v-card-actions class="justify-center">
    <v-btn @click="printResults" prepend-icon="mdi-printer" variant="outlined">
      Drucken / Als PDF speichern
    </v-btn>
  </v-card-actions>
  <v-card-actions class="justify-center">
    <p class="text-caption text-center">
      Die Ergebnisse von Persönlichkeitstests dienen nicht als abschließende Bewertung, sondern als ein Werkzeug zur Selbstreflexion und können durch die momentane Stimmung beeinflusst werden.
    </p>
  </v-card-actions>
</template>

<script setup lang="ts">
const printResults = () => {
  window.print();
};
</script>

<style>
@media print {
  /* General print body styles */
  body {
    background: white !important;
    font-size: 10pt;
  }

  /* Hide UI elements that shouldn't be in the printout */
  .v-btn,
  .v-sparkline {
    display: none !important;
  }

  /* Ensure logo is visible and inverted for printing */
  .v-img {
    -webkit-print-color-adjust: exact !important;
    color-adjust: exact !important;
    filter: invert(1);
  }

  /* Ensure all text and icons are black for readability */
  * {
    color: #000 !important;
  }

  /* Adjust card appearance for a cleaner print */
  .v-card {
    box-shadow: none !important;
    border: none !important;
    background: transparent !important;
    break-inside: avoid;
  }
  .v-card--variant-tonal {
    --v-theme-surface-variant: transparent;
  }

  .v-card-title, .v-card-text {
    padding-top: 4px !important;
    padding-bottom: 4px !important;
  }

  /* Ensure expansion panels are open and compact for printing */
  .v-expansion-panel {
    break-inside: avoid;
  }
  .v-expansion-panel-title {
    min-height: auto !important;
    padding-top: 8px !important;
    padding-bottom: 8px !important;
  }
  .v-expansion-panel-title__icon {
    display: none !important;
  }
  .v-expansion-panel__wrapper {
    grid-template-rows: 1fr !important;
  }
  .v-expansion-panel-text {
    overflow: visible !important;
  }
  .v-expansion-panel-text__wrapper {
    padding: 8px 16px !important;
  }

  /* Force two columns for the 'people' cards on du.vue */
  .v-row > .v-col[cols="12"][md="6"] {
    flex-basis: 50% !important;
    max-width: 50% !important;
    padding: 6px !important;
  }
}
</style>