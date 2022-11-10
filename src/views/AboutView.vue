<template>
  <div id="app">
    <img alt="Vue logo" src="../assets/logo.png">
    <b-card bg-variant="light">
      <b-form-group
        label-cols-lg="3"
        label="Redmine infomasion"
        label-size="lg"
        label-class="font-weight-bold pt-0"
        class="mb-0"
      >
        <!-- Authorization URL -->
        <b-form-group
          label="Authorization URL"
          label-for="nested-url"
          label-cols-sm="3"
          label-align-sm="right"
        >
          <b-form-input
            id="nested-url"
            type="url"
            v-model="oath_url"
          ></b-form-input>
        </b-form-group>
        <b-button variant="primary">
          <a :href="oath_url+oath_state">Cloud ID</a>
        </b-button>

        <!-- URL -->
        <b-form-group
          label="Jira URL:"
          label-for="nested-url"
          label-cols-sm="3"
          label-align-sm="right"
        >
          <b-form-input
            id="nested-url"
            type="url"
            v-model="url"
          ></b-form-input>
        </b-form-group>

        <!-- Filter ID -->
        <b-form-group
          label="Filter ID:"
          label-for="nested-query"
          label-cols-sm="3"
          label-align-sm="right"
        >
          <b-form-input
            id="nested-query"
            type="text"
            v-model="query"
          ></b-form-input>
        </b-form-group>

        <!-- Access Key -->
        <b-form-group
          label="APIトークン:"
          label-for="nested-access"
          label-cols-sm="3"
          label-align-sm="right"
        >
          <b-form-input
            id="nested-access"
            type="text"
            v-model="accessKey"
          ></b-form-input>
        </b-form-group>

        <!-- Button -->
        <b-form-group
          label-for="nested-access"
          label-cols-sm="11"
          label-align-sm="right"
        >
          <b-button variant="primary" @click="action">Primary</b-button>
        </b-form-group>
      </b-form-group>
    </b-card>
  </div>
</template>

<script>
import axios from 'axios';
// import { Buffer } from 'buffer';

export default {
  name: 'App',
  components: {
    // HelloWorld
  },
  data() {
    // 
    return {
      // For CloudID 
      oath_url: ""
      , oath_state: "YOUR_USER_BOUND_VALUE"
      , cloudId: null
      // For Access Token
      , access_url: "https://auth.atlassian.com/oauth/token"
      , client_id: null
      , client_secret: null
      , callback_url: ""
      // For Purpose API
      , url: "https://xxxxx.atlassian.net/rest/api/2/search?jql=filter=filter1"
      // , project: ""
      , query: "filter=filter1"
      , accessKey: ""
      , account: ""
    }
  },
  created: function () {
    // 
    console.log( this.$route.query );
    if (this.$route.query) {
      this.cloudId = this.$route.query.code;
    }
  },
  methods: {
    action() {
      // 
      console.log( this.access_url );
      axios.post(
        this.access_url
        , {
          "client_id": ""
          , "client_secret": ""
          , "code": this.cloudId
          , "redirect_uri": this.callback_url
        }
        , {
          headers: {
            "Content-Type": "application/json"
            , "Access-Control-Allow-Origin": "*"
            // , "CORS_ALLOW_ALL_ORIGINS": true
          }
        }
      )
      .then(function (res) {
        console.log(res);
      })
      .catch(function (error) {
        console.log(error);
      })
      .finally(function () {});
      
      // let auth_s = this.account + ":" + this.accessKey;
      // console.log(auth_s);

      // 
      // axios.defaults.headers.common['Access-Control-Allow-Origin'] = '*';
      // axios.defaults.headers.common['Content-Type'] = 'application/json';
      // axios.defaults.headers.common['Access-Control-Allow-Methods'] = 'GET, POST, DELETE, PUT, OPTIONS, HEAD';
      
      // axios.get(this.url
      // , {
      //   // params: { jql: this.query }
      // })
      // .then(function (response) {
      //   console.log(response);
      // })
      // .catch(function (error) {
      //   console.log(error);
      // })
      // .finally(function () {});
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
