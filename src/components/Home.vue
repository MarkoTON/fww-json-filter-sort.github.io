<template>
  <div class="container">
    <div class="row">
      <h1 class="text-center border-bottom">FACTORY WORLD WIDE</h1>

      <div class="col-12 mb-2">
        <div class="d-inline-block">
          <!-- <select class="form-control" v-model="pickCountry" @change="selectCountry">
            <option v-for="(selectCountry,index) in info" v-bind:key="index" v-bind:value="selectCountry.country">{{selectCountry.country}}</option>
          </select> -->
          <input type="text" class="form-control" v-model="searchName" placeholder="Name">
          <!-- <input type="text" class="form-control" v-model="searchTerm" placeholder="Balance"> -->
        </div>
      </div>
      <div class="col-12">
        <table class="table">
          <thead class="thead-dark">
            <tr>
              <th scope="col">#</th>
              <th scope="col">Full Name</th>
              <th scope="col">Balance</th>
              <th scope="col">Active</th>
              <th scope="col">Registered</th>
              <th scope="col">State</th>
              <th scope="col">Country</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item,index) in filteredUser" :key="item.id">
              <th scope="row">{{index+1}}</th>
              <td>{{item.fullName}}</td>
              <td>{{item.balance}}</td>
              <td>{{item.isActive}}</td>
              <td>{{item.registered}}</td>
              <td>{{item.name}}</td>
              <td>{{item.country}}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div><!-- row -->

    <div class="row">
      <div class="col">
        <!-- <div v-for="(countryFilter,index) in country" :key="index">
          <div v-for="(item) in countryFilter.users" :key="item.id">
              {{item.fullName}}
          </div>
        </div> -->
      </div><!-- col -->
    </div><!-- row -->
    <!-- {{stateUsers}} -->
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Home',
  data () {
    return {
      info:null,
      countryState:[],
      stateUsers:[],
      extUserData:[],
      pickCountry: "Chad",
      searchName:'',
      pickValue:null
    }
  },
  computed: {
    filteredUser(){
      return this.stateUsers.filter(name => {
        return name.fullName.toLowerCase().match(this.searchName)
      })
    }
  },
  methods:{
    selectCountry(){
      this.info.forEach(element => {
        // if(this.pickCountry == element.country){
          // Reset all countrys
          // this.countryState = [];
          for(let i = 0; i < element.state.length; i++){
            let metallica = element.state[i];
            metallica.country = element.country;
            // Add all country is one array
            this.countryState.push(metallica);
          }
        // };
      });
      // Reset all users
      this.stateUsers = [];

      this.countryState.forEach(element =>{
        for(let i = 0; i < element.users.length;i++){
          let nirvana = element.users[i]
          nirvana.name = element.name;
          nirvana.country = element.country;
          // Add all users in one array
          this.stateUsers.push(nirvana)
        }
      });
    },
    getData() {
      return axios.get('https://fww-demo.herokuapp.com/').then(response => (this.info = response.data))
    }
  },
  created () {
    this.getData().then((result) => {
      this.selectCountry()
    })
  },
  beforeCreate() {
  },
  beforeMount(){
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
