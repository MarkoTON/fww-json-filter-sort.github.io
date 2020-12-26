<template>
  <div class="container">
    <div class="row">
      <h1 class="text-center border-bottom">FACTORY WORLD WIDE</h1>

      <div class="col-12 mb-2">
        <div class="d-flex">
          <input type="text" class="form-control mr-1" v-model="searchName" placeholder="Name exp: Allyson">
          <input type="text" class="form-control mr-1" v-model="searchBalance" placeholder="Balance exp: 2,972.88">
          <select class="form-control mr-1" v-model="isActive" @change="statusOfUser">
            <option v-bind:value="'all'">Bouth</option>
            <option v-bind:value="true" selected>Active</option>
            <option v-bind:value="false">Not Active</option>
          </select>
          <input type="text" class="form-control mr-1" v-model="searchState" placeholder="Reg. exp: 2014-02-22T12:35:59">
          <input type="text" class="form-control mr-1" v-model="searchState" placeholder="State exp: Colorado">
          <input type="text" class="form-control" v-model="searchCountry" placeholder="State exp: Cyprus">
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
      searchName:'',
      searchBalance:'',
      searchState:'',
      searchCountry:'',
      isActive: true,
      pickValue:null
    }
  },
  computed: {
    filteredUser(){
      var pickSearch = this.searchOption();

      if(pickSearch.option == 'fullName'){
        return this.stateUsers.filter((name) => {
          return name.fullName.match(pickSearch.name)
        });
      } else if(pickSearch.option == 'balance'){
        return this.stateUsers.filter((name) => {
          return name.balance.match(pickSearch.name)
        });
      } else if(pickSearch.option == 'state'){
        return this.stateUsers.filter((name) => {
          return name.name.match(pickSearch.name)
        });
      } else if(pickSearch.option == 'country'){
        return this.stateUsers.filter((name) => {
          return name.country.match(pickSearch.name)
        });
      } else if(pickSearch.option == 'isActive'){
        console.log(pickSearch.name);
        if(pickSearch.name == 'all'){
          return this.stateUsers.filter((name) => {
            return name
          });
        } else {
          return this.stateUsers.filter((name) => {
            return name.isActive == pickSearch.name
          });
        }
      }
    }
  },
  methods:{
    statusOfUser(){
      console.log(this.isActive);
      
    },
    sortingJSON(){
      this.info.forEach(element => {
          for(let i = 0; i < element.state.length; i++){
            let metallica = element.state[i];
            metallica.country = element.country;
            // Add all country is one array
            this.countryState.push(metallica);
          }
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
      // console.log(this.stateUsers)
    },
    getData() {
      return axios.get('https://fww-demo.herokuapp.com/').then(response => (this.info = response.data))
    },
    searchOption(){
      if(this.searchName != ''){
        return {
          name: this.searchName,
          option: 'fullName'
        };
      } else if(this.searchBalance != ''){
        return {
          name: this.searchBalance,
          option: 'balance'
        };
      } else if(this.searchState != ''){
        return {
          name: this.searchState,
          option: 'state'
        };
      } else if(this.searchCountry != ''){
        return {
          name: this.searchCountry,
          option: 'country'
        };
      } else if(this.isActive == 'all' || this.isActive == true || this.isActive == false){
        return {
          name: this.isActive,
          option: 'isActive'
        };
      } 
      // else {
      //   return {
      //     name: this.searchName,
      //     option: 'fullName'
      //   };
      // }
    }
  },
  created () {
    this.getData().then((result) => {
      this.sortingJSON()
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
