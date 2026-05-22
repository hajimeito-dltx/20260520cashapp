<script setup>
import { ref, computed } from 'vue'
import axios from 'axios'

const ID = ref('')
const Name = ref('')
const Date = ref('')
const Amount = ref('')

const dataList = ref([])

const addData = async () => {
  if (!ID.value || isNaN(ID.value)) {
    console.log("IDに数値が入力されていません")
    return
  }

  const param = {
    ID: ID.value,
    Name: Name.value,
    Date: Date.value,
    Amount: Amount.value
  }

  const response = await axios.post(
    'https://m3h-hajime-cashapp.jollyriver-96f47718.japaneast.azurecontainerapps.io/api/INSERT',
    param
  )

  console.log(response.data)
}

const readData = async () => {
  const response = await axios.get(
    'https://m3h-hajime-cashapp.jollyriver-96f47718.japaneast.azurecontainerapps.io/api/SELECT'
  )

  console.log(response.data)
  dataList.value = response.data.List
}





  const totalAmount = computed(() => {
    return dataList.value.reduce((sum, data) => sum + data.Amount, 0);
  });






</script>

<template>
  <v-container>
    <v-row class="text-h3 mb-3 mt-3" justify="center">
      入出金記録を取ろう
    </v-row>

    <v-row class="mb-3 mt-3" align="center" justify="center">
      <v-col cols="4">
        <v-text-field
          class="ml-6 mr-6"
          v-model="ID"
          label="IDを入力"
          placeholder="IDを入力…"
        />
      </v-col>

      <v-col cols="6">
        <v-text-field
          class="ml-6 mr-6"
          v-model="Name"
          label="Nameを入力"
          placeholder="Nameを入力…"
        />
      </v-col>

      <v-col cols="6">
        <v-text-field
          class="ml-6 mr-6"
          v-model="Date"
          label="Dateを入力(YYYY-MM-DD)"
          placeholder="Dateを入力…"
        />
      </v-col>

      <v-col cols="6">
        <v-text-field
          class="ml-6 mr-6"
          v-model="Amount"
          label="金額を入力"
          placeholder="金額を入力…"
        />
      </v-col>
    </v-row>

    <v-row class="mb-3 mt-3" align="center" justify="center">
      入力内容　　ID: {{ ID }}　Name: {{ Name }}　Date: {{ Date }}　金額: {{ Amount }}
      <v-btn
        @click="addData"
        color="indigo"
        class="ml-4"
      >
        DBに登録
      </v-btn>
    </v-row>

    <v-row class="mb-3 mt-3" justify="center">
      <v-btn @click="readData">
        現在の登録内容を表示する
      </v-btn>
    </v-row>





    <v-row
      justify="center
      class="mb-2"
      v-for="(data, index) in dataList"
      :key="index"
    >
      合計金額：　  {{totalAmount}}
    </v-row>






    <v-row
      v-for="(data, index) in dataList"
      :key="index"
      class="mb-2"
      justify="center"
    >
      ID: {{ data.ID }}, Name: {{ data.Name }}, Date: {{ data.Date }}, Amount: {{ data.Amount }}
    </v-row>

    
  </v-container>
</template>
