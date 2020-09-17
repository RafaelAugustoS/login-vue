<template>
  <div>
    <h1>Titulo</h1>

    <form @submit="submit">
      <input
        :class="{ fieldError: erros.email }"
        type="text"
        placeholder="Digite seu email"
        v-model="email"
        @blur="checkFields"
      />
      <input
        :class="{ fieldError: erros.senha }"
        type="password"
        placeholder="Digite sua senha"
        v-model="senha"
        @blur="checkFields"
      />

      <button>Entrar</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      email: "",
      senha: "",
      erros: {
        email: false,
        senha: false,
      },
    };
  },
  methods: {
    async submit(e) {
      e.preventDefault();

      const { data } = await axios.get("http://localhost:3000/users", {
        params: {
          email: this.email,
          password: this.senha,
        },
      });

      if (data.length > 0) {
        this.$router.push({ name: "dashboard" });
      } else {
        alert("Nenhum usuario encontrado");
      }
    },
    checkFields() {
      if (this.email === "") {
        this.erros.email = true;
      } else {
        this.erros.email = false;
      }

      if (this.senha === "") {
        this.erros.senha = true;
      } else {
        this.erros.senha = false;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.fieldError {
  border: 1px solid red;
}
</style>
