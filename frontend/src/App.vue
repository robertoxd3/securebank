<script setup>
import { onMounted } from "vue";

import TheFooter from "@/components/layouts/TheFooter.vue";
import TheHeader from "@/components/layouts/TheHeader.vue";
import useAuth from "@/composables/useAuth";
import TheMenu from "@/components/layouts/TheMenu.vue";
import Alert from "@/components/layouts/Alert.vue";

const { getUserInfo, logout, state, verifier, accessToken, refreshToken } =
  useAuth();

onMounted(async () => {
  accessToken.value = localStorage.getItem("access_token");
  refreshToken.value = localStorage.getItem("refresh_token");
  state.value = localStorage.getItem("state");
  verifier.value = localStorage.getItem("verifier");

  const path = window.location.pathname;
  if (path == "/login" || path == "/callback") {
    return;
  }

  if (!accessToken.value) {
    logout();
    return;
  }

  await getUserInfo();
});
</script>

<template>
  <the-header />
  <!-- <the-menu /> -->

  <main>
    <v-container>
      <Alert />
      <!-- {{ $route.meta }} -->
      <v-btn class="ms-4 mt-3" prepend-icon="mdi-arrow-left" color="primary" v-if="$route.meta.showBackButton"
        @click="$router.go(-1)">
        Regresar
      </v-btn>
      <RouterView />
    </v-container>
  </main>

  <the-footer />
</template>