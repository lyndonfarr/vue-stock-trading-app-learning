<template>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <router-link class="navbar-brand" to="/">Stock Trader</router-link>

        <div class="collapse navbar-collapse" id="navbarSupportedContent">
            <ul class="navbar-nav mr-auto">
                <router-link
                    class="nav-item"
                    to="/portfolio"
                    activeClass="active"
                    tag="li"
                >
                    <a class="nav-link">Portfolio</a>
                </router-link>
                <router-link
                    class="nav-item"
                    to="/stocks"
                    activeClass="active"
                    tag="li"
                >
                    <a class="nav-link">Stocks</a>
                </router-link>
            </ul>
            <strong>Funds: {{ funds | currency }}</strong>
            <ul class="navbar-nav ml-auto">
                <li class="nav-item"><a class="nav-link" href="#" @click="endDay">End Day</a></li>
                <li class="nav-item dropdown">
                    <a
                        class="nav-link dropdown-toggle"
                        href="#"
                        id="navbarDropdown"
                        role="button"
                        data-toggle="dropdown"
                        aria-haspopup="true"
                        aria-expanded="false"
                    >
                        Save &amp; Load
                    </a>
                    <div class="dropdown-menu" aria-labelledby="navbarDropdown">
                        <a
                            class="dropdown-item"
                            href="#"
                            @click="saveData"
                        >
                            Save Data
                        </a>
                        <a
                            class="dropdown-item"
                            href="#"
                            @click="loadData"
                        >
                            Load Data
                        </a>
                    </div>
                </li>
            </ul>
        </div>
    </nav>
</template>

<script>
import { mapActions } from 'vuex';

export default {
    computed: {
        funds() {
            return this.$store.getters.funds;
        },
    },

    methods: {
        ...mapActions({
            randomiseStocks: `randomiseStocks`,
            fetchData: `loadData`,
        }),
        endDay() {
            this.randomiseStocks();
        },
        saveData() {
            const data = {
                funds: this.$store.getters.funds,
                stockPortfolio: this.$store.getters.stockPortfolio,
                stocks: this.$store.getters.stocks,
            };
            this.$http.put(`data.json`, data);
        },
        loadData() {
            this.fetchData();
        },
    },
}
</script>
