<script>
export default {
    props: {
        cart: Object, //singolo ordine
        plate: Object,
        keyO: Number,
    },
    methods: {
        remove() {
            let cart = JSON.parse(localStorage.getItem("cart"))
            cart[this.keyO].quantit = 0
            localStorage.setItem("cart", JSON.stringify(cart))
            this.cart.quantit = 0
        },
        modPlate(sign) {
            let cart = JSON.parse(localStorage.getItem("cart"))
            if (sign == "-") {
                let quantit = this.cart.quantit
                quantit--
                if (quantit <= -1) {
                    cart[this.keyO].quantit = 0
                    this.cart.quantit = 0
                } else {
                    cart[this.keyO].quantit--
                    this.cart.quantit--
                }
            } else {
                cart[this.keyO].quantit++
                this.cart.quantit++
            }
            localStorage.setItem('cart', JSON.stringify(cart))
        },
    },
}
</script>
<template>
    <template v-if="cart.quantit !== 0">
        <div class="d-flex flex-row gap-5">
            <div class="col-5">
                {{ plate.name }}
            </div>
            <div class="col-2">
                € {{ Math.round(plate.price / 100).toFixed(2) }}
            </div>
            <div class="col-2">
                <div class="col-md-4 add">
                    <div class="d-flex justify-content-center align-items-center switch-dock">
                        <div class="d-flex justify-content-center align-items-center joycon-left"
                            @click="modPlate(`-`)">
                            -
                        </div>
                        <div class="d-flex justify-content-center align-items-center switch-screen">
                            {{ cart.quantit }}
                        </div>
                        <div class="d-flex justify-content-center align-items-center joycon-right"
                            @click="modPlate(`+`)">
                            +
                        </div>
                    </div>
                </div>
            </div>
            <div class="col-2">
            {{ Math.round((plate.price * cart.quantit) / 100).toFixed(2) }}
            </div>
            <div class="col-1 text-danger" @click="remove()" style="font-weight: bold;">
                X
            </div>
        </div>
    </template>
</template>
<style lang="scss">
.switch-dock {
    stroke-width: 1px;
    border: 1px solid #37363D;
    width: 92px;
    height: 52px;
    border-radius: 5px;
}

.joycon-left {
    width: 20px;
    height: 40px;
    border-radius: 5px 0px 0px 5px;
    background: #F9F7ED;
    animation-fill-mode: forwards;
    animation-play-state: paused;

    &:active {
        animation-play-state: running;
    }
}

@keyframes color-change-left {
    0% {
        background-color: #F9F7ED;
    }

    50% {
        background-color: #00c3e3;
    }

    100% {
        background-color: #F9F7ED;
    }
}

.joycon-right {
    width: 20px;
    height: 40px;
    border-radius: 0px 5px 5px 0px;
    background: #F9F7ED;
    animation: color-change-right 0.1s;
    animation-play-state: paused;

    &:active {
        animation-play-state: running;
    }
}

@keyframes color-change-right {
    0% {
        background-color: #F9F7ED;
    }

    50% {
        background-color: #ff4554;
    }

    100% {
        background-color: #F9F7ED;
    }
}

.switch-screen {
    width: 40px;
}</style>