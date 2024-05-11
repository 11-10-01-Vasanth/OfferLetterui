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
            <div class="flex justify-end">
              <q-btn
                @click="generateForm(a.id)"
                icon="chevron_right"
                color="blue-grey"
                label="Generate"
                style="margin-bottom: 10px"
              />
            </div>
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
                <div class="q-mt-lg">Signature</div>
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

    <div v-if="showForm">
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
              :type="item.type"
              v-model="item.value"
              :label="item.variableName"
              :hint="`Enter ${item.variableName} *`"
              style="min-width: 300px"
              lazy-rules
              :rules="[(val) => (val && val.length > 0) || 'Please type']"
            />
          </div>

          <div class="text-center">
            <q-btn type="submit" color="blue-grey" label="Submit" no-caps />
          </div>
        </q-form>
      </div>
    </div>

    <div v-if="showTemplate">
      <div class="row q-mt-xl text-center">
        <div class="col-3">
          <q-btn
            color="blue-grey"
            label="Download"
            icon="cloud_download"
            @click="downloadPDF"
          />
        </div>

        <div class="col-3">
          <q-input
            filled
            color="blue-grey"
            style="width: 250px"
            v-model="m_name"
            type="name"
            label="Name *"
            hint="Name"
            lazy-rules
            :rules="[(val) => (val && val.length > 0) || 'Please type name']"
          />
        </div>

        <div class="col-3">
          <q-input
            filled
            color="blue-grey"
            style="width: 250px"
            v-model="email"
            type="email"
            label="Send to mail *"
            hint="Email"
            lazy-rules
            :rules="[(val) => (val && val.length > 0) || 'Please type email']"
          />
        </div>
        <div class="col-3">
          <q-btn color="blue-grey" label="Send" icon="send" @click="send" />
        </div>
      </div>

      <div class="q-mt-xl" style="border: 1px solid black">
        <div id="offer-letter" class="q-mt-xl q-px-xl">
          <div class="row justify-between">
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
          <div v-html="inputText"></div>
          <div class="row">
            <div>
              Warm Regards,
              <br />
              <b>For EdexTech IT Solutions.</b>
            </div>
          </div>
          <div class="row q-mt-md">
            <div v-if="showChoosefile">
              <input type="file" @change="handleSignatureChange" />
            </div>
          </div>
          <div class="row" v-if="signatureImage">
            <div>
              <img
                :src="signatureImage"
                alt="Signature"
                style="max-width: 100px"
              />
            </div>
          </div>
          <div>Signature</div>
          <div class="row q-my-lg q-pa-md">
            <div class="col-12 col-md-4 column justify-center items-center">
              <span>
                <span class="material-symbols-outlined"> language </span>
                www.edextech.com
              </span>
            </div>
            <div class="col-12 col-md-4 column justify-center items-center">
              <span>
                <span class="material-symbols-outlined"> mail </span>
                edextech@gmail.com
              </span>
            </div>
            <div class="col-12 col-md-4 column justify-center items-center">
              <span>
                <span class="material-symbols-outlined"> call </span> 000 - 000
                - 0000
              </span>
            </div>
          </div>
        </div>
      </div>

      <div class="row q-mt-xl q-mb-xl">
        <div class="col">
          <q-btn
            v-if="signatureImage"
            color="blue-grey"
            label="RemoveImage"
            icon="delete"
            @click="removeSignature"
          />
        </div>
      </div>
    </div>
  </q-page>
</template>

<script>
import axios from "axios";
import html2canvas from "html2canvas";
import jsPDF from "jspdf";
export default {
  data() {
    return {
      email: "",
      m_name: "",
      showChoosefile: true,
      signatureImage: null,
      responseData: "",
      responsebyid: "",
      inputFields: "",
      inputText: "",
      showTemplates: true,
      showForm: false,
      showTemplate: false,
      tempinputText: "",
      tempinputFields: "",
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
          this.inputText = "";
          this.inputText = response.data.content;
          this.tempinputText = response.data.content;
          console.log(this.inputText);
          let obj = JSON.parse(response.data.fields);
          this.inputFields = obj;
          this.tempinputFields = obj;
          console.log(this.inputFields);
          this.showTemplates = false;
          this.showForm = true;
        })
        .catch((error) => {
          console.error("Error:", error);
        });
    },
    onSubmit() {
      this.showTemplates = false;
      this.showForm = false;
      this.showTemplate = true;
      this.inputFields.forEach((e) => {
        this.inputText = this.inputText.replace(
          "{{" + e.variableName + "}}",
          e.value
        );
      });
      console.log(this.inputText);
    },
    backtoTemplates() {
      this.showTemplates = true;
      this.showForm = false;
    },

    handleSignatureChange(event) {
      const file = event.target.files[0];
      this.showChoosefile = false;
      if (file) {
        const reader = new FileReader();
        reader.onload = (e) => {
          this.signatureImage = e.target.result;
        };
        reader.readAsDataURL(file);
      }
    },
    removeSignature() {
      this.signatureImage = null;
      this.showChoosefile = true;
    },
    downloadPDF() {
      const pdf = new jsPDF();
      const offerLetter = document.getElementById("offer-letter");

      html2canvas(offerLetter).then((canvas) => {
        const imageData = canvas.toDataURL("image/png");
        const pdfWidth = 210;
        const pdfHeight = 297;
        const contentWidth = canvas.width;
        const contentHeight = canvas.height;

        const marginLeft = 20;
        const marginRight = 20;
        const marginTop = 20;
        const marginBottom = 20;

        const imgWidth = pdfWidth - marginLeft - marginRight;
        const imgHeight = (contentHeight * imgWidth) / contentWidth;

        let positionX = marginLeft;
        let positionY = marginTop;

        pdf.addImage(
          imageData,
          "PNG",
          positionX,
          positionY,
          imgWidth,
          imgHeight
        );

        let remainingHeight = pdfHeight - imgHeight - marginTop - marginBottom;

        while (remainingHeight < 0) {
          pdf.addPage();

          positionY = marginTop - remainingHeight;

          pdf.addImage(
            imageData,
            "PNG",
            positionX,
            positionY,
            imgWidth,
            imgHeight
          );

          remainingHeight = pdfHeight - imgHeight - positionY - marginBottom;
        }

        pdf.save("offer_letter.pdf");
        this.pdfFile = pdf.output("blob");
      });
    },
    send() {
      const offerLetter = document.getElementById("offer-letter");
      const subject = `Offer Letter | ${this.m_name}`;
      const message = `Dear ${this.m_name},

Congratulations!

Please find attached your detailed Offer letter. Please do acknowledge the same and confirm your DOJ. We would appreciate if you could give us a confirmation on your joining us. You are requested to report to our Madurai office at 10.30 AM.

Our office address :

AVM Complex, Annai Theresa Street, Kalavasal, Madurai.

Post Offer letter confirmation, on-boarding SPOC (xxxx) would get in touch with you.

Please reach out to me on 0000-000-000 in case of any queries.

Best regards,
[Edex Tech]`;

      html2canvas(offerLetter).then((canvas) => {
        var imageData = canvas.toDataURL("image/png");
        const formData = new FormData();
        formData.append("email", this.email);
        formData.append("imageData", imageData);
        formData.append("subject", subject);
        formData.append("message", message);

        axios
          .post("http://localhost:2001/employee/sendmail", formData, {
            headers: {
              "Content-Type": "multipart/form-data",
            },
          })
          .then(() => alert("Mail sent successfully !!!"))
          .catch((err) => console.log(err));
      });
    },
  },
};
</script>
