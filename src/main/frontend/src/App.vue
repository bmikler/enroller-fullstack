<template>
  <div id="app">
    <h1>
      <img src="./assets/logo.svg" alt="Enroller" class="logo">
      System do zapisów na zajęcia
    </h1>
    <div v-if="authenticatedUsername">
      <h2>Witaj {{ authenticatedUsername }}!
        <a @click="logout()" class="float-right  button-outline button">Wyloguj</a>
      </h2>
      <meetings-page :username="authenticatedUsername"></meetings-page>
    </div>

    <div v-else>
      <div class="error-msg" v-if="errorMessage"> {{ errorMessage }}</div>
      <login-form @login="submit($event)" :button-label=this.buttonLabel></login-form>
      <div v-if="registerMode">
        <a @click=registerModeOff()>Logowanie</a>
      </div>
      <div v-else>
        <p>Nie masz konta? <a @click=registerModeOn()>Rejestracja</a></p>
      </div>
    </div>

    
  </div>
</template>

<script>
    import "milligram";
    import LoginForm from "./LoginForm";
    import MeetingsPage from "./meetings/MeetingsPage";

    export default {
        components: {LoginForm, MeetingsPage},
        data() {
            return {
                authenticatedUsername: "",
                registerMode: false,
                buttonLabel: "Zaloguj",
                errorMessage: ''
            };
        },
        methods: {
            submit(user) {
                if (this.registerMode) {
                  this.register(user);
                } else {
                    this.login(user);
                }
                
            },
            logout() {
                this.authenticatedUsername = '';
                this.errorMessage = '';
            },
            registerModeOn() {
              this.registerMode = true;
              this.buttonLabel = "Zarejestruj"
            },

            registerModeOff() {
              this.registerMode = false;
              this.buttonLabel = "Zaloguj"
            },

            login(user) {
                this.authenticatedUsername = user;
            },

            
            register(user) {
                this.$http.post('participants', user)
                    .then(response => {
                        console.log(user);
                        console.log(user.login);
                        this.authenticatedUsername = user.login;
                    })
                    .catch(response => {
                      console.log(response);
                        this.errorMessage = response.bodyText;
                    });
}
        }
    };
</script>

<style>
  #app {
    max-width: 1000px;
    margin: 0 auto;
  }

  .logo {
    vertical-align: middle;
  }

  .error-msg {
    color: red;
  }

</style>

