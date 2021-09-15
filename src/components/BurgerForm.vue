<template>
  <div id="">
    <Message :msg="msg" v-show="msg" />
    <div>
      <form action="" id="burger-form" @submit="createBurger">
        <div class="input-container">
          <label for="name">Nome</label>
          <input
            type="text"
            name="name"
            id=""
            v-model="name"
            placeholder="Digite o seu nome"
          />
        </div>
        <div class="input-container">
          <label for="pao">Escolha o Pão</label>
          <select name="pao" id="pao" v-model="pao">
            <option value="" selected>Selecione seu pão</option>
            <option v-for="pao in paoList" :key="pao.id" :value="pao.tipo">{{
              pao.tipo
            }}</option>
          </select>
        </div>
        <div class="input-container">
          <label for="carne">Escolha a Carne do seu Burger</label>
          <select name="carne" id="carne" v-model="carne">
            <option value="" selected>Selecione seu Carne</option>
            <option
              v-for="carne in carneList"
              :key="carne.id"
              :value="carne.tipo"
            >
              {{ carne.tipo }}
            </option>
          </select>
        </div>
        <div class="input-container" id="opcionais-container">
          <label id="opcionais-title" for="opcionais">
            Selecione os opcionais
          </label>
          <div
            class="checkbox-container"
            v-for="opcional in opcionaisdata"
            :key="opcional.id"
            :value="opcional.tipo"
          >
            <input
              type="checkbox"
              name="opcionais"
              v-model="opcionais"
              :value="opcional.tipo"
            />
            <span>{{ opcional.tipo }}</span>
          </div>
        </div>
        <div class="input-container">
          <input type="submit" class="submit-btn" value="Criar meu Burger!" />
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import Message from "./Message.vue";
export default {
  name: "BurgerForm",
  data() {
    return {
      paoList: null,
      carneList: null,
      opcionaisdata: null,
      name: null,
      pao: null,
      carne: null,
      opcionais: [],
      msg: null,
    };
  },
  components: {
    Message,
  },
  methods: {
    async getIngredients() {
      const req = await fetch("http://localhost:3000/ingredientes");
      const data = await req.json();

      this.paoList = data.pao;
      this.carneList = data.carne;
      this.opcionaisdata = data.opcional;
    },

    async createBurger(e) {
      e.preventDefault();

      const data = JSON.stringify({
        name: this.name,
        carne: this.carne,
        pao: this.pao,
        opcionais: Array.from(this.opcionais),
        status: "Solicitado",
      });

      const req = await fetch("http://localhost:3000/pedidos", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: data,
      });

      const res = await req.json();

      this.msg = `Pedido Nº${res.id} realizado com sucesso`;

      setTimeout(() => (this.msg = ""), 3000);

      this.name = "";
      this.carne = "";
      this.pao = "";
      this.opcionais = [];
    },
  },
  mounted() {
    this.getIngredients();
  },
};
</script>

<style scoped>
#burger-form {
  max-width: 400px;
  margin: 0 auto;
}

.input-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}

label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #fcba03;
}

input,
select {
  padding: 5px 10px;
  width: 300px;
}

#opcionais-container {
  flex-direction: row;
  flex-wrap: wrap;
}

#opcionais-title {
  width: 100%;
}

.checkbox-container {
  display: flex;
  align-items: flex-start;
  width: 50%;
  margin-bottom: 20px;
}

.checkbox-container span,
.checkbox-container input {
  width: auto;
}

.checkbox-container span {
  font-weight: bold;
}

.submit-btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  cursor: pointer;
  transition: 0.5s;
  margin: 0 auto;
}

.submit-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>
