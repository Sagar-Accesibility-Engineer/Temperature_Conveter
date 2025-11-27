<template>
  <transition name="modal-fade">
    <div v-if="show" class="overlay">
      <transition name="modal-zoom">
        <div role="dialog" aria-modal="true" aria-labelledby="tempTitle" class="modal-box">

          <h2 id="tempTitle" class="title">Temperature Converter</h2>


          <div class="input-group">
            <label for="celsiusInput">Celsius (°C)</label>
            <input id="celsiusInput" v-model.number="celsius" @input="convertToFahrenheit" />

          </div>

          <div class="input-group">
            <label for="fahrenheitInput">Fahrenheit (°F)</label>
            <input id="fahrenheitInput" v-model.number="fahrenheit" @input="convertToCelsius" />
          </div>
          <p aria-live="polite" class="sr-only">
            {{ announcement }}
          </p>

          <button @keyup.enter="$emit('close')" @keyup.space="$emit('close')" class="close-btn"
            @click="$emit('close')">Close</button>
        </div>
      </transition>
    </div>
  </transition>
</template>

<script>
export default {
  name: "TemperatureModal",
  props: ["show"],
  data() {
  return {
    celsius: 0,
    fahrenheit: 32,
    announcement: ""
  }
},
methods: {
  convertToFahrenheit() {
    this.fahrenheit = (this.celsius * 9/5 + 32).toFixed(2)
    this.announcement = `Celsius ${this.celsius} degree equals Fahrenheit ${this.fahrenheit} degree`
  },
  convertToCelsius() {
    this.celsius = ((this.fahrenheit - 32) * 5/9).toFixed(2)
    this.announcement = `Fahrenheit ${this.fahrenheit} degree equals Celsius ${this.celsius} degree`
  }
},

  mounted() {
    window.addEventListener("keydown", (e) => {
      if (e.key === "Escape" && this.show) this.$emit("close");
    });
  }

};


</script>

<style scoped>
.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity 0.3s ease;
}

.modal-fade-enter-from,
.modal-fade-leave-to {
  opacity: 0;
}

.modal-zoom-enter-active {
  animation: zoomIn 0.35s ease;
}

.modal-zoom-leave-active {
  animation: zoomOut 0.25s ease forwards;
}

@keyframes zoomIn {
  0% {
    transform: scale(0.6);
    opacity: 0;
  }

  100% {
    transform: scale(1);
    opacity: 1;
  }
}

@keyframes zoomOut {
  0% {
    transform: scale(1);
    opacity: 1;
  }

  100% {
    transform: scale(0.6);
    opacity: 0;
  }
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.55);
  display: flex;
  justify-content: center;
  align-items: center;
  backdrop-filter: blur(2px);
}


.modal-box {
  width: 350px;
  background: #ffffff;
  padding: 25px;
  border-radius: 15px;
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.25);
  text-align: center;
  font-family: "Segoe UI", sans-serif;
}


.title {
  margin-bottom: 20px;
  font-weight: 600;
}


.input-group {
  text-align: left;
  margin: 15px 0;
}

label {
  font-size: 14px;
  color: #555;
}

input {
  width: 100%;
  padding: 10px;
  margin-top: 6px;
  border-radius: 8px;
  border: 1px solid #1f0f0f;
  outline: none;
  transition: 0.25s;
  font-size: 16px;
}

input:focus {
  border-color: #4a90e2;
  box-shadow: 0 0 6px rgba(74, 144, 226, 0.4);
}


.close-btn {
  width: 100%;
  padding: 10px;
  background: #D30D0D;
  border: none;
  border-radius: 8px;
  color: white;
  margin-top: 15px;
  font-size: 16px;
  cursor: pointer;
  transition: 0.25s;
}

.close-btn:hover {
  background: #771e1e;
}
</style>
