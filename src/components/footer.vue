<template>
<div class="bg-dark" style="color:white">
  <div class="container">
  <hr>
  <br>
    <b-row>
      <b-col>
         <h5 style="font-family:courier new">A Message from the Developer</h5>
         <br>
         <div align="left">
            <div style="text-transform: capitalize;">Good Day {{name.fname}} {{name.lname}},</div>
            <br>
            We appreciate your dedication in magnifying your church calling. With this web application, you can be able to list, monitor and track progress of your organization's attendance.
            However, some features of this application is reserved for future development.<br><br>For concerns in using this application, feel free to contact us.
         </div>
      </b-col>
       <b-col>
         <h5 style="font-family:courier new">Contact</h5>
         <br>
         <br>
          <b-icon icon="envelope-fill"  variant="light" style="width:30px;height:30px"></b-icon><br><p><i>apparatedev@gmail.com</i> </p>
            <br>
          <b-icon icon="phone-fill" style="width:30px;height:30px"></b-icon><br> <p><i> 63+ 9686106544</i></p>
      </b-col>
    </b-row>

     <!-- <img src="../assets/logo2.png" style="width:100px"/> -->

     <br>
  </div>
   <div style="background-color:whitesmoke;color:black">
        <small class="copyright-text">Copyright &copy; 
          <a><b-icon icon="code-slash"></b-icon> <b>App</b>arate</a> 2020
        </small>
    </div>
  </div>
</template>

<script>

export default {
  
 
  data() {
    return {

    }
  },
  computed:{
     name(){
       return this.$store.state.userProfile
 
      },
  },
};
</script>

<style scoped>


</style>

