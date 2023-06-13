<template>
  <div>
    <div v-if="msg">
      <div class="message-container">
        <Message :msg="msgForm" />
      </div>
    </div>
    <form id="burger-form" @submit="createBurger">
      <div class="input-container">
        <label for="nome">Nome</label>
        <input id="nome" name="name" v-model="nome" placeholder="Digite seu nome" type="text">
      </div>
      <div class="input-container">
        <label for="pao">Escolha o pão:</label>
        <select name="pao" id="pao" v-model="pao">
          <option v-bind:value="pao.tipo" v-for="pao in paes" v-bind:key="pao.id">{{ pao.tipo }}</option>
        </select>
      </div>
      <div class="input-container">
        <label for="carne">Escolha sua carne:</label>
        <select name="carne" id="carne" v-model="carne">
          <option v-bind:value="carne.tipo" v-for="carne in carnes" v-bind:key="carne.id">{{ carne.tipo }}</option>
        </select>
      </div>
      <div class="input-container">
        <label for="pao">Selecione os opcionais</label>
        <div v-for="opcional in opcionaisdata" v-bind:key="opcional.tipo" class="checkbox-container">
          <input type="checkbox" name="opcionais" id="" v-model="opcionais" v-bind:value="opcional.tipo">
          <span>{{ opcional.tipo }}</span>
        </div>
      </div>
      <div class="input-container">
        <input type="submit" value="Criar meu Burger!" class="submit-btn">
      </div>
    </form>
  </div>
</template>

<script>
import Message from './Message.vue'

export default {
  name: "BurgerForm",
  components: {
    Message
  },
  data() {
    return {
      msgForm: "",
      msg: false,
      pedidoSet: false,
      carnes: null,
      paes: null,
      opcionaisdata: null,
      nome: null,
      carne: null,
      pao: null,
      opcionais: [],
      status: "Solicitado"
    }
  },
  methods: {
    async getIngrdientes() {
      const req = await fetch("https://json-backend.vercel.app/ingredientes")
      const data = await req.json()

      this.carnes = data.carnes
      this.paes = data.paes
      this.opcionaisdata = data.opcionais
    },

    async createBurger(e) {
      e.preventDefault()
      const data = {
        nome: this.nome,
        carne: this.carne,
        pao: this.pao,
        opcionais: this.opcionais,
        status: "Solicitado"
      }

      const req = await fetch("https://json-backend.vercel.app/burgers", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data)
      })

      const resposta = await req.json()

      // Mostrando mensagem
      this.msgForm = `Pedido Nº ${resposta.id} criado com sucesso!`
      this.msg = true

      // Limpando campos
      this.nome = ""
      this.carne = ""
      this.pao = ""
      this.opcionais = []

      setTimeout(() => {
        this.msg = false
      }, 3000)
    }
  },
  mounted() {
    this.getIngrdientes()
  }
}
</script>

<style scoped>
#burger-form {
  max-width: 400px;
  margin: 0 auto;
}

.message-container {
  margin-bottom: 20px;
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
  border-left: 4px solid #FCBA03;
}

input,
select {
  padding: 5px 10px;
  width: 300px;
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
  margin-left: 6px;
  font-weight: bold;
}

.submit-btn {
  background-color: #222;
  color: #FCBA03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: .5s;
}

.submit-btn:hover {
  background: transparent;
  color: #222;
}
</style>
