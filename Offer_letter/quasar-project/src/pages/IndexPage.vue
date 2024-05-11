<template>
  <div class="q-pa-xl flex justify-center">
    <q-form
      @submit="onSubmit"
      @reset="onReset"
      class="q-gutter-md"
      v-if="showForm"
    >
      <q-input
        filled
        color="blue-grey"
        style="width: 250px;"
        type="name"
        v-model="name"
        label="Employee name *"
        hint="Name and surname"
        lazy-rules
        :rules="[(val) => (val && val.length > 0) || 'Please type name']"
      />

      <q-input
        filled
        color="blue-grey"
        style="width: 250px;"
        v-model="email"
        type="email"
        label="Employee email *"
        hint="Email"
        lazy-rules
        :rules="[(val) => (val && val.length > 0) || 'Please type email']"
      />

      <q-input
        filled
        color="blue-grey"
        style="width: 250px;"
        type="text"
        v-model="address"
        label="Employee address *"
        hint="Enter full address"
        lazy-rules
        :rules="[(val) => (val && val.length > 0) || 'Please type address']"
      />

      <q-select
        filled
        color="green-8"
        style="width: 250px;"
        v-model="role"
        :options="position"
        label="Employee role *"
        hint="Role"
        emit-value
        map-options
        clearable
        required
        lazy-rules
        :rules="[(val) => (val && val.length > 0) || 'Please select role']"
      />

      <q-select
        filled
        color="green-8"
        style="width: 250px;"
        v-model="city"
        label="City *"
        hint="Select city"
        :options="cityOptions"
        emit-value
        map-options
        clearable
        lazy-rules
        :rules="[
          (val) =>
            (val !== null && val !== undefined) || 'Please select a city',
        ]"
      />

      <q-select
        filled
        color="green-8"
        style="width: 250px;"
        v-model="state"
        label="State *"
        hint="State name"
        emit-value
        map-options
        clearable
        lazy-rules
        :options="states"
        :rules="[(val) => (val && val.length > 0) || 'Please select a state']"
      />

      <q-select
        filled
        color="green-8"
        v-model="salary"
        style="width: 250px;"
        label="Salary Range"
        hint="Select salary range"
        emit-value
        map-options
        clearable
        lazy-rules
        :options="salaryLpa"
        :rules="[
          (val) => (val && val.length > 0) || 'Please select a salary range',
        ]"
      />

      <q-input
        filled
        color="blue-grey"
        style="width: 250px;"
        v-model="company"
        type="text"
        label="Company name *"
        hint="Company name"
        lazy-rules
        :rules="[(val) => (val && val.length > 0) || 'Please type something']"
      />

      <q-input
        filled
        color="blue-grey"
        style="width: 250px;"
        v-model="joindate"
        type="date"
        label="Joining date *"
        hint="Joining date"
        lazy-rules
        :rules="[(val) => (val && val.length > 0) || 'Please select date']"
      />

      <div class="flex flex-center">
        <q-btn type="submit" color="blue-grey" label="Submit" no-caps />
        <q-btn
          label="Reset"
          type="reset"
          color="blue-grey"
          flat
          class="q-ml-sm"
        />
      </div>
    </q-form>
  </div>
  <br />
  <div class="">
    <!-- <div style="" v-if="showLetter"> -->
    <div class="flex flex-center">
      <div>
        <div style="padding: 20px" id="offer-letter">
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
          <div class="row">
            <div>
              <div>{{ enddate }}</div>
              <br />
              <div>{{ issueDate }}</div>
              <div>Name: {{ name }}</div>
              <div>Address: {{ address }}</div>
              <div>Location: {{ state }},{{ city }}</div>
            </div>
          </div>
          <div class="flex flex-center q-ma-sm">
            <div style="text-align: center">
              <u contenteditable="true">
                <p>OFFER LETTER</p>
              </u>
            </div>
          </div>
          <div class="q-px-lg" contenteditable="true" style="max-width: 500px">
            <div class="row">
              <div class="col">
                <p>
                  Congratulations! With reference to your interview, we are
                  pleased to offer you the position as <b>" {{ role }} "</b> -
                  PES at our Madurai office. Breakup of your compensation
                  package is provided herewith. As discussed we would like you
                  to join our company on {{ joindate }} at 7:00 AM unless
                  otherwise agreed to in writing or else the offer will be null
                  and void.
                </p>
              </div>
            </div>
            <div class="row">
              <div class="col">
                <div>Date of Joining : {{ joindate }}</div>
                <div>Position : {{ role }}</div>
                <div>CTC: {{ salary }}</div>
              </div>
            </div>
            <br />
            <div class="row">
              <div class="col">
                <div>
                  You are asked to report for duty at the following address:
                  EdexTech IT Solutions, AVM Complex Annai Theresa Street
                  Kalavasal, Madurai
                </div>
                <br />
                <div>
                  We count on you to take Edex to greater heights. Wish you the
                  very best!
                </div>
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
        <div class="flex flex-center">
          <q-btn
            color="blue-grey"
            label="Download"
            icon="cloud_download"
            @click="downloadPDF"
          />
          &nbsp;&nbsp;&nbsp;&nbsp;
          <q-btn color="blue-grey" label="Send" icon="send" @click="send" />
          &nbsp;&nbsp;&nbsp;&nbsp;
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
  </div>
</template>

<script>
import axios from "axios";
import html2canvas from "html2canvas";
import jsPDF from "jspdf";
export default {
  data() {
    return {
      signatureImage: null,
      showChoosefile: true,
      state: null,
      name: null,
      email: null,
      address: null,
      role: null,
      position: [
        "Manager",
        "Engineer",
        "Analyst",
        "Intern",
        "Team Lead",
        "Designer",
        "Administrator",
        "Consultant",
        "Project Manager",
        "Sales Representative",
        "Marketing Specialist",
      ],
      city: null,
      states: [
        "Andhra Pradesh",
        "Arunachal Pradesh",
        "Assam",
        "Bihar",
        "Chhattisgarh",
        "Goa",
        "Gujarat",
        "Haryana",
        "Himachal Pradesh",
        "Jharkhand",
        "Karnataka",
        "Kerala",
        "Madhya Pradesh",
        "Maharashtra",
        "Manipur",
        "Meghalaya",
        "Mizoram",
        "Nagaland",
        "Odisha",
        "Punjab",
        "Rajasthan",
        "Sikkim",
        "Tamil Nadu",
        "Telangana",
        "Tripura",
        "Uttar Pradesh",
        "Uttarakhand",
        "West Bengal",
        "Andaman and Nicobar Islands",
        "Chandigarh",
        "Dadra and Nagar Haveli and Daman and Diu",
        "Lakshadweep",
        "Delhi",
        "Puducherry",
      ],
      cityOptions: [
        "Chennai",
        "Coimbatore",
        "Madurai",
        "Tiruchirappalli",
        "Salem",
        "Tiruppur",
        "Erode",
        "Vellore",
        "Thoothukudi",
        "Dindigul",
        "Thanjavur",
        "Ranipet",
        "Sivakasi",
        "Karur",
        "Ooty",
        "Nagercoil",
        "Kanchipuram",
        "Kanyakumari",
        "Tirunelveli",
        "Pollachi",
      ],
      salary: null,
      salaryLpa: [
        "Below 2 LPA",
        "2 - 3 LPA",
        "3 - 4 LPA",
        "4 - 5 LPA",
        "5 - 6 LPA",
        "6 - 7 LPA",
        "7 - 8 LPA",
        "8 - 9 LPA",
        "9 - 10 LPA",
        "Above 10 LPA",
      ],
      joindate: null,
      issueDate: new Date().toLocaleDateString(),
      company: "abc",
      file: null,
      pdfFile: null,
      list: ["invert(70%)"],
      showLetter: false,
      showForm: true,
    };
  },
  methods: {
    onSubmit() {
      this.showLetter = true;
      this.showForm = false;
      const data = {
        name: this.name,
        email: this.email,
        role: this.role,
        joindate: this.joindate,
        company: this.company,
        address: this.address,
        city: this.city,
        state: this.state,
      };
      console.log(data);

      axios
        .post("http://localhost:2001/employee/offer", data)
        .then((response) => console.log(response))
        .catch((error) => console.log(error));
    },
    removeSignature() {
      this.signatureImage = null;
      this.showChoosefile = true;
    },
    onReset() {
      this.name = null;
      this.email = null;
      this.role = null;
      this.joindate = null;
      this.address = null;
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
    send() {
      const pdf = new jsPDF();
      const offerLetter = document.getElementById("offer-letter");
      const subject = `Offer Letter | ${this.name} | ${this.role}`;
      const message = `Dear ${this.name},

Congratulations!

Please find attached your detailed Offer letter. Please do acknowledge the same and confirm your DOJ. Your DOJ is ${this.joindate}. We would appreciate if you could give us a confirmation on your joining us. You are requested to report to our Madurai office at 10.30 AM.

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
      });
    },
  },
};
</script>
