<template>
  <v-container>
    <v-row justify="space-between">
      <!-- Left side (flex: 3) -->
      <v-col class="left-side" cols="12" sm="6">
        <v-img src="@/assets/desk.jpg" alt="Desk Image" class="left-content"></v-img>
        <div
          v-for="(coordinates, label) in imageLabels.image_labels"
          :key="label"
          class="overlay"
          :class="{ 'fade-in': selectedLabels.includes(label) }"
          :style="getOverlayStyle(label)"
        ></div>
      </v-col>

      <!-- Space between the columns -->
      <v-col cols="12" sm="1"></v-col>

      <!-- Right side (flex: 9) -->
      <v-col cols="12" sm="5">
        <ImageCheckboxes
          :imageLabels="imageLabels"
          :selectedLabels="selectedLabels"
          @update:selectedLabels="updateSelectedLabels"
        />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import ImageCheckboxes from "@/components/ImageCheckboxes.vue";
import data from "@/assets/coords.json";

export default {
  name: "HelloWorld",
  data() {
    return {
      imageLabels: {},
      selectedLabels: [],
      dataLoaded: false, // Add a flag to track data loading status
    };
  },
  created() {
    if (!this.dataLoaded) {
      this.loadData();
    }
  },
  methods: {
    async loadData() {
      try {
        console.log("THIS IS THE JSON", data);
        this.imageLabels = data;
        this.dataLoaded = true; // Set the flag to true after loading data
      } catch (error) {
        console.error("Error loading data:", error);
      }
    },
    updateSelectedLabels(newSelectedLabels) {
      this.selectedLabels = newSelectedLabels;
    },
    getOverlayStyle(label) {
      const coordinates = this.imageLabels.image_labels[label];
      if (coordinates && this.selectedLabels.includes(label)) {
        const [top, left, height, width] = coordinates.map(parseFloat);
        return {
          top: `${top}%`,
          left: `${left}%`,
          height: `${height}%`,
          width: `${width}%`,
        };
      }
      return {};
    },
  },
  components: {
    ImageCheckboxes,
  },
};
</script>

<style scoped>
.left-side {
  flex: 3;
  position: relative;
  padding: 0px;
}

.left-content {
  height: 100%;
}

.overlay {
  position: absolute;
  border: 1px solid red;
  background-color: rgba(255, 0, 0, 0.2);
  pointer-events: none;
  box-sizing: border-box;
  opacity: 0;
  transition: opacity 0.5s ease-in;
}

.overlay.fade-in {
  opacity: 1;
}
</style>
