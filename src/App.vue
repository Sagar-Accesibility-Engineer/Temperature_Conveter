<script setup>

</script>

<template>
  <div class="main-container">
    
    <!-- 
      Button to open the temperature converter modal.
      aria-label improves accessibility for screen reader users.
    -->
    <button 
      aria-label="Open temperature converter modal" 
      class="open-btn" 
      @click="openModal"
    >
      Convert Temperature
    </button>

    <!-- 
      Temperature Modal Component
      :show controls visibility
      @close listens for close event from modal
    -->
    <TemperatureModal 
      :show="modalVisible" 
      @close="modalVisible = false" 
    />
  </div>
</template>

<script>
/*
  Importing the TemperatureModal component
  which handles all modal logic + accessibility + conversions.
*/
import TemperatureModal from "./components/TemperatureModal.vue";

export default {
  components: { TemperatureModal },

  data() {
    return {
      modalVisible: false   // Controls whether modal is open or closed
    };
  },

  methods: {
    /*
      Opens the modal and sets focus on the Celsius input field.
      nextTick ensures the modal is rendered before focusing.
    */
    openModal() {
      this.modalVisible = true;

      this.$nextTick(() => {
        document.getElementById("celsiusInput")?.focus();
      });
    }
  }
};
</script>

<style>
/* Main container for centering and layout */
.main-container {
  text-align: center;
  margin-top: 100px;
}

/* Style for the "Convert Temperature" button */
.open-btn {
  padding: 15px 30px;
  background: rgb(7, 81, 172);
  color: white;
  border: none;
  font-size: 20px;
  border-radius: 10px;
  cursor: pointer;
  transition: 0.3s ease;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.15);
}

/* Hover effect for better user feedback */
.open-btn:hover {
  background: #0058cc;
  transform: scale(1.05);
}
</style>
