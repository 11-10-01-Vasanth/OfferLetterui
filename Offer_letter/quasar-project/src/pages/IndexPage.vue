<template>
  <q-page class="flex flex-center">
    <div class="q-pa-md" style="max-width: 400px">
      <q-form @submit="onSubmit" @reset="onReset" class="q-gutter-md">
        <q-input
          filled
          v-model="name"
          label="Your name *"
          hint="Name and surname"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-input
          filled
          v-model="email"
          type="email"
          label="Your email *"
          hint="Email"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-input
          filled
          v-model="role"
          type="text"
          label="Your role *"
          hint="Role"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-input
          filled
          v-model="company"
          type="text"
          label="Company name *"
          hint="Company name"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-input
          filled
          v-model="joindate"
          type="date"
          label="Your joining date *"
          hint="Joining date"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <div>
          <q-btn type="submit" color="primary" label="Submit" no-caps />
          <q-btn
            label="Reset"
            type="reset"
            color="primary"
            flat
            class="q-ml-sm"
          />
        </div>
      </q-form>
    </div>
    <q-dialog v-model="dialog" :backdrop-filter="backdropFilter" persistent>
      <q-card>
        <div id="offer-letter">
          <q-card-section
            class="row items-center justify-center q-pb-none text-h6"
          >
            Offer Letter
          </q-card-section>
          <q-card-section>
            <div
              style="padding: 10px; font-size: 0.8rem; letter-spacing: 0.8px"
            >
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
              <br />
              <div class="row">
                <div>
                  <div>{{ issueDate }}</div>
                  <div>Name: {{ name }}</div>
                  <div>Location: {{ location }}</div>
                </div>
              </div>
              <br />
              <div class="row">
                <div>
                  <div>
                    Congratulations! With reference to your interview, we are
                    pleased to offer you the position as <b>" {{ role }} "</b> -
                    PES at our Madurai office. Breakup of your compensation
                    package is provided herewith. As discussed we would like you
                    to join our company on {{ joindate }} at 7:00 AM unless
                    otherwise agreed to in writing or else the offer will be
                    null and void.
                  </div>
                  <br />
                  <div>Date of Joining : {{ joindate }}</div>
                  <div>Position : {{ role }}</div>
                </div>
              </div>
              <br />
              <div class="row">
                <div>
                  <div>
                    You are asked to report for duty at the following address:
                    EdexTech IT Solutions, AVM Complex Annai Theresa Street
                    Kalavasal, Madurai
                  </div>
                  <br />
                  <div>
                    We count on you to take Edex to greater heights. Wish you
                    the very best!
                  </div>
                </div>
              </div>
              <br />
              <div class="row">
                <div>
                  Warm Regards,
                  <br />
                  <b>For EdexTech IT Solutions.</b>
                </div>
              </div>
            </div>
          </q-card-section>
        </div>
        <q-card-actions align="right">
          <q-btn
            flat
            color="primary"
            label="Download"
            icon="download"
            @click="downloadPDF"
          />
          <q-btn flat label="Close" color="primary" @click="dialog = false" />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script>
import axios from "axios";
import html2canvas from "html2canvas";
import jsPDF from "jspdf";
export default {
  data() {
    return {
      name: null,
      location: "Madurai",
      email: null,
      role: null,
      joindate: null,
      issueDate: new Date().toLocaleDateString(),
      company: "abc",
      file: null,
      pdfFile: null,
      list: ["invert(70%)"],
      // dialog: false,
      dialog: true,
      backdropFilter: null,
    };
  },
  methods: {
    onSubmit() {
      this.backdropFilter = "invert(70%)";
      this.dialog = true;
      const data = {
        name: this.name,
        email: this.email,
        role: this.role,
        joindate: this.joindate,
        company: this.company,
      };
      console.log(data);

      axios
        .post("http://localhost:2001/employee/offer", data)
        .then((response) => console.log(response))
        .catch((error) => console.log(error));

      this.sentMail();
      // alert("Mail sent successfully to " + this.email);
    },

    sentMail() {
      const subject = "Offer Letter";
      const message = "Congratulations !!!";
      const formData = new FormData();
      formData.append(
        "file",
        new Blob([this.pdfFile], { type: "application/pdf" }),
        "Offer_Letter.pdf"
      );
      formData.append("email", this.email);
      formData.append("subject", subject);
      formData.append("message", message);

      console.log(formData);
      axios
        .post("http://localhost:2001/employee/sendmail", formData, {
          headers: {
            "Content-Type": "multipart/form-data",
          },
        })
        .then((res) => console.log(res))
        .catch((err) => console.log(err));
    },

    onReset() {
      this.name = null;
      this.email = null;
      this.role = null;
      this.joindate = null;
      this.company = null;
    },
    downloadPDF() {
      const pdf = new jsPDF();
      const offerLetter = document.getElementById("offer-letter");

      html2canvas(offerLetter).then((canvas) => {
        var imageData = canvas.toDataURL("image/png");
        const imgWidth = 210; // A4 width in mm
        const pageHeight = imgWidth * 1.414; // A4 ratio
        const imgHeight = (canvas.height * imgWidth) / canvas.width;
        let heightLeft = imgHeight;
        let position = 0;

        pdf.addImage(imageData, "PNG", 0, position, imgWidth, imgHeight);
        heightLeft -= pageHeight;

        while (heightLeft >= 0) {
          position = heightLeft - imgHeight;
          pdf.addPage();
          pdf.addImage(imageData, "PNG", 0, position, imgWidth, imgHeight);
          heightLeft -= pageHeight;
        }
        pdf.save("offer_letter.pdf");
        this.pdfFile = pdf.output("blob");
        console.log(imageData);
      });
    },
  },
};
</script>
