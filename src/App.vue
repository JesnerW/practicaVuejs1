<template>
  <Suspense>
    <template #default>
      <Home />
    </template>
    <template #fallback>
      <SplashScreen />
    </template>
  </Suspense>
</template>

<script>
import SplashScreen from "./components/SplashScreen.vue";
import { defineAsyncComponent } from "vue";
export default {
  components: {
    SplashScreen,
    Home: defineAsyncComponent(
      () =>
        new Promise((resolve) => {
          setTimeout(() => {
            resolve(import("./components/HomePage.vue"));
          }, 1000);
        })
    ),
  },
};
</script>

<style>
body {
  width: 50vw;
  margin: 0 auto;
  font-family: Arial, Helvetica, sans-serif;
}
.app {
  min-height: 100vh;
  margin: 0;
}
* {
  --brand-green: #04b500;
  --brand-blue: #0689b0;
}
@media (max-width: 700px) {
  body {
    width: 100vw;
  }
}
</style>
