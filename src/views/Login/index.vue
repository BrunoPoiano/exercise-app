<template>
  <div class="login-wrapper">
      <AboutView />
    <div class="login-form">
      <div class="card">
        <h3>Login</h3>
        <form @submit.prevent="handleLogin">
          <input
            class="input"
            placeholder="email"
            type="text"
            v-model="form.email"
          />
          <input
            class="input"
            placeholder="password"
            type="password"
            v-model="form.password"
          />
          <button type="submit" class="button">Logar</button>
        </form>
        <RouterLink to="create-account">
          <button type="submit" class="button">Criar Conta</button>
        </RouterLink>
      </div>
    </div>
  </div>
</template>

<script>
import AboutView from "../AboutView.vue";
import axios from "@/axios";
import { useRouter } from "vue-router";
import { ref } from "vue";
export default {
  components: { AboutView },
  setup() {
    const router = useRouter();
    const form = ref({});

    const handleLogin = (e) => {
      const params = {
        email: form.value.email,
        password: form.value.password,
      };

      axios.post("login", params).then(({ data }) => {
        localStorage.setItem("EXER_TOKEN", data.token);
        localStorage.setItem("EXER_USER", JSON.stringify(data.user));

        router.go("/");
      });
    };

    return {
      handleLogin,
      form,
    };
  },
};
</script>

<style scoped>
.login-wrapper {
  width: 100%;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
  gap: 20px;
  place-items: center;

  & .card {
    max-width: 400px;
    width: 100%;
    padding: 15px;

    border: 1px solid black;
    border-radius: var(--border-radius);

    display: grid;
    gap: 10px;

    & form {
      display: grid;
      gap: 10px;
    }
  }
}
</style>