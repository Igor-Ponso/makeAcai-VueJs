<template>
  <div id="">
    <Message :msg="msg" v-show="msg" />
    <div>
      <form action="" id="form" @submit="create">
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
          <label for="tamanho">Escolha o Tamanho</label>
          <select name="tamanho" id="tamanho" v-model="tamanho">
            <option
              v-for="tamanho in tamanhoList"
              :key="tamanho.id"
              :value="tamanho.tipo"
              >{{ tamanho.tipo }}</option
            >
          </select>
        </div>
        <div class="input-container">
          <label for="fruta">Escolha a fruta do seu Açaí</label>
          <select name="fruta" id="fruta" v-model="fruta">
            <option
              v-for="fruta in frutaList"
              :key="fruta.id"
              :value="fruta.tipo"
            >
              {{ fruta.tipo }}
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
          <input type="submit" class="submit-btn" value="Criar meu Açaí!" />
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import Message from "./Message.vue";
export default {
  name: "Form",
  data() {
    return {
      tamanhoList: null,
      frutaList: null,
      opcionaisdata: null,
      name: null,
      tamanho: null,
      fruta: null,
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

      this.tamanhoList = data.tamanho;
      this.frutaList = data.fruta;
      this.opcionaisdata = data.opcional;
    },

    async create(e) {
      e.preventDefault();

      const data = JSON.stringify({
        name: this.name,
        fruta: this.fruta,
        tamanho: this.tamanho,
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
      this.fruta = "";
      this.tamanho = "";
      this.opcionais = [];
    },
  },
  mounted() {
    this.getIngredients();
  },
};
</script>

<style scoped>
#form {
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
  border-left: 4px solid #b30083;
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
  margin-left: 15px
}

.submit-btn {
  background-color: #222;
  color: #f561cd;
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
