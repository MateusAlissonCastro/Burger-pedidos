<template>
    <Messagem :msg="msg" v-show="showMsg"/>
    <div id="burger-table">
        <div>
            <div id="burger-table-heading">
                <div class="order-id">#</div>
                <div>Cliente:</div>
                <div>Pão:</div>
                <div>Carne:</div>
                <div>Opcionais:</div>
                <div>Ações:</div>
            </div>
        </div>
        <div id="burger-table-rows" v-for="burger in burgers" :key="burger.id">
            <div class="burger-table-row">
                <div class="order-number">{{ burger.id }}</div>
                <div>{{burger.name}}</div>
                <div>{{burger.bread}}</div>
                <div>{{burger.meat}}</div>
                <div>
                    <ul>
                        <li v-for="opcional, index in burger.opcionais" :key="index">{{opcional}}</li>
                    </ul>
                </div>
                <div>
                    <select name="status" class="satus" @change="updateStatus($event, burger.id)">
                        <option v-for="s, index in status" :value="s" :key="index" :selected="burger.status == s">{{ s }}</option>
                    </select>
                    <button class="delete-btn" @click="cancelarBurger(burger.id)">Cancelar</button>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import Messagem from './Messade.vue'
export default{
    name: "Dashboard",
    components: {
        Messagem
    },
    data(){
        return{
            burgers: null,
            burgers_id: null,
            status: ["Solicitado", "Em produção", "Finalizado"],
            msg: null,
            showMsg: false
        }
    },
    methods: {
        async getPedidos(){
            const req = await fetch('http://localhost:3000/burgers')

            const myburges = await req.json()

            this.burgers = myburges            
        },
        async cancelarBurger(id) {
            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: 'DELETE'
            })

            const res = await req.json()

            this.msg = `Pedido cancelado com sucesso`
            this.showMsg = true

            setTimeout( () => this.showMsg = false, 3000)

            this.getPedidos()
        },
        async updateStatus(event, id){
            const option  =  event.target.value
            const dataJson = JSON.stringify({status:option})
            const req = await fetch(`http://localhost:3000/burgers/${id}`,{
                method: "PATCH",
                headers: {"Content-Type":"application/json"},
                body: dataJson
            })
            const res = await req.json()

            this.msg = `O pedido n°${res.id} foi atualizado para ${res.status}`
            this.showMsg = true

            setTimeout( () => this.showMsg = false, 3000)

            this.getPedidos()

            console.log(res)
        }
    },
    mounted() {
        this.getPedidos()
    }
}
</script>
<style>
#burger-table{
    max-width: 1200px;
    margin: 0 auto;
}
#burger-table-heading,
#burger-table-rows,
.burger-table-row{
    display: flex;
    flex-wrap: wrap;
}
#burger-table-heading{
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
}
#burger-table-heading div,
.burger-table-row div{
    width: 19%;
}
.burger-table-row{
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid #CCC;
}
#burger-table-heading .order-id,
.burger-table-row .order-number{
    width: 5%;
}
select{
    padding: 12px 6px;
    margin-right: 12px;
}
.delete-btn{
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
.delete-btn:hover{
background-color: transparent;
color: #222;
}
</style>