<template>
    <div>
        <MessadeVue :msg="msg" v-show="showMsg"/>
        <div>
            <form action="" id="burger-form" @submit="creatBurger">
                <div class=" input-container">
                    <label for="name">Nome do cliente:</label>
                    <input type="text" id="name" name="name" v-model="name" placeholder="Digite seu nome">
                </div>
                <div class="input-container">
                    <label for="bread">Escolha o pão:</label>
                    <select name="bread" id="bread" v-model="bread">
                        <opiton value="">Selecione o seu pão</opiton>
                        <option v-for="bread in breads" :key="bread.id" :value="bread.tipo">{{ bread.tipo }}</option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="meat">Escolha a carne do seu burger:</label>
                    <select name="meat" id="meat" v-model="meat">
                        <opiton value="">Selecione o tipo de carne</opiton>
                        <option v-for="meat in meats" :key="meat.id" :value="meat.tipo">{{ meat.tipo }}</option>
                    </select>
                </div>
                <div id="opcionais-container" class="input container">
                    <label id="opcionais-title" for="opcionais">Selecione os opcionais:</label>
                    <div class="checkbox-container" v-for="opcional in opcaodata" :key="opcional.id">
                        <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
                        <span>{{ opcional.tipo }}</span>
                    </div>
                </div>
                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Enviar">
                </div>
            </form>
        </div>
    </div>
</template>
<script>
import MessadeVue from './Messade.vue'

export default {
    name: "Formulario",
    components: {
        MessadeVue
    },
    data() {
        return {
            breads: null,
            meats: null,
            opcaodata: null,
            name: null,
            bread: null,
            meat: null,
            opcionais: [],
            status: "Solicitado",
            msg: null,
            showMsg: false
        }
    },
    methods: {
        async getIngredients() {
            const req = await fetch("http://localhost:3000/ingredientes")
            const data = await req.json()

            this.breads = data.paes
            this.meats = data.carnes
            this.opcaodata = data.opcionais
        },
        async creatBurger(e) {
            e.preventDefault()
            const data = {
                name: this.name,
                bread: this.bread,
                meat: this.meat,
                opcionais: Array.from(this.opcionais),
                status: "Solicitado",
            }
            const dataJson = JSON.stringify(data)

            const req = await fetch("http://localhost:3000/burgers", {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: dataJson
            })

            const res = await req.json()

            this.showMsg = true
            this.msg = `Pedido n° ${res.id} realizado com sucesso`

            setTimeout( () => this.showMsg = false, 3000)

            this.name = ""
            this.meat = ""
            this.bread = ""
            this.opcionais = ""
            //console.log(res)
        }
    },
    mounted() {
        this.getIngredients()
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

input,
select {
    padding: 5px 10px;
    width: 300px;
}

#opcionais-container {
    display: flex;
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
    margin-top: 10px;
    margin-bottom: 10px;
}

.checkbox-container span,
.checkbox-container input {
    width: auto;
    /* margin-top: 10px; */
}

.checkbox-container span {
    margin-left: 6px;
    font-weight: bold;
}

.submit-btn {
    background-color: #222;
    color: #FBCA03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
}

.submit-btn:hover {
    background-color: transparent;
    color: #222;
}
</style>