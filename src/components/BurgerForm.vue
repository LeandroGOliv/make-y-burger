<template>
  <div>
    <Message :msg="msg" v-show="msg" />
    <div>
      <form id="burger_form" @submit.prevent="createBurger">
        <div class="input_container">
          <!-- transformar esse input em um componente sepa-->
          <label for="nome">Nome do cliente:</label>
          <input
            type="text"
            name="nome"
            id="nome"
            placeholder="Digite o seu nome"
            v-model="nome"
          />
        </div>
        <div class="input_container">
          <label for="pao">Escolha o pão:</label>
          <select name="pao" id="pao" v-model="pao">
            <option value="" disabled>Selecione o seu pão</option>
            <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">
              {{ pao.tipo }}
            </option>
          </select>
        </div>
        <div class="input_container">
          <label for="carne">Selecione a carne do seu burger:</label>
          <select name="carne" id="carne" v-model="carne">
            <option value="" disabled>Selecione o tipo de carne</option>
            <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">
              {{ carne.tipo }}
            </option>
          </select>
        </div>
        <div id="opcionais_container" class="input_container">
          <label id="opcionais_title" for="opcionais"
            >Selecione os opcionais:</label
          >
          <div
            class="checkbox_container"
            v-for="opcional in opcionaisData"
            :key="opcional.id"
          >
            <input
              type="checkbox"
              name="opcionais"
              id="opcionais"
              v-model="opcionais"
              :value="opcional.tipo"
            />
            <span>{{ opcional.tipo }}</span>
          </div>
        </div>
        <div class="input_container">
          <input type="submit" value="Criar meu Burger!" class="submit_btn" />
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import Message from "./Message.vue";

//consts que recebem os dados do back end
const paes = ref([]);
const carnes = ref([]);
const opcionaisData = ref([]);

//consts que recebem os vmodels dos inputs
const nome = ref("");
const pao = ref("");
const carne = ref("");
const opcionais = ref([]);

const msg = ref("");

async function getIgredients() {
  const req = await fetch("http://localhost:3000/ingredientes");
  const data = await req.json();

  paes.value = data.paes;
  carnes.value = data.carnes;
  opcionaisData.value = data.opcionais;
}
onMounted(() => {
  getIgredients();
});

async function createBurger() {
  const data = {
    nome: nome.value,
    carne: carne.value,
    pao: pao.value,
    opcionais: [...opcionais.value],
    status: "Solicitado",
  };
  console.log(data);
  const dataJson = JSON.stringify(data);

  const req = await fetch("http://localhost:3000/burgers", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: dataJson,
  });

  const res = await req.json();

  msg.value = `Pedido código '${res.id}' relizado com sucesso!`;

  setTimeout(() => (msg.value = ""), 3000);

  limparCampos();
}

const limparCampos = () => {
  nome.value = "";
  pao.value = "";
  carne.value = "";
  opcionais.value = [];
};
</script>

<style scoped>
#burger_form {
  max-width: 400px;
  margin: 0 auto;
}

.input_container {
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

#opcionais_container {
  flex-direction: row;
  flex-wrap: wrap;
}

#opcionais_title {
  width: 100%;
}

.checkbox_container {
  display: flex;
  align-items: flex-start;
  width: 50%;
  margin-bottom: 20px;
}

.checkbox_container span,
.checkbox_container input {
  width: auto;
}

.checkbox_container span {
  margin-left: 6px;
  font-weight: bold;
}

.submit_btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}

.submit_btn:hover {
  background-color: transparent;
  color: #222;
}
</style>
