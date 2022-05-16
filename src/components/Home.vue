<template>
  <div style="display: flex; justify-content: center">
    <div style="width: 80%">
      <h1>貨號!!</h1>
      <div style="display: flex">
        <el-input
          v-model="input.id"
          type="tel"
          size="large"
          placeholder="請輸入貨號"
        />
        <el-button
          type="primary"
          size="large"
          style="margin-left: 10px;"
          :loading="loading"
          @click="queryById"
        >
          查詢
        </el-button>
      </div>
      <div style="margin-top: 40px">
        <el-input
          v-model="input.name"
          type="textarea"
          :autosize="{ minRows: 5}"
          placeholder="請輸入描述"
        />
      </div>
      <div style="margin-top: 40px">
        <el-button
          type="primary"
          size="large"
          style="width: 100%"
          :loading="loading"
          :disabled="!input.id || !input.name"
          @click="submit"
        >
          送出
        </el-button>
      </div>
    </div>

  </div>
</template>

<script>
import { ref } from "vue";
import axios from "axios";

export default {
  name: "Home",
  setup() {
    const input = ref({
      id: '',
      name: '',
      row: undefined,
      operator: ''
    })
    const loading = ref(false)

    function submit() {
      const value = input.value
      console.log("value", value)
      loading.value = true
      axios.post(process.env.VUE_APP_BASE_URL, {
        type: value.row ? 'update' : 'create',
        id: value.id,
        name: value.name,
        row: value.row,
        operator: 'amber' // TODO
      }, {
        headers: {
          'Content-Type': 'text/plain;charset=utf-8',
        }
      })
        .then(function (response) {
          console.log(response);
        })
        .catch(function (error) {
          console.log(error);
        })
        .finally(() => {
          loading.value = false
        })
    }

    function queryById() {
      loading.value = true
      axios.post(process.env.VUE_APP_BASE_URL, {
        type: 'query',
        id: input.value.id
      }, {
        headers: {
          'Content-Type': 'text/plain;charset=utf-8',
        }
      })
        .then(function (response) {
          input.value = {
            id: input.value.id,
            name: response.data.name,
            row: response.data.row,
          }
        })
        .catch(function (error) {
          console.log(error);
        })
        .finally(() => {
          loading.value = false
        })
    }

    return {
      input,
      submit,
      queryById,
      loading,
    };
  },
}
</script>
