<template>
    <div class="col-6 col-md-4">
        <div class="card card-info mt-4">
            <div class="card-header">
                {{ stock.name }}
                <small>(Price: {{ stock.price }} | Quantity: {{ stock.quantity }})</small>
            </div>
            <div class="card-body">
                <div>
                    <input
                        type="number"
                        class="form-control"
                        placeholder="Quantity"
                        v-model.number="quantity"
                        :class="{ danger: insufficientQuantity }"
                    >
                </div>
            </div>
            <div class="card-footer">
                <button
                    class="btn btn-info btn-block"
                    @click="sellStock"
                    :disabled="insufficientQuantity || quantity <= 0 || !Number.isInteger(quantity)"
                >
                    {{ insufficientQuantity ? `Not enough stocks` : `Sell` }}
                </button>
            </div>
        </div>
    </div>
</template>

<script>
import {mapActions} from 'vuex';

export default {
    name: `PortfolioStock`,

    props: {
        stock: {
            type: Object,
        },
    },

    data() {
        return {
            quantity: 0,
        }
    },

    computed: {
        insufficientQuantity() {
            return this.quantity > this.stock.quantity;
        },
    },

    methods: {
        ...mapActions({
            placeSellOrder: `sellStock`,
        }),
        sellStock() {
            const order = {
                stockId: this.stock.id,
                stockPrice: this.stock.price,
                quantity: this.quantity,
            };
            this.placeSellOrder(order);
            this.quantity = 0;
        },
    },
}
</script>

<style scoped>
.danger {
    border: 1px solid red;
}
</style>
