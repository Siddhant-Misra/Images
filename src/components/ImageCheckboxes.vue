<template>
  <div>
    <h1>{{ imageLabels.image_name }}</h1>
    <div v-for="(coordinates, label) in imageLabels.image_labels" :key="label">
      <v-checkbox :model-value="isChecked(label)" @update:model-value="toggleCheckbox(label, coordinates)" :label="label"></v-checkbox>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    imageLabels: Object,
    selectedLabels: Array,
  },
  methods: {
    isChecked(label) {
      return this.selectedLabels.includes(label);
    },
    toggleCheckbox(label, coordinates) {
      const updatedLabels = this.selectedLabels.includes(label)
        ? this.selectedLabels.filter((selectedLabel) => selectedLabel !== label)
        : [...this.selectedLabels, label];

      this.$emit("update:selectedLabels", updatedLabels, coordinates);
    },
  },
};
</script>

<style scoped>
/* You can add some styling for better alignment if needed */
</style>
