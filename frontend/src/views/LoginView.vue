<template>
  <div class="d-flex justify-content-center mt-10">
    <v-card class="text-center p-5 pt-4 pb-4 shadow">
      <v-img class="mx-auto" src="https://www.itca.edu.sv/wp-content/themes/elaniin-itca/images/logoColor.png"
        style="max-width: 350px" />
      <h2 class="text-center text-h5">
        <b>Bienvenido a tu banca digital</b> 👋
      </h2>
      <base-button title="Iniciar sesión" type="primary" class="mt-5 mb-3 p-5 py-0" @click="redirectToProvider()" />
    </v-card>
  </div>
</template>

<script setup>
import { onMounted } from "vue";
import useAuth from "../composables/useAuth";
import BaseButton from "../components/base-components/BaseButton.vue";
import { useRouter } from "vue-router";
import gatewayApi from "../services/gatewayApi";
const {
  challenge,
  state,
  verifier,
  isLoggedIn,
  cryptoSha256,
  createRandomString,
  base64Url,
  redirectToProvider,
} = useAuth();
const router = useRouter();

onMounted(async () => {
  state.value = createRandomString(40);
  verifier.value = createRandomString(128);

  challenge.value = base64Url(cryptoSha256(verifier.value));
  localStorage.setItem("state", state.value);
  localStorage.setItem("verifier", verifier.value);
});
</script>

<style scoped>
h2 {
  font-size: 26px;
  font-weight: normal;
}
</style>
