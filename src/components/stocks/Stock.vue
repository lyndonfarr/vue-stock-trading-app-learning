<template>
    <div class="col-6 col-md-4">
        <div class="card card-success mt-4">
            <div class="card-header">
                {{ stock.name }}
                <small>(Price: {{ stock.price }})</small>
            </div>
            <div class="card-body">
                <div>
                    <input
                        type="number"
                        class="form-control"
                        placeholder="Quantity"
                        v-model.number="quantity"
                        :class="{ danger: insufficientFunds }"
                    >
                </div>
            </div>
            <div class="card-footer">
                <button
                    class="btn btn-success btn-block"
                    @click="buyStock"
                    :disabled="insufficientFunds || quantity <= 0 || !Number.isInteger(quantity)"
                >
                    {{ insufficientFunds ? `Insufficient Funds` : `Buy` }}
                </button>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    name: `StocksStock`,

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
        funds() {
            return this.$store.getters.funds;
        },
        insufficientFunds() {
            return this.quantity * this.stock.price > this.funds;
        },
    },

    methods: {
        buyStock() {
            const order = {
                stockId: this.stock.id,
                stockPrice: this.stock.price,
                quantity: this.quantity,
            };
            this.$store.dispatch('buyStock', order);
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