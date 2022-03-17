<template>
  <div id="dashboard" style="background-color:white">
    <!-- <img alt="Vue logo" src="../assets/logo.png" />
    <HelloWorld msg="Welcome to Your Vue.js App" /> -->
    <header class="bg-dark" align="left">
    <section align="right">
      <div class="container">
       <router-link to="/About"><b-badge size="sm" variant="light">About Us <b-icon icon="people-fill" aria-hidden="true"></b-icon></b-badge></router-link> |
     
       <b-dropdown size="sm" :text="userProfile.username"  right variant="outline-light">

          <b-dropdown-item class="list-group-item"> 
            <b-icon icon="person"></b-icon><small class="text-muted"> Name </small>
            <h5>{{ userProfile.fname }} {{ userProfile.mname }} {{ userProfile.lname }}</h5>
          </b-dropdown-item>
         
          <b-dropdown-item class="list-group-item">
             <b-icon icon="bookmark-star"></b-icon><small class="text-muted"> Calling </small>
            <h5>{{ userProfile.org }} {{ userProfile.calling }}</h5>
          </b-dropdown-item>

           <b-dropdown-item class="list-group-item">
             <b-icon icon="house-door"></b-icon><small class="text-muted"> Ward</small>
            <h5>{{ userProfile.ward }}</h5>
          </b-dropdown-item>

           <b-dropdown-item class="list-group-item">
             <b-icon icon="flag"></b-icon><small class="text-muted"> Stake</small>
            <h5>{{ userProfile.stake }}</h5>
          </b-dropdown-item>
        
           <b-dropdown-item class="list-group-item">
              <b-btn id="logout" variant="outline-info" style="width:100%" size="lg" @click="logout()"><b-icon icon="door-open-fill"></b-icon>logout</b-btn>
          </b-dropdown-item>

        </b-dropdown> 
      
      </div>
    </section>
  </header>
  <br>
  <br>

    <b-jumbotron
      id="jumbo"
      :header="orgName"
      :lead="wardName" 
      align="center"
      header-level="4"
    >
    </b-jumbotron>

<div>

   <div class="container">
        <LineChart :chart-data="datacollection"></LineChart> 
        <br>
        <br>
          <div class="row" style="text-align:left;">
              <caption style="margin-left:20px;">  
                <sup><b>Note</b> : If data won't reflect/show after submitting or updating, just click the refresh button. Data may not Sync in realtime due to internet data traffic . 
                <br>
                </sup>
              </caption>
          </div>
      <br>
      
        <div align="left">
         <b-btn variant="out-light" style="margin:5px" @click="refreshPage()"><b-icon icon="arrow-clockwise" aria-hidden="true"></b-icon><small> Refresh</small></b-btn>
          <b-btn variant="out-light" style="margin:5px" @click="fillData()"><b-icon icon="bar-chart-fill" aria-hidden="true" animation="fade"></b-icon><small> Update Data</small></b-btn>  
        </div>

   </div>
       <br>
       <br>
  <b-tabs content-class="mt-3" align="center">

    <!---------------------------------------------------------------- chart --------------------------------------------------------------------------->
    <b-tab>
      <template #title>
        <a><b-btn variant="outline"> <small>Attendance</small></b-btn></a>
      </template>
      

        <!---------------------------------------------------------------- list --------------------------------------------------------------------------->
        <br>
     <div class="container"> 

  <div class="row" style="text-align:left;">
      <caption style="margin-left:20px;">  
        <sup><b>Direction</b> : Check the box if the person is present in the current
         meeting.
         <br>

         <b>Note</b> : For incorrect name information, kindly delete and replace replace with new and accurate details.
         </sup>
      </caption>
  </div>
  <br>
  <br>

    <br>
    <br>
    <!---------------------------------------------------------------- calendar --------------------------------------------------------------------------->
    <div align="left">
        <b-form-datepicker 
        id="example-datepicker" 
        v-model="chartValue" 
        class="mb-2" 
        :min="minDate" 
        :max="maxDate" 
        :date-disabled-fn="dateDisabled" 
        locale="en" 
        size="lg"
        reset-button
        close-button
        placeholder="Choose a date"
        nav-button-variant="dark"
        :date-format-options="{ month: 'short', day: '2-digit', year: 'numeric'}"
        @context="onContext"
        >
        </b-form-datepicker>
    </div>
    <br>

    <div>
      <b-row style="text-align:center;">

            <table style="width:100%;">
              <thead> 
                <tr style="font-family:Helvetica;box-shadow:0px 1px 5px grey;height:50px;border-radius:25px;">

                  <th>Lastname</th>
                  <th>Firstname</th>
                  <th>Middlename</th>
                  <th><b-icon-check2-square></b-icon-check2-square></th>
                  <th>Delete</th>
                </tr>
              </thead>

              <br>
              <tbody>
                <tr v-for="(item,index) in getlistname" :key='item' class='rowInfo' style="height:40px;">

                    <td style="text-transform: capitalize;">{{item.lastname}}</td>
                    <td style="text-transform: capitalize;">{{item.firstname}}</td>
                    <td style="text-transform: capitalize;">{{item.mname}}</td>
                    

                    <td>
                      <b-form-checkbox
                        :id="item.lastname + item.firstname + item.mname"
                        v-model="checked"
                        :value="item.lastname + ', ' + item.firstname + ' ' + item.mname" 
                        :checked="isSelected" 
                        class = 'unchecked' 
                      >
                      </b-form-checkbox> 
                    </td>
                    <td class='text-center'>
                      <b-row>
                        <b-col>
                          <b-icon-trash v-b-tooltip.hover title="Delete this Person" @click="deletePerson(item,index)"></b-icon-trash>
                        </b-col>
                      </b-row>
                    </td>        
                </tr>
              </tbody>
            </table>
        </b-row>
      </div>

      <hr>

        <!----------------------------------------- start of add modal ----------------------------------------------->

    <div>
      <b-avatar
        id='addPersonAvatar' 
        v-b-modal.modal-center 
        v-b-tooltip.hover 
        variant="info" 
        title="click to add person"
        size="lg"
        >
        <b-icon-person-plus></b-icon-person-plus>
      </b-avatar>
      <br>
      <br>
     
      <b-modal v-if="modalShow" id="modal-center" title="Add Name" hide-footer centered>

            <b-form autocomplete="off">    
                <b-form-group><!------ add lastname -------->
                  <b-form-input
                    v-model="input.lname"
                    placeholder="lastname"
                    :maxlength="max" 
                    style="text-align:center"
                    >
                  </b-form-input>
                </b-form-group>

                <b-form-group><!------ add firstname -------->
                  <b-form-input
                    v-model="input.fname"
                    placeholder="firstname"
                    :maxlength="max"
                    style="text-align:center"
                    >
                  </b-form-input>
                </b-form-group>

                <b-form-group>
                    <b-form-input
                    v-model="input.mname"
                    placeholder="middlename"
                    :maxlength="max"
                    style="text-align:center"
                    >
                  </b-form-input>
                  
                </b-form-group>
            </b-form>
            <br>
            <b-btn class="addEditBtn" variant="outline-success" style="width:100%;margin:0px 0px 10px 0px;" @click="addperson()">save</b-btn>
            <b-btn variant="outline-warning" class="addEditBtn" style="width:100%" @click="clear()">clear</b-btn>
      </b-modal>
    </div>

    <br>
              <!----------------------------------------- End of add/edit modal ----------------------------------------------->


               <!----------------------------------------- Attendance Report ----------------------------------------------->
               <br>
               <br>
               <br>
               <br>
               <br>

    <div class="container">
      <b-row style="box-shadow:0px 0px 5px grey;border-radius:15px;background-color:whitesmoke">
       
        <b-col style="padding:50px">
           <h3 align="center"> Summary Details</h3>
          <h5 align="center"> Attendance Report : {{formatted}}</h5>
          <hr>
          <br>
              <ol align="left">
                <li v-for="chix in checked" :key="chix" style="font-family:Verdana ;text-transform: capitalize;">{{chix}}</li>
              </ol> 
              <br>
              <br>
              <br>
              <div align="center"> 
               <b-button id='btnsubmit' variant="outline-info" style="width:80%" @click="addDatesAndLogs()"><b-icon-cursor-fill></b-icon-cursor-fill> Submit</b-button>
              </div>
        </b-col>
      </b-row>
    </div>
</div>
    </b-tab>


 


    <b-tab title="Logs" >
      <template #title>
        <a><b-btn  variant="outline"> <small>Log Records</small></b-btn><!--log btn--></a>
      </template>
      <br>

        <b-row align="left">
          
          <b-col></b-col>
          <b-col cols="12" md="4" lg="4" sm="12">

              <caption style="margin-left:50px">  
                 <small class="muted"><b>Description:</b></small>
              </caption>
   
              <small class="muted" style="margin-left:50px">List of persons that are present in a certian sunday date.</small>
              <hr>
              <br>
            
              <ul class="logssRow">
                <li v-for="(logz,index) in getlogs.slice().reverse()" :key="logz" class="attendanceLogs" style="text-transform: capitalize">
                  <div id="dataLabelsModal">
                    <br>
                  
                    <h6><b>{{chartDatesForLogs[index]}}</b></h6> 
                  </div>
                  
                  <ol class="list-group">
                    <li v-for="logies in logz.present" :key="logies" class="list-group-item">
                      {{logies}}
                    </li>
                  </ol>
                </li>
              </ul>
          
          </b-col>
              <b-col></b-col>
        </b-row>
   

    </b-tab>
    <b-tab title="Attendee">
      <template #title>
        <a><b-btn variant="outline"> <small>Attendees</small></b-btn><!--attendee--></a>
      </template>
      <br>
        <b-row>
          <b-col></b-col>
          <b-col cols="12" md="4" lg="4" sm="12">

              <h6 class="muted" style="margin-left:50px">Person Activity Percentage Since {{chartDates[0]}}</h6>
              <br>
              <br> 
              <br>
              <br>
              


              <b-row align="center">
                      <b-col >
                       Names
                      </b-col>
                      <b-col>
                        <b-row align="center">
                          <b-col>
                            Attendance
                          </b-col>
                          <b-col>
                           % Ratio
                          </b-col>
                        </b-row>
                      </b-col>
                      
                    </b-row>
                  <hr>
                  
                    <b-row align="center">
                      <b-col>
                      <tr  v-for="sam in attendeesName" :key="sam" style="border-bottom: solid grey 1px;height:40px;">
                          <td style="text-transform: capitalize;">{{sam}}</td> 
                      </tr>
                      </b-col>

                      <b-col>
                        <b-row>
                          <b-col>
                          <tr  v-for="attendee in attendeesOccurance" :key="attendee" style="border-bottom: solid grey 1px;height:40px;">
                              <td style="text-transform: capitalize;"><h6>{{attendee}}<small> / {{getlogs.length}}</small></h6></td> 
                             
                          </tr>
                          </b-col>
                          <b-col>
                            <tr  v-for="attendee in attendeesOccurance" :key="attendee" style="border-bottom: solid grey 1px;height:40px;">
                               <td><h5 v-if="attendee/getlogs2.length*100 >= 61" style="color:green">{{(attendee/getlogs.length*100).toFixed(0)}} %</h5>

                               <h5 v-else-if="attendee/getlogs2.length*100 >= 40" style="color:orange">{{(attendee/getlogs.length*100).toFixed(0)}} %</h5>
                             
                               <h5 v-else-if="attendee/getlogs2.length*100 <= 40" style="color:#DF3027">{{(attendee/getlogs.length*100).toFixed(0)}} %</h5>
                               
                               
                               </td>  
                            

                              
                        
                               
                            </tr>
                          </b-col>
                        </b-row>
                      </b-col>
                    </b-row>
          </b-col>
          <b-col></b-col>
        </b-row>
     
    </b-tab>

  </b-tabs>
</div>
    <!--------------------------- end main content ----------------------------->
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>

    <div>
      <b-row >
            <b-col></b-col>
            <b-col cols="10" md="8" lg="6" align="center">
                <p  style="font-size:20px"><i>" When performance is <b>measured</b>, performance <b>improves</b>. When performance is measured and <b>reported</b>, the rate of improvement <b>accelerates</b>. "</i></p>
                <br>
                <p ><i>—  President Thomas S. Monson</i></p>
            </b-col>
            <b-col></b-col>
        </b-row> 
    </div>
    <br>
    <br>
    <br>

    <footer1/>

  </div>
</template>

<script>

import { mapState } from 'vuex'
import moment from 'moment'
import LineChart from '../dataChart.js'
import footer1 from '../components/footer.vue'

import  fb from '../attendancefirebase.js'
import firebase from 'firebase'
import Swal from 'sweetalert2'

export default {
   name: "Home",

  components: {
    LineChart,
    footer1
    
  },
 
   filters: {
    formatDate(val) {
      if (!val) { return '-' }
      
      let date = val.toDate()
      return moment(date).fromNow()
    },
    trimLength(val) {
      if (val.length < 200) { return val }
      return `${val.substring(0, 200)}...`
    }
  },

  data() {
      const now = new Date()
      const today = new Date(now.getFullYear(), now.getMonth(), now.getDate())
      // 15th two months prior
      const minDate = new Date(today)
      minDate.setMonth(minDate.getMonth() - 6)
      minDate.setDate(0)
      // 15th in two months
      const maxDate = new Date(today)
      maxDate.setMonth(maxDate.getMonth() + 1)
      maxDate.setDate(0)


    return {
      datacollection:null,
      chartPetsa:[this.chartDates],
      getTao:[this.getPerson],

      //////////////////////////local vars
      
       items: [],
        input: {
          lname: '',
          fname: '',
          mname:'',
        },
        
        checked: [],
        unchecked: [],

        max: 36,
        maxAge: 2,

        savebtn: true,
        updatebtn: false,

        editMsg: false,
        addMsg: true,

        modalShow: true,//v-if use to open the same modal 
        modal2Show: true,//v-if use to open the same modal 

        isSelected: false,
        notSelected: true,

       
        logsChecked:[],

        variable:[],
        araw:[],

        x:null,


        axe:[],

        isLoggedIn:false,
        currentUsser:false,

        count:0,
        all:[],

        abc:[],

        sampless:[],


        random:'',
        mp:[],


        haha:null,
        attendeesOccurance:[],
        attendeesName:null,

        qwerty:[],

        realLogs:this.$store.state.userProfile.logs,
        mvp:[],
        chartValue:'',
        minDate: minDate,
        maxDate: maxDate,
        formatted: '',
        selected: '',
        xxxx:[],
       
       


 

    

    }
  },
    computed: {
      ...mapState(['userProfile','posts']),

      wardName () {
      return this.$store.state.userProfile.ward + " Ward"
      },
      orgName () {
      return this.$store.state.userProfile.org + " Organization"
      },
      chartDates(){
       return this.$store.state.userProfile.chartdates
      },
      chartDatesForLogs(){
       let a = this.$store.state.userProfile.chartdates
      return a.slice().reverse();
      },
      getlistname(){

       return this.$store.state.userProfile.listname
 
      },
       getlogs(){
       return this.$store.state.userProfile.logs
    
      },
       getChartDataValue(){
       return this.$store.state.userProfile.logs.map(function(s){
          return s.present.length;
      })
      },
      logsDate(){

          let xoxo = this.$store.state.userProfile.logs;
         let oxox = xoxo.slice().reverse();
       return oxox.map(function(s){
          return s.date_Entered.seconds;
      })
      },
      getlogs2(){
         return this.$store.state.userProfile.logs.map(function(s){
          return s.present;
      })
      }
       
    },
    mounted () {
      this.fillData()
    },
    methods:{
      onContext(ctx) {
        // The date formatted in the locale, or the `label-no-date-selected` string
        this.formatted = ctx.selectedFormatted
        // The following will be an empty string until a valid date is entered
        this.selected = ctx.selectedYMD

        
      },
      
      dateDisabled(ymd, date) {
      const weekday = date.getDay();
      return weekday >= 1 //&& weekday <= 5
     
    },

       fillData () {

          let result = {}
          let qwerty = this.$store.state.userProfile.logs

 //convert to key value pairs
                for (var i = 0; i < qwerty.length ;i++) {
                  result [qwerty[i].present] = qwerty[i].present;
                }
             
             //convert to display only the value, not the key or entries
               // let cge = Object.values(result);
                this.mp = Object.values(result);

                //  let dog = this.mp;
                 let dog = this.mp;

                 for (var o = 0; o < dog.length; o++) {
                  this.qwerty.push(dog[o].length);

                 }

              // convert from array of array into one array
              let getlogs2 = this.getlogs2;
                let dili = getlogs2.flat(1);
                this.xxxx = dili;

                this.haha = dili

                function foo(dili) {
                    var a = [],
                      b = [],
                      prev;
                    dili.sort();
                    for (var i = 0; i < dili.length; i++) {
                      if (dili[i] !== prev) {
                        a.push(dili[i]);
                        b.push(1);
                      } else {
                        b[b.length - 1]++;
                      }
                      prev = dili[i];
                    }
                    return [a, b];
                  }
                  var hihi = foo(dili);
                
                  this.attendeesName = hihi[0]
                  this.attendeesOccurance = hihi[1]

        
       // let dog = this.realLogs;
        let userLogs = this.$store.state.userProfile.logs
        for (var ii = 0; ii < userLogs; ii++) {
                  this.mvp.push(userLogs[ii].present.length);
                  

                 }

        this.datacollection = {
           labels:this.chartDates,
            datasets: [{   
                a:this.checked,
                borderColor:'lightblue',
                borderDashOffset:.1,
                pointBorderColor:'white',
                pointBackgroundColor:'skyblue',
                pointRadius: 5,
                pointHoverRadius: 10,
                label:'Attendance',
                events:onclick,
                data:this.getChartDataValue
              
              }], 
        }

       },
      
    addDates(){       
            this.$store.dispatch('addDates',this.formatted)
        
    },
     log(){
       this.$store.dispatch('log', this.checked,this.chartValue);
       this.checked = [];
    
    },
    


//////////////////////////////////////////////////////////////////////////////////////////////////////   sumbitting report


    addDatesAndLogs(){
     
      if(this.chartValue == ''){// dapat hindi empty ang petsa at
          Swal.fire({
              title:'Please Choose a Date',
              icon: 'warning',
              timer:2000,
              showCancelButton: false,
              showConfirmButton: false
            })
      }
      else if(this.chartDates.includes(this.formatted)){
          Swal.fire({
              title:' Please Choose Another Date ',
              text:'You Had Already Submitted your '+ this.formatted + ' Report',
              icon: 'error',
              //timer:2000,
              showCancelButton: false,
              showConfirmButton: true
            })
      }
      else{
       Swal.fire({
          title: 'Confirmation!',
          text: 'Please Confirm if you are sure with Attendance Report',
          icon: 'warning',
          showCancelButton: true,
          confirmButtonText: 'Yes',
          cancelButtonText: 'Go back'
        }).then((result) => {
          if (result.value) {

        if(this.chartValue != '' && this.checked.length != 0){//kung may binigay na petsa, okay lang na walang attendance
         this.addDates();
         this.log();
         this.chartValue = ''
          Swal.fire({
              title:'Report Submitted Successfully!',
              icon: 'success',
              timer:1000,
              showCancelButton: false,
              showConfirmButton: false
            })
       }
       else if(this.chartValue != '' && this.checked.length == 0){//may petsa at attendance
         this.addDates();
         this.log();
         this.chartValue = ''

         Swal.fire("This page will refresh to sync with database")
          .then(() => {
             location.reload();
          });
       }
              
          }else if (result.dismiss === Swal.DismissReason.cancel) {
            Swal.fire({
              title:'Report Cancelled',
              icon: 'error',
              timer:1000,
              showCancelButton: false,
              showConfirmButton: false
            })
          }
        })
      }
      

    },
  
     addperson() {
    

       if(this.input.lname == '' || this.input.fname ==''){
          Swal.fire({
          title: 'Something Missing!',
          text:'Person`s lastname and firstname is required',
          icon: 'error',
          showCancelButton: false,
          showConfirmButton: true
        })
       }
       else{
        Swal.fire({
          title: 'Person has been Added',
          icon: 'success',
          timer:1000,
          showCancelButton: false,
          showConfirmButton: false
        })

      this.$store.dispatch('addperson', {
        lastname: this.input.lname,
        firstname: this.input.fname,
        middlename: this.input.mname,
      })
  
      this.clear();
       }
    },

    a(){
      console.log(this.getlistname.length)
      console.log(this.getlistname)
    },

    deletePerson(item,index){// this function is not pass to store

     Swal.fire({
          title: 'Are you sure?',
          text: 'You cannot retrieve person`s info once deleted',
          icon: 'warning',
          showCancelButton: true,
          confirmButtonText: 'Yes, delete it!',
          cancelButtonText: 'No, keep it'
        }).then((result) => {
          if (result.value) {

                // delete person locally
                // this.getlistname.splice(id,1);// javascript way, it works!
                this.$delete(this.getlistname, index); // vue js way , it works!

                // delete person in db
                const user  = fb.auth.currentUser.uid
                fb.usersCollection.doc(user).update({
                  listname:firebase.firestore.FieldValue.arrayRemove(item,index) 
              })

            Swal.fire({
              title: 'Person Deleted!',
              text: 'This person has been deleted.',
              icon: 'success',
              timer:1000,
              showCancelButton: false,
              showConfirmButton: false 
            })

          }else if (result.dismiss === Swal.DismissReason.cancel) {
            Swal.fire({
              title:'Cancelled',
              text: 'Person`s info stays :)',
              icon: 'error',
              timer:1000,
              showCancelButton: false,
              showConfirmButton: false
            })
          }
        })

    },
    
    clear(){
      this.input.lname = ''
      this.input.fname = ''
      this.input.mname = ''
    },
   
   
    firstDayOfWeek(){

      function getMonday(d) {
        d = new Date(d);
        var day = d.getDay(),
            diff = d.getDate() - day + (day == 0 ? -6:0); // adjust when day is sunday:0
        return new Date(d.setDate(diff));
      }

      getMonday(new Date()); // Mon Nov 08 2010
      console.log(getMonday(new Date()));
    },

      logout() {
      Swal.fire({
          title: 'Are you sure to logout?',
          text: 'You will be directed to login page',
          icon: 'warning',
          showCancelButton: true,
          confirmButtonText: 'Yes',
          cancelButtonText: 'No'
        }).then((result) => {
          if (result.value) {

            this.$store.dispatch('logout')
              
          }else if (result.dismiss === Swal.DismissReason.cancel) {
           console.log("cancel logout")
          }
        })

     
    },

    refreshPage(){
      location.reload();
    }

    }
   





}
</script>
<style scoped>
.rowInfo:hover{
  box-shadow: 0px 0px 2px grey;
  border-radius:10px;
  cursor:pointer;
}
ul li{
 list-style-type: none;
}
.attendanceLogs{
   border-radius:10px;
   margin:5px;
   padding:15px;
   box-shadow:0px 0px 7px grey;
   font-style:bold;
}
.addAnothePerson{
  margin:5px 0px 5px 0px;
}
#logout{
  cursor: pointer;
}
header{
   padding: 17px;
}
#jumbo{
  text-transform: capitalize;
  background-image: url('../assets/flock3.png');
  color:black;
 
 
  
}
.list-group-item{
  text-transform: capitalize;
  
}


</style>