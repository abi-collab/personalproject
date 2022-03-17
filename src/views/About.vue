<template>
  <div>
    <div class="container">
      <br>
      <br>
        <div align='left'>
          <router-link to="/"> <b-icon icon="box-arrow-in-left" aria-hidden="true" font-scale="4"></b-icon></router-link>
        </div>
        <div>
          <b-jumbotron style="background-color:white">
            <template #header>About Us</template>
      
          <hr class="my-4">
          <br>
        <b-row>
          <b-col></b-col>
          <b-col md="6" sm="12">
            <p>
              APParate IT Solutions was established on 2020 which is a team of imaginative personalities who thinks often much about individuals who needs a hand in meeting there particular technical needs. 
              <br><br>We create dynamic and progressive web applications that can be practically run in almost operating systems and even accessible in phones, tablets and PC's .   
              <br><br>Using the latest updates of today's technological trends in the web development industry, we are free available for hire in a scope of innovative disciplines for a variety of jobs and projects.
            </p>
          </b-col>
          <b-col></b-col>
        </b-row>
        </b-jumbotron>
      </div>
    </div>
          <br>
          <br>
          <br>
          <br>
          <br>
          <footer1/>

  </div>
</template>

<script>
import footer1 from '../components/footer'
export default {
  components:{
    footer1
  }
  
}
</script>
