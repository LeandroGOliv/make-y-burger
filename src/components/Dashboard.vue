<template>
  <div id="burger_table">
    <Message :msg="msg" v-show="msg" />
    <div>
      <div id="burger_table_heading">
        <div class="order_id">#:</div>
        <div>Cliente:</div>
        <div>Pão:</div>
        <div>Carne:</div>
        <div>Opcionais:</div>
        <div>Ações:</div>
      </div>
    </div>
    <div id="burger_table_rows">
      <div class="burger_table_row" v-for="burger in burgers" :key="burger.id">
        <div class="order_number">{{ burger.id }}</div>
        <div>{{ burger.nome }}</div>
        <div>{{ burger.pao }}</div>
        <div>{{ burger.carne }}</div>
        <div>
          <ul>
            <li v-for="(opcional, index) in burger.opcionais" :key="index">
              {{ opcional }}
            </li>
          </ul>
        </div>
        <div>
          <select
            name="status"
            class="status"
            @change="updateBurger($event, burger.id)"
          >
            <option value="" disabled>Selecione</option>
            <option
              :value="s.tipo"
              v-for="s in status"
              :key="s.id"
              :selected="burger.status == s.tipo"
            >
              {{ s.tipo }}
            </option>
          </select>
          <button class="delete_btn" @click="deleteBurger(burger.id)">
            Cancelar
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import Message from "./Message.vue";
import { onMounted, ref } from "vue";
const burgers = ref(null);
const burger_id = ref(null);
const status = ref([]);

const msg = ref("");

async function getPedidos() {
  const req = await fetch("http://localhost:3000/burgers");
  const data = await req.json();

  burgers.value = data;

  console.log(burgers.value);

  getStatus();
}
async function getStatus() {
  const req = await fetch("http://localhost:3000/status");
  const data = await req.json();

  status.value = data;
}
async function deleteBurger(id) {
  const req = await fetch(`http://localhost:3000/burgers/${id}`, {
    // caso fosse uma outra api talvez fosse de outra forma
    method: "DELETE",
  });
  const res = await req.json();

  msg.value = `Pedido removido com sucesso!`;

  setTimeout(() => (msg.value = ""), 3000);

  getPedidos(); // força uma atualizacao do sistemas por meio do backend
}
async function updateBurger(event, id) {
  const option = event.target.value;
  const dataJson = JSON.stringify({ status: option });

  const req = await fetch(`http://localhost:3000/burgers/${id}`, {
    method: "PATCH",
    headers: {
      "Content-Type": "application/json",
    },
    body: dataJson,
  });
  const res = await req.json();

  msg.value = `O pedido código "${res.id}" foi atualizado para "${res.status}"!`;

  setTimeout(() => (msg.value = ""), 3000);
}

onMounted(() => {
  getPedidos();
});
</script>

<style scoped>
#burger_table {
  max-width: 1200px;
  margin: 0 auto;
}

#burger_table_heading,
#burger_table_rows,
.burger_table_row {
  display: flex;
  flex-wrap: wrap;
}

#burger_table_heading {
  font-weight: bold;
  padding: 12px;
  border-bottom: 3px solid #333;
}

#burger_table_heading div,
.burger_table_row div {
  width: 19%;
}

.burger_table_row {
  width: 100%;
  padding: 12px;
  border-bottom: 1px solid #ccc;
}

#burger_table_heading .order_id,
.burger_table_row .order_number {
  width: 5%;
}

select {
  padding: 12px 6px;
  margin-right: 12px;
}

.delete_btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  font-size: 16px;
  border: 2px solid #222;
  padding: 10px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}

.delete_btn:hover {
  background-color: transparent;
  color: #222;
}
</style>
