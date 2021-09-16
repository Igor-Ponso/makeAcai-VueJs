<template>
  <div id="pedido-table">
    <Message :msg="msg" v-show="msg" />
    <div>
      <div id="pedido-table-heading">
        <div class="order-id">#</div>
        <div>Cliente:</div>
        <div>Tamanho:</div>
        <div>Fruta:</div>
        <div>Opcionais</div>
        <div>Ações:</div>
      </div>
      <div id="pedido-table-rows">
        <div v-show="burgers == null">Sem nenhum pedido</div>
        <div
          class="pedido-table-row"
          v-for="pedido in burgers"
          :key="pedido.id"
        >
          <div class="order-number">{{ pedido.id }}</div>
          <div>{{ pedido.name }}</div>
          <div>{{ pedido.tamanho }}</div>
          <div>{{ pedido.fruta }}</div>
          <div>
            <ul>
              <li v-for="(opcional, index) in pedido.opcionais" :key="index">
                {{ opcional }}
              </li>
            </ul>
          </div>
          <select
            name="status"
            id=""
            class="status"
            @change="updatePedidos($event, pedido.id)"
          >
            <option
              v-for="stat in status"
              :key="stat.id"
              :value="stat.tipo"
              :selected="stat.tipo == pedido.status"
            >
              {{ stat.tipo }}
            </option>
          </select>
          <button class="delete-btn" @click="deletePedido(pedido.id)">
            Cancelar
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Message from "./Message.vue";

export default {
  name: "Dashboard",
  data() {
    return {
      burgers: null,
      burger_id: null,
      status: [],
      msg: null,
    };
  },
  components: {
    Message,
  },
  methods: {
    async getPedidos() {
      const req = await fetch("http://localhost:3000/pedidos");
      this.burgers = await req.json();

      //resgatar os status
      this.getStatus();
    },
    async getStatus() {
      const req = await fetch("http://localhost:3000/status");
      this.status = await req.json();
    },

    async deletePedido(id) {
      const req = await fetch(`http://localhost:3000/pedidos/${id}`, {
        method: "DELETE",
      });

      const res = await req.json();

      this.getPedidos();

      this.msg = `Pedido ${id} removido`;
      setTimeout(() => (this.msg = ""), 3000);
    },

    async updatePedidos(event, id) {
      const opt = event.target.value;

      const dataJson = JSON.stringify({ status: opt });

      const req = await fetch(`http://localhost:3000/pedidos/${id}`, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });

      const res = await req.json();

      this.msg = `Pedido ${id} atualizado para ${opt}`;
      setTimeout(() => (this.msg = ""), 3000);
    },
  },
  mounted() {
    this.getPedidos();
  },
};
</script>

<style scoped>
#pedido-table {
  max-width: 1200px;
  margin: 0 auto;
}

#pedido-table-heading,
#pedido-table-rows,
.pedido-table-row {
  display: flex;
  flex-wrap: wrap;
}

#pedido-table-heading {
  font-weight: bold;
  padding: 12px;
  border-bottom: 3px solid #333;
}

#pedido-table-heading div,
.pedido-table-row div {
  width: 19%;
}

.pedido-table-row {
  width: 100%;
  padding: 12px;
  border-bottom: 1px solid #ccc;
}

#pedido-table-heading .order-id,
.pedido-table-row .order-number {
  width: 5%;
}

select {
  padding: 12px 6px;
  margin-right: 12px;
}

.delete-btn {
  background-color: #222;
  color: #b30083;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}

.delete-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>
