<template>
    <div class="flex justify-center items-center h-screen">
      <div>
        <Message v-bind:msg="msgForm" v-if="msg" />
      </div>
      <form class="w-full max-w-md" id="burger-form" @submit="createBurger">
        <div class="mt-4">
          <label class="block font-bold mb-2 pl-3 pb-1 border-l-4 border-yellow-400" for="nome">Nome</label>
          <input class="w-full px-3 py-2 border rounded focus:outline-none focus:ring-2 focus:ring-yellow-400" id="nome" name="name" v-model="nome" placeholder="Digite seu nome" type="text">
        </div>
        <div class="mb-6">
          <label class="block font-bold pl-3 border-l-4 border-yellow-400" for="pao">Escolha o pão:</label>
          <select class="w-full px-3 py-2 border rounded focus:outline-none focus:ring-2 focus:ring-yellow-400" name="pao" id="pao" v-model="pao">
            <option v-bind:value="pao.tipo" v-for="pao in paes" v-bind:key="pao.id">{{ pao.tipo }}</option>
          </select>
        </div>
        <div class="mb-6">
          <label class="block font-bold pl-3 border-l-4 border-yellow-400" for="carne">Escolha sua carne:</label>
          <select class="w-full px-3 py-2 border rounded focus:outline-none focus:ring-2 focus:ring-yellow-400" name="carne" id="carne" v-model="carne">
            <option v-bind:value="carne.tipo" v-for="carne in carnes" v-bind:key="carne.id">{{ carne.tipo }}</option>
          </select>
        </div>
        <div class="mb-6">
          <label class="block font-bold pl-3 border-l-4 border-yellow-400" for="pao">Selecione os opcionais</label>
          <div v-for="opcional in opcionaisdata" v-bind:key="opcional.tipo">
            <input class="mt-2" type="checkbox" name="opcionais" id="" v-model="opcionais" v-bind:value="opcional.tipo">
            <span>{{ opcional.tipo }}</span>
          </div>
        </div>
        <div class="mb-4">
          <input type="submit" class="w-full py-2 text-center font-bold text-yellow-400 bg-gray-800 border-gray-800 border-solid rounded hover:bg-transparent hover:text-gray-800" value="Criar meu Burger!">
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
            this.opcionaisdata  = data.opcionais
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
                headers: {"Content-Type": "application/json"},
                body: JSON.stringify(data)
            }) 

            const resposta = await req.json()
            
            // Mostrando mensagem
            this.msgForm = `Pedido N ${resposta.id} criado com sucesso!`
            this.msg = true

            // Limpando campos 

            this.nome = ""
            this.carne = ""
            this.pao = ""
            this.opcionais = []

            setTimeout(() => this.msg = false, 3000)
            
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

 input, select {
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
    transition: .5s
 }

 .submit-btn:hover {
    background: transparent;
    color: #222; 
 }

</style>


