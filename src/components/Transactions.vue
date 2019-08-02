<template>
    <v-container>
        <v-layout wrap row>
            <v-flex xs12>
                <v-data-table class="transactions"
                    :headers="headers"
                    :items="transactions"
                    hide-default-footer
                    show-expand
                    :loading="loading"
                    item-key="id">
                    <template v-slot:top>
                        <v-toolbar flat color="white">
                            <v-toolbar-title class="hidden-sm-and-down">Transactions</v-toolbar-title>
                            <v-spacer></v-spacer>
                            <v-select
                            color="#213d8f" item-color="#213d8f"
                            :items="actions"
                            v-model="action"
                            class="mt-10 mr-2"
                            label="Action" outlined solo></v-select>
                            <v-select
                            color="#213d8f" item-color="#213d8f"
                            :items="currencies"
                            v-model="currencyCode"
                            class="mt-10 mr-2"
                            label="Currency" outlined solo></v-select>
                            <v-btn class="mt-2" large color="#8ec03f" @click="loadTransactions">Search</v-btn>
                        </v-toolbar>
                    </template>
                    <template v-slot:expanded-item="{ item, headers }">
                        <td :colspan="headers.length">
                            <v-container>
                                <v-layout wrap>
                                    <v-flex xs12 sm6>
                                        <v-container>
                                            <v-layout>
                                                <v-flex xs6><span class="details-header">ID:</span></v-flex>
                                                <v-flex xs6><span class="details-text">{{item.id}}</span></v-flex>
                                            </v-layout>
                                        </v-container>
                                    </v-flex>
                                    <v-flex xs12 sm6>
                                        <v-container>
                                            <v-layout>
                                                <v-flex xs6><span class="details-header">First 6 digits:</span></v-flex>
                                                <v-flex xs6><span class="details-text">{{item.card.firstSixDigits}} XXXX</span></v-flex>
                                            </v-layout>
                                        </v-container>
                                    </v-flex>
                                    <v-flex xs12 sm6>
                                        <v-container>
                                            <v-layout>
                                                <v-flex xs6><span class="details-header">Tracking code:</span></v-flex>
                                                <v-flex xs6><span class="details-text">{{item.trackingCode}}</span></v-flex>
                                            </v-layout>
                                        </v-container>
                                    </v-flex>
                                    <v-flex xs12 sm6>
                                        <v-container>
                                            <v-layout>
                                                <v-flex xs6><span class="details-header">Expiry month:</span></v-flex>
                                                <v-flex xs6><span class="details-text">{{item.card.expiryMonth}}</span></v-flex>
                                            </v-layout>
                                        </v-container>
                                    </v-flex>
                                    <v-flex xs12 sm6>
                                        <v-container>
                                            <v-layout>
                                                <v-flex xs6><span class="details-header">Brand ID:</span></v-flex>
                                                <v-flex xs6><span class="details-text">{{item.brandId}}</span></v-flex>
                                            </v-layout>
                                        </v-container>
                                    </v-flex>
                                    <v-flex xs12 sm6>
                                        <v-container>
                                            <v-layout>
                                                <v-flex xs6><span class="details-header">Expiry year:</span></v-flex>
                                                <v-flex xs6><span class="details-text">{{item.card.expiryYear}}</span></v-flex>
                                            </v-layout>
                                        </v-container>
                                    </v-flex>
                                </v-layout>
                            </v-container>
                        </td>
                    </template>
                </v-data-table>
            </v-flex>
        </v-layout>
    </v-container>
</template>

<style lang="scss">
.transactions {
    th {
        span {
            font-family: 'Source Sans Pro', sans-serif;
            font-size: 14px;
            color: #213d8f;
            font-weight: 600;
        }
    }
    td {
        span {
            font-family: 'Source Sans Pro', sans-serif;
            font-size: 14px;
            font-weight: 400;
        }
    }
    tr.expanded.expanded__content {
        box-shadow: none !important;
        background-color: #e8ebf3;
    }
}
.details-header {
    font-family: 'Source Sans Pro', sans-serif;
    font-size: 16px;
    color: #213d8f;
    font-weight: 400;
    line-height: 25px;
}
.details-text {
    font-family: 'Source Sans Pro', sans-serif;
    font-size: 16px;
    font-weight: 400;
    line-height: 25px;
}
.expended-content {
    box-shadow: none;
}
</style>


<script lang="ts">
import Vue from "vue";

import axios from "axios";

export default Vue.extend({
    data: function() {
        return {
            loading: false,
            currencies: [{text: "None", value: ""}, "EUR", "USD", "JPY"],
            actions: [
                {
                    text: "None",
                    value: ""
                },
                {
                    text: "Payment", 
                    value: "payment"
                }, 
                {
                    text: "Credit", 
                    value: "credit"
                }
            ],
            currencyCode: null,
            action: null,
            headers: [
                {
                    text: "Name",
                    value: "card.holderName",
                    sortable: false,
                    align: "left"
                },
                {
                    text: "Last 4 Digits",
                    value: "card.lastFourDigits",
                    sortable: false,
                    align: "center"
                },
                {
                    text: "Action",
                    value: "action",
                    sortable: false,
                    align: "center"
                },
                {
                    text: "Amount",
                    value: "amount",
                    align: "right"
                },
                {
                    text: "Currency",
                    value: "currencyCode",
                    align: "center"
                }
            ],
            transactions: []
        };
    },
    methods: {
        loadTransactions: function () {
            this.loading = true;
            axios
            .get("/transactions", { 
                params: {
                    currencyCode: this.currencyCode ? this.currencyCode : undefined,
                    action: this.action ? this.action : undefined
                }
             })
            .then(response => {
                this.loading = false;
                if (response.status === 200) {
                    this.transactions = response.data;
                } else {
                    console.error(response.data);
                }
            })
            .catch(error => {
                console.error(error);
                this.loading = false;
            });
        }
    },
    mounted: function() {
    }
});
</script>

