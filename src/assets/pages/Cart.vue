<script>
import AppOrder from "../components/appOrder.vue"
import axios from 'axios'

export default {
    data() {
        return {
            cartFull: false,
            arrCart: [],
            plates: [],
            name: "", 
            lastname: "",
            email: "",
            address: "",
            cp: "",
            phone: "",
            date: "",
            card_number: "",
            cvv: "",
            card_name: "",
            prova: "",
            isPayment: 0
        }
    },
    methods: {
        searchData() {
            console.log("siamo dentro");
            if(localStorage.getItem("cart") !== null) {
                this.cartFull = true;

                let cart = JSON.parse(localStorage.getItem("cart"))
                this.arrCart = cart
            }
        },
        getData() {
            axios.get("http://127.0.0.1:8000/api/data")
                .then((response) => {
                    this.plates = response.data.plates
                })
        },
        sendPayment() {
            let platesForm = []
            let priceAll = null
            let res_id = this.arrCart[0].res_id
            this.arrCart.forEach(element => {
                if (element.quantit !== 0) {
                    platesForm.push(element)
                }
            });
            platesForm.forEach(element => {
                this.plates.forEach(plate => {
                    if (plate.id == element.id) {
                        priceAll += parseInt(element.quantit) * parseInt(plate.price)
                    }
                });
                
            });
            axios
                .post("http://localhost:8000/api/orders/makePayment", {
                    name: this.name, 
                    lastname: this.lastname,
                    email: this.email,
                    address: this.address,
                    pc: this.cp,
                    phone: this.phone,
                    date: this.date,
                    card_number: this.card_number,
                    cvv: this.cvv,
                    card_name: this.card_name,
                    plates: platesForm,
                    price: priceAll,
                    res_id: res_id
                })
                .then((response) => {
                    console.log(response);
                    if (response.status == 200) {
                        this.isPayment = 1
                        localStorage.clear()
                        this.name = "" 
                        this.lastname = ""
                        this.email = ""
                        this.address = ""
                        this.cp = ""
                        this.phone = ""
                        this.date = ""
                        this.card_number = ""
                        this.cvv = ""
                        this.card_name = ""
                        platesForm = ""
                        priceAll = ""
                        res_id = ""
                        this.arrCart = []
                    } else {
                        this.isPayment = 2
                    }
                })
                .catch((error) => {
                    console.log(error)
                })
        }
    },
    created () {
        this.searchData();
        this.getData()
    },
    components: {
        AppOrder,
    }
}

</script>

<template>
    <template v-if="cartFull == false">
        
    </template>
    <div class="alert alert-success my-3" v-if="isPayment == 1">
            Il pagamento è stato inviato con successo.
        </div>
        <div class="alert alert-success my-3" v-if="isPayment == 2">
            Il pagamento è stato inviato con successo.
        </div>
    <template v-else>
        <div class="container dis d-flex justify-content-between">
            <div class="cont-cart">
                <div class="d-flex flex-row  gap-5">
                    <div class="plate fs-4 fw-bold col-5">Piatti</div>
                    <div class="fs-4 col-2 fw-bold">Prezzo</div>
                    <div class="fs-4 col-2 fw-bold">Quantità</div>
                    <div class="fs-4 col-2 fw-bold">Totale</div>
                    <div class="fs-4 col-1 fw-bold">X</div>
                </div>
                <AppOrder v-for="cart, key in arrCart" :cart="cart" :plate="plates[cart.id - 1]" :keyO="key"/>

                
            </div>
        </div>
        <form @submit.prevent="sendPayment" class="d-flex flex-column container-sm">
        <label for="name">Nome</label>
        <input type="text" id="name" v-model="name">
        <label for="lastname">Cognome</label>
        <input type="text" id="lastname" v-model="lastname">
        <label for="email">email</label>
        <input type="email" id="email" v-model="email">
        <label for="address">indirizzo</label>
        <input type="text" id="address" v-model="address">
        <label for="pc">Cap. Postale</label>
        <input type="number" id="pc" v-model="cp">
        <label for="phone">Numero di telefono</label>
        <input type="text" id="phone" v-model="phone">
        <label for="date">data di consegna</label>
        <input type="date" id="date" v-model="date">
        <label for="card_number">numero sulla carta</label>
        <input type="number" id="card_number" v-model="card_number">
        <label for="cvv">CVV</label>
        <input type="number" id="cvv" v-model="cvv">
        <label for="card_name">Nome sulla Carta</label>
        <input type="text" id="card_name" v-model="card_name">
        <button type="submit">Invia</button>
    </form>
    </template>
</template>

<style lang="scss">
.dis {
    margin-top: 2rem;
    background-color: rgb(231, 165, 80);
    border-radius: 1rem;

}

.cont-cart {
    margin-left: 1rem;

}

.plate {
    padding-inline: 3rem;
}

</style>