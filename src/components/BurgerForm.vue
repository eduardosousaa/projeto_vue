<template>
    <div>
        <Message :msg="msg" v-show="msg" />
        <div>
            <form id="burger-form" @submit="createBurger">
                <div class="input-container">
                    <label for="nome">Nome do cliente: </label>
                    <input type="text" name="nome" id="nome" v-model="nome" placeholder="Digite o seu nome: ">
                </div>
                <div class="input-container">
                    <label for="pao">Escolha o pão: </label>
                    <select name="pao" id="pao" v-model="pao">
                        <option value="">Selecione o seu pão</option>
                        <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">
                            {{ pao.tipo }}
                        </option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="carne">Escolha a carne do seu Burger: </label>
                    <select name="carne" id="carne" v-model="carne">
                        <option value="">Selecione o tipo de carne</option>
                        <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">
                            {{ carne.tipo }}
                        </option>
                    </select>
                </div>
                <div id="opcionais-conteiner" class="input-container">

                    <label id="opicionais-title" for="opicionais">Selecione os opicionais: </label>

                    <div class="checkbox-container" v-for="opcional in opcionaisData" :key="opcional.id">
                        <input type="checkbox" name="opicionais" id="opicionais"
                        :value="opcional.tipo"
                        v-model="opcionais[opcional.tipo]">
                        <span>{{ opcional.tipo }}</span>
                    </div>
    
                </div>
                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Criar o meu Burger!">
                </div>
            </form>
        </div>
    </div>
    
</template>

<script>

    import Message from './Message.vue'

    export default {
        name: 'BurgerForm',
        data() {
            return {
                paes: null,
                carnes: null,
                opcionaisData: null,
                nome: null,
                pao: null,
                carne: null,
                opcionais: [],
                msg: null
            }
        },
        methods: {
            async getIngredientes() {
                const req = await fetch('http://localhost:3000/ingredientes')
                const data = await req.json()

                this.paes = data.paes
                this.carnes = data.carnes
                this.opcionaisData = data.opcionais

                // console.log(data);
            },
            async createBurger(e) {

                e.preventDefault();

                const data = {
                    nome: this.nome,
                    pao: this.pao,
                    carne: this.carne,
                    opcionais: Object.keys(this.opcionais), //Trasformar em array
                    status: "Solicitado",

                }
                const dataJson = JSON.stringify(data);

                const req = await fetch('http://localhost:3000/burgers', {
                    method: "POST",
                    headers: { "Content-Type": "application/json"},
                    body: dataJson
                });

                const res = await req.json();

                // colocar uma mensagem para o sistema
                this.msg = `Pedido № ${res.id} realizado com sucesso!`

                // limpar mensagem
                setTimeout(() => {
                    this.msg = ""; // Limpar a mensagem após 3 segundos
                }, 3000);

                // limpar os dados
                this.nome = "";
                this.pao = "";
                this.carne = "";
                this.opcionais = {};
            }
        },
        mounted() {
            this.getIngredientes()
        },
        components: {
            Message
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
        padding: 5px 15px;
        border-left: 4px solid #FCBA03;
    }

    input, select {
        padding: 5px 10px;
        width: 300px;
    }

    #opcionais-conteiner {
        flex-direction: row;
        flex-wrap: wrap;
    }

    #opicionais-title {
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
        width: auto; /*Para não herdar das outras alturas css*/
    }

    .checkbox-container span {
        margin-left: 6px;
        font-weight: bold;
    }

    .submit-btn {
        background-color: #222;
        color: #FCBA03;
        font-weight: bold;
        font-size: 16px;
        padding: 10px;
        border: 2px solid #222;
        /* margin: 0 auto; */
        cursor: pointer;
        transition: 0.5s;
    }

    .submit-btn:hover {
        background-color: transparent;
        color: #222;
    }

</style>