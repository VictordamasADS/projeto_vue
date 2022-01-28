<template>
    <div id="pizza-table">
        <Message :msg="msg" v-show="msg"/>
        <div>
            <div id="pizza-table-heading">
                <div class="order-id">#</div>
                <div>Cliente:</div>
                <div>Endereço:</div>
                <div>Tamanho:</div>
                <div>Borda:</div>
                <div>Ingredientes:</div>
                <div>Ações:</div>
            </div>
        </div>

        <div id="pizza-table-rows">
            <div class="pizza-table-row" v-for="pizza in pizzas" :key="pizza.id">
                <div class="order-number">{{pizza.id}}</div>
                <div>{{pizza.nome}}</div>
                <div>{{pizza.endereco}}</div>
                <div>{{pizza.tamanho}}</div>
                <div>{{pizza.borda}}</div>
                <div>
                    <ul>
                        <li v-for="(tipo, index) in pizza.tipos" :key="index">{{tipo}}</li>
                    </ul>
                </div>
                <div>
                    <select name="status" class="status" @change="updatedPizza($event, pizza.id)">
                        <option value="">Selecione</option>
                        <option v-for="s in status" :key="s.id" :value="s.tipo" :selected="pizza.status == s.tipo">
                            {{s.tipo}}
                        </option>
                    </select>
                    <button class="delete-btn" @click="deletePizza(pizza.id)">Cancelar</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Message from "./Message.vue"

export default {
    name: "Dashboard",
    data() {
        return {
            pizzas: null,
            pizzas_id: null,
            status: [],
            msg: null
        }
    },
    components: {
        Message
    },
    methods: {

        async getPedidos() {
            const req = await fetch("http://localhost:3000/pizzas");

            const data = await req.json();

            this.pizzas = data;

            // resgatar os status
            this.getStatus();
        },
        async getStatus() {
            const req = await fetch(" http://localhost:3000/status");

            const data = await req.json()

            this.status = data;
        },
        async deletePizza(id) {
            const req = await fetch(`http://localhost:3000/pizzas/${id}`, {
                method: "DELETE"
            });

            const res = await req.json();

            //resposta ao cliente
            this.msg = `O Pedido foi removido com sucesso.`;

            //limpar resposta depois de alguns segundos
            setTimeout(() => this.msg = "", 5000);


            this.getPedidos();
        },
        async updatedPizza(event, id) {
            const option = event.target.value;
            const dataJson = JSON.stringify({ status: option })
            const req = await fetch(`http://localhost:3000/pizzas/${id}`, {
                method: "PATCH",
                headers: { "Content-Type": "application/json" },
                body: dataJson
            })

            const res = await req.json()  
            
            this.msg = `O Status do pedido Nº ${res.id} foi atualizado para ${res.status}.`;

            setTimeout(() => this.msg = "", 5000);

        }   
    },
    mounted() {
        this.getPedidos();
    }
}
</script>

<style scoped>
    #pizza-table {
        max-width: 1700px;
        margin: 0 auto;
    }

    #pizza-table-heading,
    #pizza-table-rows,
    .pizza-table-row {
        display: flex;
        flex-wrap: wrap;
    }

    #pizza-table-heading {
        font-weight: bold;
        padding: 12px;
        border-bottom: 3px solid #333;
    }

    #pizza-table-heading div,
    .pizza-table-row div {
        width: 15%;
    }

    .pizza-table-row {
        width: 100%;
        padding: 12px;
        border: 1px solid #CCC;
    }

    #pizza-table-heading .order-id,
    .pizza-table-row .order-number {
        width: 5%;
    }

    select {
        padding: 12px 6px;
        margin-right: 12px;
    }

    .delete-btn {
        background-color: #222;
        color: #FCBA03;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 16px;
        margin: 0 auto;
        cursor: pointer;
        transition: 1s;
    }

    .delete-btn:hover {
        background-color: transparent;
        color: #222;
    }

</style>