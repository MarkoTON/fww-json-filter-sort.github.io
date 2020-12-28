<template>
  <div class="container">
    <div class="row">
      <h1 class="text-center border-bottom">FACTORY WORLD WIDE</h1>
      <div class="col-12 mb-2">
        <div class="d-flex">
          <input type="text" class="form-control mr-1 filterName" v-model="searchName" placeholder="Name exp: Allyson">
          <input type="text" class="form-control mr-1 filterBalance" v-model="searchBalance" placeholder="Balance exp: 2,972.88">
          <select class="form-control mr-1 selectStatus" v-model="isActive" >
            <option v-bind:value="'all'">Both</option>
            <option v-bind:value="true">Active</option>
            <option v-bind:value="false">Not Active</option>
          </select>
          <input type="text" class="form-control mr-1 filterDate" v-model="searchDate" placeholder="Reg. exp: 2014-02-22T12:35:59">
          <input type="text" class="form-control mr-1 filterState" v-model="searchState" placeholder="State exp: Colorado">
          <input type="text" class="form-control filterCountry" v-model="searchCountry" placeholder="State exp: Cyprus">
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
      isActive: 'all',
      searchDate:'',
      pickValue:null,
      searchOptionsClasses:['filterName', 'filterBalance', 'selectStatus', 'filterDate', 'filterState', 'filterCountry']
    }
  },
  watch : {
    // searchName:function(val) {
    //   this.searchBalance = '';
    //   this.isActive = 'all';
    //   this.searchDate = '';
    //   this.searchState = '';
    //   this.searchCountry = '';
    // },
    // searchBalance:function(val) {
    //   this.searchName = '';
    //   this.isActive = 'all';
    //   this.searchDate = '';
    //   this.searchState = '';
    //   this.searchCountry = '';
    // },
    // isActive:function(val) {
    //   this.searchName = '';
    //   this.searchBalance = '';
    //   this.searchDate = '';
    //   this.searchState = '';
    //   this.searchCountry = '';
    // },
    // searchDate:function(val) {
    //   this.searchName = '';
    //   this.isActive = 'all';
    //   this.searchBalance = '';
    //   this.searchState = '';
    //   this.searchCountry = '';
    // },
    // searchState:function(val) {
    //   this.searchName = '';
    //   this.isActive = 'all';
    //   this.searchDate = '';
    //   this.searchBalance = '';
    //   this.searchCountry = '';
    // },
    // searchCountry:function(val) {
    //   this.searchName = '';
    //   this.isActive = 'all';
    //   this.searchDate = '';
    //   this.searchState = '';
    //   this.searchBalance = '';
    // }
  },
  computed: {
    // rows() {
    //   return this.stateUsers.length
    // },
    filteredUser(){
      var pickSearch = this.searchOption();

      if(pickSearch.option == 'fullName'){
        // console.log(pickSearch.name.toLowerCase());
        return this.stateUsers.filter((name) => {
          return name.fullName.toLowerCase().match(pickSearch.name)
        });
      } else if(pickSearch.option == 'balance'){
        return this.stateUsers.filter((name) => {
          return name.balance.match(pickSearch.name)
        });
      } else if(pickSearch.option == 'state'){
        return this.stateUsers.filter((name) => {
          return name.name.toLowerCase().match(pickSearch.name)
        });
      } else if(pickSearch.option == 'country'){
        return this.stateUsers.filter((name) => {
          return name.country.toLowerCase().match(pickSearch.name)
        });
      } else if(pickSearch.option == 'registered'){
        return this.stateUsers.filter((name) => {
          return name.registered.match(pickSearch.name)
        });
      } else if(pickSearch.option == 'isActive'){
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
    // Here JSON is filter and sorted in one array of Objects that is easier to loop over
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
      console.log(this.stateUsers);
    },
    getData() {
      // Geting data from API - FWW
      return axios.get('https://fww-demo.herokuapp.com/').then(response => (this.info = response.data))
    },
    searchOption(){
      if(this.searchName != ''){
        return {
          name: this.searchName.toLowerCase(),
          option: 'fullName'
        };
      } else if(this.searchBalance != ''){
        return {
          name: this.searchBalance,
          option: 'balance'
        };
      } else if(this.searchState != ''){
        return {
          name: this.searchState.toLowerCase(),
          option: 'state'
        };
      } else if(this.searchCountry != ''){
        return {
          name: this.searchCountry.toLowerCase(),
          option: 'country'
        };
      } else if(this.searchDate != ''){
        return {
          name: this.searchDate,
          option: 'registered'
        };
      } else if(this.isActive == 'all' || this.isActive == true || this.isActive == false){
        return {
          name: this.isActive,
          option: 'isActive'
        };
      }
    },
    reset(e){
      // console.log("Nirvana and Metallica");
      // console.log(e.target.classList);
      // this.searchOptionsClasses.forEach(element => {
      //   console.log(element)
      //   if(e.target.classList.contains(element)){
      //     console.log(true);
      //     // e.target.classList.add("disabled")
      //   } else {
      //     console.log(false);
      //     e.target.classList.add("disabled")
      //   }
      // });
    }
  },
  created () {
    this.getData().then((result) => {
      this.sortingJSON()
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
