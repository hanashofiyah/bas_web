<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import { inject, reactive } from 'vue';
import router from '@/router/index';
import { VNumberInput } from 'vuetify/labs/VNumberInput'

const data = reactive({
    accountID: '',
    bankID: '',
    amount: '',
    transactionDate: '',
    snackbar: false,
    pesanTransfer: ''
})

const myAxios = inject('myAxios')

const transfer = () => {
    console.log('transfer clicked', data);
    
    myAxios.post('/transaction/transfer-bank', {
        accountID: data.accountID,
        bankID: data.bankID,
        amount: data.amount,
        transactionDate: data.transactionDate
    }).then((res) => {
        if (res.status == 200){
            data.pesanTransfer = "Anda Berhasil Transfer"
            router.push('transfer')
        }
        data.snackbar = true
    }, () => {
        data.pesanTransfer = "Anda Gagal Transfer"
        data.snackbar = true
    })
}
</script>

<template>
  <div class="container">
    <div>
          <label>Bank ID</label>
          <v-text-field type="text" v-model="data.bankID"></v-text-field>
      </div>
      <div>
          <label>Account ID</label>
          <v-text-field type="text" v-model="data.accountID"></v-text-field>
      </div>
      <div>
          <label>Amount</label>
          <v-number-input v-model="data.amount"></v-number-input>
      </div>
      <div>
          <label>Transaction Date</label>
          <v-text-field type="text" v-model="data.transactionDate"></v-text-field>
      </div>
      <v-card class="pa-5">
          <div>
              <v-btn color="blue" @click="transfer">
                  Transfer
              </v-btn>
          </div>
      </v-card>
      <v-snackbar v-model="data.snackbar">
          {{ data.pesanTransfer }}

          <template v-slot:actions>
              <v-btn color="blue" variant="text" @click="snackbar = false">
              Close
              </v-btn>
          </template>
      </v-snackbar>
  </div>
</template>