<template>
    <div>
        <Message :msg="msg" v-show="msg"/>
        <div>
            <form id="pizza-form" @submit="createPizza">
                <div class="input-container">
                    <label for="nome">Nome do cliente</label>
                    <input type="text" id="nome" name="name" v-model="nome" placeholder="Digite o seu nome">

                    <label for="endereco">Endereço</label>
                    <input type="text" id="endereco" name="endereco" v-model="endereco" placeholder="Digite o seu endereço">
                </div>

                <div class="input-container">
                    <label for="tamanho">Escolha o tamanho da pizza:</label>
                    <select name="tamanho" id="tamanho" v-model="tamanho">
                        <option value="">Selecione o tamanho</option>
                        <option v-for="tamanho in tamanhos" :key="tamanho.id" :value="tamanho.tipo">
                            {{tamanho.tipo}}
                        </option>
                    </select>
                </div>    

                <div class="input-container">
                    <label for="borda">Escolha o sabor da borda:</label>
                    <select name="borda" id="borda" v-model="borda">
                        <option value="">Selecione a borda</option>
                        <option v-for="borda in bordas" :key="borda.id" :value="borda.tipo">
                            {{borda.tipo}}
                        </option>
                    </select>
                </div>

                <div id="ingredientes-container" class="input-container">
                    <label id="ingredientes-title" for="tipos">Selecione os ingredientes:</label>
                    <div class="checkbox-container" v-for="tipo in tiposingredientes" :key="tipo.id">
                        <input type="checkbox" name="tipos" v-model="tipos" :value="tipo.tipo">
                        <span>{{tipo.tipo}}</span>
                    </div>    
                </div>

                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Finalizar o pedido">
                </div>
            </form>
        </div>
    </div>    
</template>

<script>
import Message from "./Message.vue"

export default {
    name: "PizzaForm",
    data() {
        return {
            tamanhos: null,
            bordas: null,
            tiposingredientes: null,
            nome: null,
            endereco: null,
            tamanho: null,
            borda: null,
            tipos: [],
            msg: null
        }
    },
    methods: {
        async getIngredientes() {
            const req = await fetch("http://localhost:3000/ingredientes");
            const data = await req.json();

            this.tamanhos = data.tamanhos;
            this.bordas = data.bordas;
            this.tiposingredientes = data.tipos;
        },
        async createPizza(e) {
            e.preventDefault();

            const data = {
                nome: this.nome,
                endereco: this.endereco,
                tamanho: this.tamanho,
                borda: this.borda,
                tipos: Array.from(this.tipos),
                status: "Solicitado"
            }

            const dataJson = JSON.stringify(data);

            const req = await fetch("http://localhost:3000/pizzas", {
                method: "POST",
                headers: {"Content-Type": "application/json"},
                body: dataJson
            })

            const res = await req.json();
            
            //resposta ao cliente
            this.msg = `Pedido Nº ${res.id} realizado com sucesso!`;

            //limpar resposta depois de alguns segundos
            setTimeout(() => this.msg = "", 5000);

            //limpar os campos
            this.nome = "";
            this.endereco = "";
            this.tamanho = "";
            this.borda = "";
            this.tipos = "";
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
    #pizza-form {
        max-width: 400px;
        margin: 0 auto;
    }

    .input-container {
        display: flex;
        flex-direction: column;
        margin-bottom: 10px;
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

    #ingredientes-container {
        flex-direction: row;
        flex-wrap: wrap;
    }

    #ingredientes-title {
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
        margin-left: 5px;
        font-weight: bold;
    }

    .submit-btn {
        background-color: #222;
        color: #FCBA03;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 16px;
        cursor: pointer;
        transition: 1s;
    }

    .submit-btn:hover {
        background-color: transparent;
        color: #222;
    }

</style>