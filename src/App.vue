<script setup>
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <div class="main-container">
    <button aria-label="Open temperature converter modal" class="open-btn" @click="openModal">
      Convert Temperature
    </button>

    <TemperatureModal :show="modalVisible" @close="modalVisible = false" />
  </div>
</template>

<script>
import TemperatureModal from "./components/TemperatureModal.vue";

export default {
  components: { TemperatureModal },
  data() {
    return {
      modalVisible: false
    };
  },
  methods: {
    openModal() {
      this.modalVisible = true;
      this.$nextTick(() => {
        document.getElementById("celsiusInput").focus();
      });
    }
  }
};
</script>

<style>
.main-container {
  text-align: center;
  margin-top: 100px;
}

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

.open-btn:hover {
  background: #0058cc;
  transform: scale(1.05);
}
</style>
