<template>
  <q-page>
    <div v-for="a in responseData" style="border: 1px solid black">
      <q-btn @click="generateForm(a.id)">Click</q-btn>
      <pre>{{ a.content }}</pre>
    </div>
    <div>
      <q-form @submit="onSubmit" class="q-gutter-md">
        <div v-for="(item, index) in inputFields" :key="index">
          <q-input
            filled
            color="green-8"
            :type="item.type"
            v-model="item.value"
            :label="item.variableName"
            :hint="`Enter ${item.variableName} *`"
            lazy-rules
            :rules="[(val) => (val && val.length > 0) || 'Please type']"
          />
        </div>
        <q-btn type="submit" color="primary" label="Submit" no-caps />
      </q-form>
    </div>
  </q-page>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      responseData: "",
      responsebyid: "",
      inputFields: [],
    };
  },
  mounted() {
    axios
      .get("http://localhost:2001/template/get")
      .then((response) => {
        this.responseData = response.data;
        console.log(response.data);
      })
      .catch((error) => {
        console.error("Error:", error);
      });
  },
  methods: {
    generateForm(id) {
      axios
        .get(`http://localhost:2001/template/get/${id}`)
        .then((response) => {
          // this.inputFields = [];
          const val = { val: "" };
          let obj = JSON.parse(response.data.fields);
          this.inputFields.push(val, obj);
          console.log(this.inputFields);
        })
        .catch((error) => {
          console.error("Error:", error);
        });
    },
  },
};
</script>
