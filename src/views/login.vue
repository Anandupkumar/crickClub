<template>
  <div>
    <!-- page-wrapper Start-->
    <div class="container-fluid p-0">
      <div class="row m-0">
        <div class="col-12 p-0">
          <div class="login-card">
            <div>
              <div class="login-main">
                <!-- <b-card no-body style="padding: 35px 0px 0px 0px"> -->
                  <!-- <b-tabs pills vertical> -->
                    <!-- <b-card-text> -->
                      <!-- <form class="theme-form"> -->
                      <h4>Sign in to account</h4>

                      <div class="form-group">
                        <input
                          v-model="phone"
                          class="form-control"
                          type="text"
                          id="phone"
                          required=""
                          placeholder="Enter your Phone"
                        />
                      </div>
                      <div class="form-group mb-0">
                        <button
                          class="btn btn-primary btn-block"
                          type="button"
                          @click="signUp"
                        >
                          Login
                        </button>
                      </div>

                      <!-- </form> -->
                    <!-- </b-card-text> -->
                    <div id="recaptcha-container"></div>
                  <!-- </b-tabs> -->
                <!-- </b-card> -->
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- latest jquery-->
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import firebase from "../firebaseConfig";

const phone = ref("");
const appVerifier = ref(null);
const verificationId = ref(null);

onMounted(() => {
  appVerifier.value = new firebase.auth.RecaptchaVerifier(
    "recaptcha-container",
    {
      size: "invisible",
    }
  );
});

const signUp = () => {
  if(phone.value !== ""){
    // console.log(phone.value);
    sendVerificationCode();
  }
}

const sendVerificationCode = () => {
  firebase
    .auth()
    .signInWithPhoneNumber(phone.value, appVerifier.value)
    .then((confirmationResult) => {
      verificationId.value = confirmationResult.verificationId;
      console.log(verificationId.value);
    })
    .catch((error) => {
      console.error(error);
    });
};
</script>

<style>
/* Add your styles here */
</style>
