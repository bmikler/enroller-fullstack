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
      <login-form @login="submit($event)" :button-label=this.buttonLabel></login-form>
    </div>
    <div v-if="registerMode"> 
      <a @click=registerModeOff()>Logowanie</a>
    </div>
    <div v-else>
      <p>Nie masz konta? <a @click=registerModeOn()>Rejestracja</a></p>
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
                buttonLabel: "Zaloguj"
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
                        // udało się
                        console.log(response)
                    })
                    .catch(response => {
                        // nie udało sie     
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
</style>

