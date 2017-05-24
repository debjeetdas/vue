<template>
  <div id="app">
    <!--<form @submit.prevent="search">-->
      <input v-on:input="search" placeholder="Enter a github username!" v-model="searchInput"/>
    <!--</form>-->
    <p v-if="data.login">
      {{ data.name }} ({{ data.login }})
    </p>
    <p v-else>{{ errorMsg }}</p>
    <form @submit.prevent="postPost">
      <input type="text" v-model="user" required/>
      <input type="text" v-model="job" required/>
      <input type="submit" value="submit">
    </form>
    <p v-if="userData.name">
      {{ userData.name }} ({{ userData.job }})
    </p>
  </div>
</template>

<script>
    import axios from 'axios';
    import _ from 'lodash';

    export default {
        data() {
            return {
                searchInput: '',
                data: [],
                errorMsg: '',
                user: '',
                job: '',
                userData: {}
            }
        },
        watch:{
          searchInput : function(){
            this.search();
          }
        },
        methods: {
            search: _.debounce(function() {
                console.log(this.searchInput);
                const api = `https://api.github.com/users/${this.searchInput}`

                axios.get(api)
                    .then(response => {
                        this.data = response.data
                    }).catch(error => {
                        this.errorMsg = 'No user or no location!'
                        this.data = []
                    })
            }),
            postPost() {
                const new_api = `https://reqres.in/api/users`;
                const user = this.user;
                const job = this.job;
                // console.log(user + ' ' + job);
                axios.post(new_api, {
                        "name": user,
                        "job": job
                    })
                    .then(response => {
                        this.userData = response.data
                    }).catch(error => {
                        // this.errorMsg = 'No user or no location!'
                        // this.data = []
                    })
            }
        }
    }
</script>