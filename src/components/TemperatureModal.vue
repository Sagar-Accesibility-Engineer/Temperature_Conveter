<template>
  <!-- Modal wrapper with fade transition -->
  <transition name="modal-fade">
    <div v-if="show" class="overlay">
      <!-- Inner modal box with zoom animation -->
      <transition name="modal-zoom">

        <!-- Accessibility: Identifies modal as dialog -->
        <div role="dialog" aria-modal="true" aria-label="Temperature converter modal" class="modal-box" ref="modal">
          
          <!-- Modal title -->
          <h2 id="tempTitle" class="title">Temperature Converter</h2>

          <!-- Celsius input group -->
          <div class="input-group">
            <label for="celsiusInput">Celsius (°C)</label>
            <input id="celsiusInput" ref="firstFocusable" 
            v-model.number="celsius" 
            @input="convertToFahrenheit" 
            placeholder="Enter Celsius"
            />
          </div>

          <!-- Fahrenheit input group -->
          <div class="input-group">
            <label for="fahrenheitInput">Fahrenheit (°F)</label>
            <input id="fahrenheitInput" v-model.number="fahrenheit" @input="convertToCelsius"
            placeholder="Enter Fahrenheit"
            />
          </div>

          <!-- Screen reader announcement for conversion updates -->
          <p aria-live="polite" class="sr-only">
            {{ announcement }}
          </p>

          <!-- Close button with focus trap and keyboard accessibility -->
          <button ref="lastFocusable" 
            @keyup.enter="$emit('close')"
            @keyup.space="$emit('close')"
            class="close-btn"
            @click="$emit('close')" 
            >
            Close
          </button>

        </div>
      </transition>
    </div>
  </transition>
</template>

<script>
export default {
  name: "TemperatureModal",
  props: ["show"], // Accepts show prop to toggle modal visibility

  data() {
    return {
      celsius: "", // Stores Celsius input
      fahrenheit: "", // Stores Fahrenheit input
      announcement: "", // Stores text for screen reader announcements
      focusableElements: [] // Stores all focusable elements inside modal
    };
  },

  watch: {
    // Watcher for modal visibility
    show(newVal) {
      if (newVal) this.activateFocusTrap(); // Activate focus trap when modal opens
    }
  },

  methods: {
    // Convert Celsius to Fahrenheit
    convertToFahrenheit() {
      if (this.celsius === "") {
        this.fahrenheit = "";
        return;
      }
      this.fahrenheit = (this.celsius * 9 / 5 + 32).toFixed(2);
      this.announcement = `Celsius ${this.celsius} degree equals Fahrenheit ${this.fahrenheit} degree`;
    },

    // Convert Fahrenheit to Celsius
    convertToCelsius() {
      if (this.fahrenheit === "") {
        this.celsius = "";
        return;
      }
      this.celsius = ((this.fahrenheit - 32) * 5 / 9).toFixed(2);
      this.announcement = `Fahrenheit ${this.fahrenheit} degree equals Celsius ${this.celsius} degree`;
    },

    // Focus trap logic to keep keyboard navigation inside modal
    activateFocusTrap() {
      this.$nextTick(() => {
        const modal = this.$refs.modal;

        // Select all focusable elements inside modal
        this.focusableElements = modal.querySelectorAll(
          "button, input, select, textarea, a[href], [tabindex]:not([tabindex='-1'])"
        );

        // Focus the first input when modal opens
        this.$refs.firstFocusable.focus();

        // Listen for Tab key to implement focus trap
        modal.addEventListener("keydown", this.handleTabKey);
      });
    },

    // Handle tabbing inside modal
    handleTabKey(e) {
      if (e.key !== "Tab") return;

      const first = this.$refs.firstFocusable;
      const last = this.$refs.lastFocusable;

      if (e.shiftKey) {
        // Shift + Tab → cycle backward
        if (document.activeElement === first) {
          e.preventDefault();
          last.focus();
        }
      } else {
        // Tab → cycle forward
        if (document.activeElement === last) {
          e.preventDefault();
          first.focus();
        }
      }
    }
  },

  mounted() {
    // Close modal on Escape key
    window.addEventListener("keydown", (e) => {
      if (e.key === "Escape" && this.show) this.$emit("close");
    });
  }
};
</script>

<style scoped>
/* Fade animation for modal overlay */
.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity 0.3s ease;
}

.modal-fade-enter-from,
.modal-fade-leave-to {
  opacity: 0;
}

/* Zoom animation for modal box */
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

/* Overlay styling */
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.55);
  /* Semi-transparent background */
  display: flex;
  justify-content: center;
  align-items: center;
  backdrop-filter: blur(2px);
}

/* Modal box styling */
.modal-box {
  width: 350px;
  background: #ffffff;
  padding: 25px;
  border-radius: 15px;
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.25);
  text-align: center;
  font-family: "Segoe UI", sans-serif;
}

/* Title styling */
.title {
  margin-bottom: 20px;
  font-weight: 600;
}

/* Input group styling */
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

/* Input focus effect */
input:focus {
  border-color: #4a90e2;
  box-shadow: 0 0 6px rgba(74, 144, 226, 0.4);
}

/* Close button styling */
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
