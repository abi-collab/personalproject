<template>
<div>
  <div class="container">
    <br>
    <div>
      <b-jumbotron style="background-color:white;">
        <template #header>The Shepherd</template>
        <template #lead>
          “How think you? if a man have an hundred sheep, and one of them be gone astray, does he not leave the ninety and nine, and goes into the mountains, and seeks that which is gone astray?.”  &#8213; Mat 18:12–14
        </template>
        <hr class="my-4">
      </b-jumbotron>
    </div>
    

    <!----------------------- login header ends ------------------------------->

    <b-row>
      <b-col></b-col>
      <b-col cols="12" lg="6" md="8">
        <b-card id="card" header-tag="header" footer-tag="footer">
        <section>
          <div :class="{ 'signup-form': !showLoginForm }" class="col2">

            <!----------------------- login ------------------------------->
            <form v-if="showLoginForm" @submit.prevent>  
              <h1>Log In</h1>
              <small>Start Organizing Attendances</small>
              
             
              <br>
              <br>
              <br>

              <b-form-group
                align="left"
                label="Enter your email"
                label-for="emails"
               >
                <b-form-input id="emails" v-model.trim="loginForm.email" type="text" placeholder="you@gmail.com"></b-form-input>
              </b-form-group>
              

              <b-form-group
                align="left"
                label="Enter your Password"
                label-for="pass"
                >
                <b-form-input id="pass" v-model.trim="loginForm.password" type="password" placeholder="***********"></b-form-input>
              </b-form-group>
          
             
              <b-btn id="login" variant="info" @click="login()">Log In</b-btn>
              <br>
              <br>
             

                                 <!---------------------- password reset ----------------------------->

              <div class="extras"> 
                <a v-b-modal.modal-1 class="link">Forgot Password?</a> | 
                <a class="link" @click="toggleForm()">Create an Account</a>

                 <b-modal id="modal-1" centered hide-footer>

                    <h3>Reset Password</h3>
                    <div v-if="!showSuccess">
                      <p>Enter your email to reset your password</p>
                      <b-form @submit.prevent>
                        <b-form-input v-model.trim="email" type="email" placeholder="you@email.com" />
                      </b-form>
                      <b-btn style="margin-top:10px;width:100%;" variant="outline-warning" @click="resetPassword()">Send</b-btn> 
                    </div>

                  <p v-else>Success! Check your email for a reset link. It may take a couple of minutes to receive the request.</p>
                  <p v-if="errorMsg !== ''" class="error">{{ errorMsg }}</p>
                  <br>
                </b-modal>
              </div>

               <!---------------------- password reset ----------------------------->
            </form>


            <!---------------------- sign up ----------------------------->

            <form v-else @submit.prevent>
              <h1>Sign Up!</h1>
              <small>Create an Account & Start Organizing Attendance</small>
              <br>
              <br>
              <br>
                <!---------------------------------------------  name Details  --------------------------------------->
                <b-card header="Name" class="text-center">

                    <b-form-group label="Lastname" label-for="Lastname" align="left">
                      <b-form-input id="Lastname" v-model.trim="signupForm.lname" type="text" required></b-form-input>
                    </b-form-group>
                     <b-form-group label="Firstname" label-for="Firstname" align="left">
                      <b-form-input id="Firstname" v-model.trim="signupForm.fname" type="text" required></b-form-input>
                    </b-form-group>
                     <b-form-group label="Middlename" label-for="Middlename" align="left">
                      <b-form-input id="Middlename" v-model.trim="signupForm.mname" type="text"></b-form-input>
                    </b-form-group>

                </b-card>
                <br>

                <!---------------------------------------------  Lds Membership Details  --------------------------------------->
               <b-card header="Lds Membership Details" class="text-center">

                  <b-form-group label="Stake" label-for="stake" description="Example : Cavite, CDO East, Cadiz" align="left">
                    <b-form-input id="stake" v-model.trim="signupForm.stake" type="text"></b-form-input>
                  </b-form-group>

                  <b-form-group label="Ward/Branch" label-for="ward" description="Example : Noveleta, General Trias 1st, Manolo Fortich " align="left">
                    <b-form-input id="ward" v-model.trim="signupForm.ward" type="text"></b-form-input>
                  </b-form-group>

                  <b-form-group label="Calling" label-for="calling"  align="left">
                    <!-- <b-form-input id="calling" v-model.trim="signupForm.calling" type="text"></b-form-input> -->
                     <b-form-select id="calling" v-model.trim="signupForm.calling" :options="callings"></b-form-select>
                  </b-form-group>

                  <b-form-group label="Organization" label-for="organization" description="If your calling or organization is not in the choices above, just pick a temporary one, and feel free to contact the developer about this concern." align="left">
                    <!-- <b-form-input id="org" v-model.trim="signupForm.org" type="text" ></b-form-input> -->
                     <b-form-select id="org" v-model.trim="signupForm.org" :options="org" ></b-form-select>
                  </b-form-group>

               </b-card>
               <br>

                <!--------------------------------------------- Authentication Details  --------------------------------------->
               <b-card header="Authentication Details" class="text-center">
                   <b-form-group label="Email" label-for="email2" description="Please Enter a valid Email Address to avoid delays. Example : you@email.com" align="left">
                      <b-form-input id="email2" v-model.trim="signupForm.email" type="text"></b-form-input>
                    </b-form-group>

                     <b-form-group label="username" label-for="username" align="left">
                        <b-form-input id="username" v-model.trim="signupForm.username" type="text"></b-form-input>
                      </b-form-group> 

                    <b-form-group label="Password" label-for="password2" description="min of 6 characters" align="left">
                      <b-form-input id="password2" v-model.trim="signupForm.password" type="password"></b-form-input>
                    </b-form-group>

               </b-card>

              <b-btn style="width:100%" variant="info" @click="signup()">Sign Up</b-btn>
              <br>
              <br>

              <div class="extras">
                <a @click="toggleForm()"><b-icon icon="arrow-return-left" aria-hidden="true"></b-icon>Back to Log In</a>
              </div>
            </form>
           
          </div>
        </section>

        </b-card>

 

      </b-col>
      <b-col></b-col>
    </b-row>
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

  </div>
  <footer1/>

  </div>
</template>

<script>
// import firebase from 'firebase';
//  import passwordReset from './passwordReset';
 import { auth } from '../attendancefirebase'
 import  fb from 'firebase'
 import Swal from 'sweetalert2'
 import footer1 from './footer.vue'


export default {
   name: "Login",
  components: {
    footer1
  },
  data() {
    
    return {
      // email: "",
      // password: ""
       loginForm: {
        email: '',
        password: ''
      },
      signupForm: {
        lname:'',
        fname:'',
        mname:'',

        username: '',
        email: '',
        password: '',
        petsa:'',

        stake:'',
        ward:'',
        calling: '',
        org:''
      },
      showLoginForm: true,
      // showPasswordReset: false,

      //////////////////////// for password reset props
      email: '',
      showSuccess: false,
      errorMsg: '',
      value:'',

      callings: [     
          { value: 'Bishop', text: 'Bishop' },
          { value: '1st Councilor', text: '1st Councilor' },
          { value: '2nd Councilor', text: '2nd Councilor' },
          { value: 'Adviser', text: 'Adviser' },
          { value: 'Leader', text: 'Leader' },
          { value: 'Consultant', text: 'Consultant' },
          { value: 'Secretary', text: 'Secretary' },
          { value: 'Ward Clerk', text: 'Ward Clerk' },
          { value: 'Ward Executive Secretary', text: 'Ward Executive Secretary' },
          { value: 'Ward Assistant Clerk - Finance', text: 'Ward Assistant Clerk - Finance' },

        ],
        org:[
          { value: 'Bishopric', text: 'Bishopric' },
          { value: 'Elders Quorum', text: 'Elder`s Quorum' },
          { value: 'Relief Society', text: 'Relief Society' },
          { value: 'Young Men', text: 'Young Men' },
          { value: 'Young Women', text: 'Young Women' },
          { value: 'Primary', text: 'Primary' },
          { value: 'Ward Temple and Family History', text: 'Ward Temple and Family History' },
        ]
    }
  },
  methods: {


    toggleForm() {
      this.showLoginForm = !this.showLoginForm
    },
    // togglePasswordReset() {
    //   this.showPasswordReset = !this.showPasswordReset
    // },
    login(e) {

       try{

         fb.auth().signInWithEmailAndPassword(this.loginForm.email,this.loginForm.password).then(
          currentUser =>{

          this.$store.dispatch('login', {
          email: this.loginForm.email,
          password: this.loginForm.password
          })


          console.log(currentUser);
           
          const Toast = Swal.mixin({
          toast: true,
          position: 'center',
          showConfirmButton: false,
          timer: 4000,
          timerProgressBar: true,
          onOpen: (toast) => {
            toast.addEventListener('mouseenter', Swal.stopTimer)
            toast.addEventListener('mouseleave', Swal.resumeTimer)
          }
        })   
          Toast.fire({
              icon: 'success',
              title: 'Signing In as ' + this.loginForm.email + ', Data Syncing...'
            })    
          },
          err =>{
           // alert(err);
             Swal.fire({
              title:err.message,
              icon:'warning',
              showCancelButton: false,
              showConfirmButton: true
              }) 
           
          } 
        )
       }catch{
          e.preventDefault();

       }
    },
    signup() {

      //this.petsafunction();

      this.$store.dispatch('signup', {
        email: this.signupForm.email,
        password: this.signupForm.password,

        username: this.signupForm.username,

        lname: this.signupForm.lname,
        fname: this.signupForm.fname,
        mname: this.signupForm.mname,
        petsa:this.signupForm.petsa,

        stake:this.signupForm.stake,
        ward:this.signupForm.ward,
        calling: this.signupForm.calling,
        org:this.signupForm.org

      })
    },

    async resetPassword() {
      // reset logic
      this.errorMsg = ''

        try {
            await auth.sendPasswordResetEmail(this.email)
            this.showSuccess = true
        } catch (err) {
            this.errorMsg = err.message
        }
    },

    // petsafunction(){

    //   let endOfWeek = function(date){
        
    //     var lastday = date.getDate() - (date.getDay() - 1) + 6;
    //     return new Date(date.setDate(lastday));
    //   }

    // let dt = new Date(); 

    // const months = ["Jan", "Feb", "Mar","Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"];

    // let x = endOfWeek(dt);
    // let a = x.getDate();
    // //let b = x.getMonth() + 1;
    // let b = months[x.getMonth()];
    // let c = x.getFullYear();

    // let fixdate = b +" "+ a +", " + c;

    // console.log(fixdate);
    // this.signupForm.petsa = fixdate;

    // },


  }
};
</script>

<style>
#login,
#signUp {
  width: 100%;
}
.link{
  cursor: pointer;
}
#card{
  border-radius:10px;
  background-color: white;
}
</style>
