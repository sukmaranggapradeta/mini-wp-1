<template>
  <div class>
    <g-signin-button
      class="google-button"
      :params="googleSignInParams"
      @success="onSignInSuccess"
      @error="onSignInError"
    ><a >
      <span class="google-button__icon">
        <svg viewBox="0 0 366 372" xmlns="http://www.w3.org/2000/svg">
          <path
            d="M125.9 10.2c40.2-13.9 85.3-13.6 125.3 1.1 22.2 8.2 42.5 21 59.9 37.1-5.8 6.3-12.1 12.2-18.1 18.3l-34.2 34.2c-11.3-10.8-25.1-19-40.1-23.6-17.6-5.3-36.6-6.1-54.6-2.2-21 4.5-40.5 15.5-55.6 30.9-12.2 12.3-21.4 27.5-27 43.9-20.3-15.8-40.6-31.5-61-47.3 21.5-43 60.1-76.9 105.4-92.4z"
            id="Shape"
            fill="#EA4335"
          ></path>
          <path
            d="M20.6 102.4c20.3 15.8 40.6 31.5 61 47.3-8 23.3-8 49.2 0 72.4-20.3 15.8-40.6 31.6-60.9 47.3C1.9 232.7-3.8 189.6 4.4 149.2c3.3-16.2 8.7-32 16.2-46.8z"
            id="Shape"
            fill="#FBBC05"
          ></path>
          <path
            d="M361.7 151.1c5.8 32.7 4.5 66.8-4.7 98.8-8.5 29.3-24.6 56.5-47.1 77.2l-59.1-45.9c19.5-13.1 33.3-34.3 37.2-57.5H186.6c.1-24.2.1-48.4.1-72.6h175z"
            id="Shape"
            fill="#4285F4"
          ></path>
          <path
            d="M81.4 222.2c7.8 22.9 22.8 43.2 42.6 57.1 12.4 8.7 26.6 14.9 41.4 17.9 14.6 3 29.7 2.6 44.4.1 14.6-2.6 28.7-7.9 41-16.2l59.1 45.9c-21.3 19.7-48 33.1-76.2 39.6-31.2 7.1-64.2 7.3-95.2-1-24.6-6.5-47.7-18.2-67.6-34.1-20.9-16.6-38.3-38-50.4-62 20.3-15.7 40.6-31.5 60.9-47.3z"
            fill="#34A853"
          ></path>
        </svg>
      </span>
      <span class="google-button__text">Sign in with Google</span></a>
    </g-signin-button>
  </div>
</template>
 
<script>
export default {
  props: ["myServer"],
  data() {
    return {
      /**
       * The Auth2 parameters, as seen on
       * https://developers.google.com/identity/sign-in/web/reference#gapiauth2initparams.
       * As the very least, a valid client_id must present.
       * @type {Object}
       */
      googleSignInParams: {
        client_id:
          "435243723579-rk8fjkdnman011rj78bobfn8p41u40je.apps.googleusercontent.com"
      }
    };
  },
  methods: {
    onSignInSuccess(googleUser) {
      // `googleUser` is the GoogleUser object that represents the just-signed-in user.
      // See https://developers.google.com/identity/sign-in/web/reference#users
      const idToken = googleUser.getAuthResponse().id_token;
      // console.log(idToken, " ini token google")
      axios
        .post(`${this.myServer}/googleSignIn`, { idToken })
        .then(({ data }) => {
          localStorage.setItem("token", data.token);
          localStorage.setItem("picture", data.picture);
          localStorage.setItem("name", data.name);
          localStorage.setItem("email", data.email);
          localStorage.setItem("id", data.id);
          this.$emit("initialization");
        })
        .catch(err => {
          Swal.fire({
            type: "error",
            title: `${err}`,
            animation: false,
            customClass: {
              popup: "animated tada"
            }
          });
        });
      //   const profile = googleUser.getBasicProfile() // etc etc
      //   console.log(profile)
    },
    onSignInError(error) {
      // `error` contains any error occurred.
      console.log("OH NOES", error);
    }
  }
};
</script> 
 
<style>
a{
  cursor: pointer;
}
.google-button {
  height: 40px;
  border-width: 0;
  background: white;
  color: #434343;
  border-radius: 5px;
  white-space: nowrap;
  box-shadow: 1px 1px 0px 1px rgba(0, 0, 0, 0.05);
  transition-property: background-color, box-shadow;
  transition-duration: 150ms;
  transition-timing-function: ease-in-out;
  padding: 0;
}
/* &:focus, */
.google-button:hover {
  box-shadow: 1px 4px 5px 1px rgba(0, 0, 0, 0.1);
  
}

.google-button:active {
  background-color: #e5e5e5;
  box-shadow: none;
  transition-duration: 10ms;
}

.google-button__icon {
  display: inline-block;
  vertical-align: middle;
  margin: 8px 0 8px 8px;
  width: 18px;
  height: 18px;
  box-sizing: border-box;
}

.google-button__icon--plus {
  width: 27px;
}

.google-button__text {
  display: inline-block;
  vertical-align: middle;
  padding: 0 24px;
  font-size: 14px;
  font-weight: bold;
  font-family: "Roboto", arial, sans-serif;
}
</style> 