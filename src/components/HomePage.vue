<template>
  <v-container class="d-flex flex-column align-center">
    <v-card class="body pa-5 text-center">
      <v-card-title class="heading mt-4 mb-4"
        >ADVICE # {{ count }}</v-card-title
      >
      <v-card-text class="para"
        >"{{
          advice || "Hardwork beats talent when talent doen't work hard"
        }}"</v-card-text
      >
      <div v-if="isScreenLarge" class="line mt-5 mb-11 d-flex justify-center">
        <img src="/src/assets/pattern-divider-desktop.svg" alt="" />
      </div>
      <div v-else class="line mt-5 mb-11 d-flex justify-center">
        <img src="/src/assets/pattern-divider-mobile.svg" alt="" />
      </div>
    </v-card>
    <div
      @click="getAdvice"
      :loading="loading"
      class="btn d-flex justify-center align-center"
    >
      <v-icon icon="mdi-dice-5" size="large"></v-icon>
    </div>
  </v-container>
</template>
  
  <script setup>
import { ref } from "vue";
import axios from "axios";

const count = ref(1);
const advice = ref(""); // To store the advice text
const loading = ref(false); // Loading state for the button

const isScreenLarge = ref(window.innerWidth >= 599.5);

// Function to handle screen resizing
const handleResize = () => {
  isScreenLarge.value = window.innerWidth >= 599.5;
};

// Add and remove resize event listener
onMounted(() => window.addEventListener("resize", handleResize));
onBeforeUnmount(() => window.removeEventListener("resize", handleResize));

const getAdvice = async () => {
  loading.value = true;

  try {
    const response = await axios.get("https://api.adviceslip.com/advice");
    advice.value = response.data.slip.advice; // Extract advice from API response
    count.value += 1;
  } catch (error) {
    console.error("Error fetching advice:", error);
    advice.value = "Failed to fetch advice. Please try again!";
  } finally {
    loading.value = false; // Stop the loading state
  }
};
</script>
  
  <style scoped>
@font-face {
  font-family: f1;
  src: url(/src/assets/Manrope-Bold.ttf);
}
.heading {
  color: hsl(150, 100%, 66%);
  font-family: f1;
  font-size: 11px;
  letter-spacing: 2px;
}
.para {
  color: hsl(193, 51%, 90%);
  font-size: 25px;
  font-family: f1;
}
.body {
  border-radius: 10px;
  background-color: hsl(218, 20%, 24%);
  width: 480px;
}
.btn {
  cursor: pointer;
  background-color: hsl(150, 100%, 66%);
  color: hsl(229, 22%, 15%);
  width: 60px;
  height: 60px;
  border-radius: 50%;
  position: relative;
  top: -30px;
}
.btn:hover {
  box-shadow: 0 0 30px hsl(150, 100%, 66%);
}

@media (max-width: 599.5px) {
  .body {
    width: 400px;
  }
}
</style>
  