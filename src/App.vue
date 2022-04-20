<template>
  <div class="min-h-full font-Poppins box-border">
    <Navigation />
    <router-view />
  </div>
</template>

<script>
import Navigation from "./components/Navigation.vue";
import { ref } from "vue";
import { supabase } from "./supabase/init";
import store from "./store/index";

export default {
  components: { Navigation },
  setup() {
    // set null - prevent from render out
    const appReady = ref(null);
    // Checking to see user already logged in
    const user = supabase.auth.user();

    // If user does not exist -> make app ready
    if (!user) {
      appReady.value = true;
    }

    // Runs when there is a auth state change
    // if user is logged in -> fire
    supabase.auth.onAuthStateChange((_, session) => {
      store.methods.setUser(session);
      appReady.value = true;
    });
    return { appReady };
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@100;200;300;400;500;600;700&display=swap");
</style>
