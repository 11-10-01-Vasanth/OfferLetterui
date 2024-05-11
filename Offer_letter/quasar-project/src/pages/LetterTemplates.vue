<template>
  <q-page class="flex flex-center">
    <div v-if="showTemplates" style="margin-top: 20px">
      <div class="row">
        <div
          class="col-12 q-px-lg"
          v-for="a in responseData"
          :key="a.id"
          style="margin-bottom: 20px"
        >
          <div class="flex justify-center q-px-xl">
            <h4 style="font-size: 1.5rem; font-weight: bold; color: #333">
              {{ a.templatename }}
            </h4>
          </div>
          <q-card style="border: 1px solid #ccc; border-radius: 8px">
            <q-card-section>
              <div
                class="row justify-between"
                style="
                  word-wrap: break-word;
                  white-space: pre-wrap;
                  font-family: 'Courier New', monospace;
                  font-size: 1rem;
                  color: #555;
                "
              >
                <div class="col-4 column items-center justify-center">
                  <h4><b>EdexTech</b></h4>
                </div>
                <div class="col-4 column items-center justify-center">
                  <div>
                    <div>
                      AVM Complex <br />
                      Annai Theresa Street <br />
                      Kalavasal, Madurai
                    </div>
                  </div>
                </div>
              </div>
              <div v-html="a.content"></div>
              <div
                style="
                  word-wrap: break-word;
                  white-space: pre-wrap;
                  font-family: 'Courier New', monospace;
                  font-size: 1rem;
                  color: #555;
                "
              >
                <div class="row">
                  <div>
                    Warm Regards,
                    <br />
                    <b>For EdexTech IT Solutions.</b>
                  </div>
                </div>
                <div class="q-mt-md">Signature</div>
                <div class="row q-my-lg q-pa-md">
                  <div
                    class="col-12 col-md-4 column justify-center items-center"
                  >
                    <span>
                      <span class="material-symbols-outlined"> language </span>
                      www.edextech.com
                    </span>
                  </div>
                  <div
                    class="col-12 col-md-4 column justify-center items-center"
                  >
                    <span>
                      <span class="material-symbols-outlined"> mail </span>
                      edextech@gmail.com
                    </span>
                  </div>
                  <div
                    class="col-12 col-md-4 column justify-center items-center"
                  >
                    <span>
                      <span class="material-symbols-outlined"> call </span> 000
                      - 000 - 0000
                    </span>
                  </div>
                </div>
              </div>
            </q-card-section>
          </q-card>
        </div>
      </div>
    </div>
  </q-page>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      responseData: "",
      inputFields: "",
      inputText: "",
      showTemplates: true,
      showForm: false,
      showTemplate: false,
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
  methods: {},
};
</script>


