<template>
  <q-page class="flex flex-center">
    <div>
      <div class="row justify-center q-pa-xl" v-if="showCreate">
        <div class="flex">
          <div class="col-md-4 col-sm-6 col-12 q-px-lg">
            <q-input
              filled
              style="width: 250px"
              label="Variable name *"
              hint="Variable name"
              color="blue-grey"
              v-model="variableName"
              lazy-rules
              :rules="[(val) => (val && val.length > 0) || 'Please type ...']"
            />
          </div>

          <div class="col-md-4 col-sm-6 col-12 q-px-lg">
            <q-select
              filled
              style="width: 250px"
              color="green-8"
              label="Select Type *"
              hint="Select type"
              emit-value
              map-options
              clearable
              lazy-rules
              v-model="selectedType"
              :options="options"
              :rules="[(val) => (val && val.length > 0) || 'Please select ...']"
            />
          </div>

          <div class="col-md-4 col-sm-6 col-12 q-px-lg q-pa-sm">
            <q-btn
              color="blue-grey"
              label="Add"
              icon="add"
              @click="addInputField"
            />
          </div>
        </div>

        <div class="col-12 q-pa-xl">
          <q-editor
            v-model="inputText"
            :options="{
              minHeight: '500px',
              maxHeight: '1000px',
              toolbar: true,
            }"
          />
        </div>
        <div class="row text-center q-mt-xl">
          <div class="col q-px-xl">
            <q-input
              filled
              style="width: 250px"
              label="Template Name *"
              hint="Template name"
              color="blue-grey"
              v-model="tempname"
              lazy-rules
              :rules="[(val) => (val && val.length > 0) || 'Please type ...']"
            />
          </div>
          <div class="col q-px-xl q-mt-sm">
            <q-btn
              color="blue-grey"
              label="Generate"
              @click="generateTemplate"
            />
          </div>
        </div>
      </div>

      <div class="row justify-content-center q-pa-xl" v-if="showForm">
        <div class="flex justify-center">
          <div class="col-md-3 col-sm-6 col-12 q-px-lg">
            <q-btn
              color="blue-grey"
              no-caps
              icon="arrow_back"
              label="back"
              @click="backtoTemplates"
            />
            <div class="q-mt-xl">
              <q-form @submit="onSubmit" class="q-gutter-md">
                <div v-for="(item, index) in inputFields" :key="index">
                  <q-input
                    filled
                    color="blue-grey"
                    style="width: 250px"
                    :type="item.type"
                    v-model="item.value"
                    :label="item.variableName"
                    :hint="`Enter ${item.variableName} *`"
                    lazy-rules
                    :rules="[
                      (val) => (val && val.length > 0) || 'Please type name',
                    ]"
                  />
                </div>
                <q-btn type="submit" color="blue-grey" label="Submit" no-caps />
              </q-form>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div v-if="showTemplate">
      <pre>{{ inputText }}</pre>
      <div v-html="formattedHTML"></div>
    </div>
  </q-page>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      // fieldItems: [{ name: "" }, { course_name: "" }],
      formattedHTML: "",
      letterTemplate: null,
      showTemplate: false,
      showCreate: true,
      showForm: false,
      selectedType: "",
      variableName: "",
      tempname: "",
      inputText: "",
      inputFields: [],
      options: [
        "text",
        "number",
        "email",
        "datetime-local",
        "date",
        "file",
        "image",
        "password",
        "tel",
        "time",
        "url",
      ],
    };
  },
  methods: {
    addInputField() {
      if (this.variableName == "" && this.selectedType == "") {
        alert("Fields required");
      } else {
        const newItem = {
          type: this.selectedType,
          variableName: this.variableName,
          value: this.value,
        };
        this.inputFields.push(newItem);
        console.log(this.inputFields);
        const newInput = `{{${newItem.variableName}}}`;
        this.inputText += newInput;
        this.selectedType = null;
        this.variableName = null;
      }
    },
    generateTemplate() {
      if (this.inputText == "") {
        this.showCreate = true;
        alert("Create the template...");
      } else {
        const htmlContent = `<pre>${this.inputText}</pre>`;
        const data = {
          templatename: this.tempname,
          content: this.inputText,
          fields: JSON.stringify(this.inputFields),
        };
        // const data = {
        //   fields: this.inputFields,
        //   content: htmlContent,
        // };
        console.log("++++++++++++++++++++", data);
        axios
          .post("http://localhost:2001/template/create", data)
          .then((response) => console.log(response))
          .catch((error) => console.log(error));
        console.log(htmlContent.toString());
        this.showCreate = false;
        this.showForm = true;
      }
    },
    backtoTemplates() {
      this.showCreate = true;
      this.showForm = false;
    },
    onSubmit() {
      this.inputFields.forEach((e) => {
        this.inputText = this.inputText.replace(
          "{{" + e.variableName + "}}",
          e.value
        );
      });
      this.showTemplate = true;
      this.showCreate = false;
      this.showForm = false;
    },
  },
};
</script>

<style>
/* Add any necessary CSS styles */
</style>
