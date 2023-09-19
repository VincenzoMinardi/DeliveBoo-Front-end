<script>
import axios from "axios";

export default {
    data() {
        return {
            plates: [],
            cart: [],
        };
    },
    methods: {
        addPlate(id) {
            let cart = localStorage.getItem("cart")
            let quant = document.getElementById(id).value
            document.querySelectorAll(".text-danger").forEach(element => {
                element.innerHTML = ""
            })
            if (quant <= 0 || quant == null) {
                document.getElementById(`error`+id).innerHTML = 'valore minimo richiesto "1"'
            } else {
                if (cart !== null) {
                    if (cart.includes('{"id":"'+ id) == true) {
                        let obj = JSON.parse(cart)
                        obj.forEach(element => {
                           if(element.id == id) {
                            console.log(element.quant);
                            element.quant = quant
                           } 
                        });
                        console.log(obj);
                        // .replace('{"id":"'+ id + '", "quant":"'+ quant +'"}', '{"id":"'+ id + '", "quant":"'+ quantChange +'"}')
                        console.log("oggetto aggiunto 1");
                    } else {    
                        cart = cart.replace("]", ",")
                        cart += '{"id":"'+ id + '", "quant":"'+ quant +'"}]'

            localStorage.setItem("cart", cart);

                        console.log("oggetto aggiunto 2");
                    }
                } else {
                    let cart = '[{"id":"'+ id + '", "quant":"'+ quant +'"}]'
                    localStorage.setItem("cart", cart)
                    console.log("oggetto creato 3");
                }
                this.cart = JSON.parse(cart)    
            }
            
        },
        destroyStorage() {
            localStorage.clear()
            console.log("Sono Morte, il distruttore di mondi");
        },
        getData() {
            axios.get("http://127.0.0.1:8000/api/data")
                .then((response) => {
                    this.plates = response.data.plates
                    console.log("fatto");
                })
        }
    },
    created (){
        this.getData()
    }
}
</script>
<template>
    <button @click="destroyStorage()" style="background-color: red; width: 100px; height: 50px; color: aliceblue;">Nuke</button>
    <div v-for="plate in plates">
        <div class="d-flex">
            <input type="number" :name="plate.id" :id="plate.id">
            <div style="width:30px; height: 30px; background-color: black; margin: 5px;" @click="addPlate(plate.id)"></div>
            <span class="text-danger" style="font-weight: bold;" :id="`error${ plate.id }`"></span>
        </div>
        
    </div>
</template>
<style lang="scss"></style>
