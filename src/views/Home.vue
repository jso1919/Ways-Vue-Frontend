<template>
  <div>
    <Header/>
    <section class="hero">
      
      <h1 class="hero-title">SELECT A SERVICE</h1>
      <div class="top-btn">
        <a href="https://app.waysfl.com/#/" target="_blank" class="top-btn__container">
          <div class="top-btn__fourpoints"></div>
          <div class="top-btn__rightside">
            <span class="top-btn__title">CRUISE PORT SHUTTLE</span>
            <span class="top-btn__subtitle">1800 SOUTH FEDERAL HWY <br> FORT LAUDERDALE, FL 33316</span>
          </div>
        </a>        
      </div>
      <div class="bottom-section">
        <a v-scroll-to=" '#book-a-ride' " class="bottom-section__btn" href="#">
          <h1>PORT OF MIAMI</h1>
        </a>
        <a v-scroll-to=" '#book-a-ride' " class="bottom-section__btn" href="#">
          <h1>MIAMI AIRPORT</h1>
        </a>
      </div>
      <div class="bottom-section">
        <a v-scroll-to=" '#book-a-ride' " class="bottom-section__btn" href="#">
          <h1>PORT EVERGLADES</h1>
        </a>
        <a v-scroll-to=" '#book-a-ride' " class="bottom-section__btn" href="#">
          <h1>FT LAUDERDALE AIRPORT</h1>
        </a>
      </div>
    </section>

    <section class="book-a-ride" id="book-a-ride">
      <div class="book-a-ride__city"></div>
      <div class="book-a-ride__content">
        <form class="book-a-ride__form" @submit="submitForm">
          <div class="book-a-ride__formlabel">contact us </div>
            <div class="book-a-ride__formcontent">

              <div class="book-a-ride__formfield">
                <h5>FIRST NAME</h5>
                <input type="text" v-model="contactForm.name">
              </div>
              <!-- <div class="book-a-ride__formfield">
                <h5>LAST NAME</h5>
                <input type="text" v-model="contactForm.lastName">
              </div> -->
              <div class="book-a-ride__formfield">
                <h5>EMAIL</h5>
                <input type="text" v-model="contactForm.email">
              </div>
              <div class="book-a-ride__formfield">
                <h5>PHONE</h5>
                <input type="text" v-model="contactForm.phone">
              </div>
              <div class="book-a-ride__textarea">
                <h5>MESSAGE</h5>
                <input type="text-area" rows="9" cols="50" v-model="contactForm.message">
              </div>
              
              <div class="book-a-ride__oneway">
                <button v-if="!loading" class="btn btn-warning" :disabled="contactForm.name && contactForm.email && contactForm.phone && contactForm.message ? (false) : (true)">SEND</button>
                <button v-if="loading" class="btn btn-warning" >LOADING <i class="fa fa-spinner fa-spin"></i></button>
              </div>
              <p class="text-center book-a-ride__lowertext col-12">Ways nonstop shuttle will take you directly to your destination. Different from share ride, you do not share the ride with other passengers. Good for families, friends and large groups.</p>
            
            </div>
        </form>
        <h1 class="book-a-ride__slogan">WE ARE THE BRIDGE TO YOUR DESTINATION</h1>
      </div>
    </section>

    
  
    <section class="about container" id="about-us">
      <h1>about ways</h1>
      <p>Ways Shuttle Solutions is a web/mobile application designed for you to book shuttle services. Ways is built to help you experience reliable, flexible and convenient shuttle service to your destination.</p>
      <div class="about-features container">
        <div class="about-features-1">
          <img src="../../src/assets/img/shield.png" alt="">
          <h1>SAFETY FIRST</h1>
          <p>All affiliated ground transportation companies are licensed and insured.  
          <br>Drivers must pass extensive background checks and pass training protocol</p>
        </div>
        <div class="about-features-1">
          <img src="../../src/assets/img/24-hours.png" alt="">
          <h1>AVAILABLE 24/7</h1>
          <p>Pre-arranged private transportation is available 24/7</p>
        </div>
        <div class="about-features-1">
          <img src="../../src/assets/img/acreditations.png" alt="">
          <h1>ACREDITATIONS</h1>
          <p>We are a start-up company, in the process of building trustworthy acreditations.</p>
        </div>
      </div>
    </section>

    <section class="airport-trans">
      <div class="airport-trans__text">
        <p>Proper <b>planning</b> and preparation prevents a </br> poor travel experience.</p>
        <b class="airport-trans__hash"> #dontbethepersoninthemiddleofthestreet</b>
        <br>  </br>
        <br>  </br>
        <p>Use any mobile device and schedule your</p> 
        <a v-scroll-to=" '#book-a-ride' " href="#" class="airport-trans__btn">
          <span>ride now !</span>
        </a> </p>
      </div>
    </section>
    
    <!-- <Payment/> -->
    <!-- <CustomAddress/ -->

    <Footer/>
  </div>
</template>

<script>
import axios from 'axios'

import Header from '@/components/Header'
import Footer from '@/components/Footer'
import * as config from '@/config/settings'
import Payment from '@/views/Modal/Payment'
// import CustomAddress from '@/views/Modal/CustomAddress'
import { serverBus } from '@/main'

export default {
  name: 'Home',
  components: {
    Footer,
    Header,
    Payment,
    // CustomAddress
  },

  data(){
    return{
      contactForm: {
        name: " ",
        email: " ",
        phone: " ",
        message: " ",
        
      },
      form: {
        round_trip: false
      },
      loading: false,
      destinyPoints:[],
      picker: new Date().toISOString().substr(0, 10),
      landscape: false,
      reactive: false,
      show: false,

    }
  },

  // created(){
  //   this.fetchDropOff()
  // },

  methods: {
    // fetchDropOff(){
    //   axios({
    //     url: config.defaultURL + '/points?type=12',
    //     method:'get',
    //     data:this.form,
    //     headers: {
    //       "content-type": "application/json"
    //     }
    //   })
    //   .then((response) => {
    //     if(response.status === 200) {
    //       this.destinyPoints = response.data
    //     }
    //   })
    //   .catch((error) => {
    //     error.response.data.map((m,index) => {
    //       this.loader = false
    //       this.$toasted.show(m, {
    //         position:'top-right',
    //         duration: 5000,
    //         type: 'error',
    //         closeOnSwipe: true
    //       })
    //     })
    //   })
    // },

    submitForm(evt){
      evt.preventDefault()
      // this.loading = true
      axios({
        url: config.defaultURL + '/contact_forms',
        method: 'post',
        data: this.contactForm,
        headers: {
          "content-type": "application/json"
        }
      })
       .then(response => {
         this.$toasted.show('Thank you for contacting contact')({
           position: 'bottom-center',
           duration: 1000,
           type: 'success',
           closeOnSwipe: true
         })

        // if(response.status === 200) {
        //   this.contactForm = response.data
        // }
      })
      // .catch((error) => {
      //   error.response.data.map((m,index) => {
      //     this.loader = false
      //     this.$toasted.show(m, {
      //       position:'top-right',
      //       duration: 5000,
      //       type: 'error',
      //       closeOnSwipe: true
      //     })
      //   })
      // })
      this.contactForm = [ ]
    },

    // createRide(evt){
    //   evt.preventDefault()
    //   this.loading = true
    //   axios({
    //     url: config.defaultURL + '/rides',
    //     method:'post',
    //     data:this.form,
    //     headers: {
    //       "content-type": "application/json"
    //     }
    //   })
    //   .then((response) => {
    //     if(response.status === 201) {
    //       this.loading = false
    //       localStorage.setItem('currentRide', JSON.stringify(response.data))
    //       serverBus.$emit('openPayment', response.data.ride_price)
    //     }
    //   })
    //   .catch((error) => {
    //     error.response.data.map((m,index) => {
    //       this.loading = false
    //       this.$toasted.show(m, {
    //         position:'top-right',
    //         duration: 5000,
    //         type: 'error',
    //         closeOnSwipe: true
    //       })
    //     })
    //   })
    // }
  }
}
</script>