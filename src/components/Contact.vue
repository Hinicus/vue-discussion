<script setup>
import { ref } from "vue";
import { Notyf } from "notyf";
import "notyf/notyf.min.css";
import { errorMessages } from "vue/compiler-sfc";
import axios from "axios";

const notyf = new Notyf();

const WEB3FORM_ACCESS_KEY = "771b0ace-64c1-406e-90ea-7c675cfc92d4";

//initial value for the email:

const subject = "New Message from Portfolio Contact Form";

const name = ref("");
const email = ref("");
const message = ref("");

const isLoading = ref(false);

//Function for sending the email:
async function submitForm() {
  isLoading.value = true;
  try {
    const response = await axios.post(
      "https://api.web3forms.com/submit",
      {
        access_key: WEB3FORM_ACCESS_KEY,
        subject: subject,
        name: name.value,
        message: message.value,
      },
      {
        headers: {
          "Content-Type": "application/json",
          Accept: "application/json",
        },
      },
    );

    const result = response.data;

    if (result.success) {
      notyf.success("Message Sent!");
      name.value = "";
      email.value = "";
      message.value = "";
    }
  } catch (e) {
    notyf.error("Failed to send email");
  } finally {
    isLoading.value = false;
  }
}
</script>

<template>
  <!-- contact -->
  <h1 class="text-center my-4 pt-5" id="contact">Contact</h1>
  <div class="contact-section">
    <div class="row align-items-center mt-4">
      <div class="col-md-6 map-container">
        <iframe
          id="gmap_canvas"
          src="https://maps.google.com/maps?q=centro%20escolar%20university%20manila&t=&z=13&ie=UTF8&iwloc=&output=embed"
          frameborder="0"
          scrolling="no"
          marginheight="0"
          marginwidth="0"></iframe>
      </div>
      <div class="col-md-6">
        <form @submit.prevent="submitForm">
          <div class="mb-3">
            <input
              type="text"
              class="form-control contact-form-control"
              placeholder="First Name M.I. Last Name"
              v-model="name" />
          </div>
          <div class="mb-3">
            <input
              type="email"
              class="form-control contact-form-control"
              placeholder="Email"
              v-model="email" />
          </div>
          <div class="mb-3">
            <textarea
              class="form-control contact-form-control"
              rows="6"
              placeholder="Message"
              v-model="message"></textarea>
          </div>
          <div class="form-footer">
            <div class="social-icons">
              <!--                                <a href="https://www.facebook.com/profile.php?id=100085701498879" id="facebook"><i class="fab fa-facebook"></i></a> -->
              <a
                href="https://www.linkedin.com/in/charles-babbage-8291a6211/"
                id="linkedin"
                ><i class="fab fa-linkedin"></i
              ></a>
              <a href="https://gitlab.com/cbabbage0991" id="gitlab"
                ><i class="fab fa-gitlab"></i
              ></a>
              <a href="https://github.com/cbabbage0991" id="github"
                ><i class="fab fa-github"></i
              ></a>
            </div>
            <button
              type="submit"
              class="submit-btn pl-5 pr-5"
              :disabled="isLoading">
              {{ isLoading ? "Sending ... " : "Submit" }}
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>
