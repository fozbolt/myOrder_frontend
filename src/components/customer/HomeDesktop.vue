<template>
    <div id="homepageBody">
      <!-- tost - plan: turn into component-->
      <div class="toast-container position-fixed top-0 end-0 p-3">
          <div id="basicToast" ref="basicToast" class="toast" role="alert" aria-live="assertive" aria-atomic="true">
              <div 
                  class="toast-header text-light" 
                  :style="toastMessage !== 'Thanks for subscribing' ?  'background-color:red' : 'background-color:green' ">
                  
                  <img v-if="toastMessage === 'Thanks for subscribing'" id="successIcon" src="@/assets/successIcon.png"/>
                  <h6 class="my-0">{{toastMessage}}</h6>
              </div>
          </div>  
      </div>
  
        <!-- Modal - plan: make as component -->
           <div class="modal fade" id="staticBackdrop" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
              <div class="modal-dialog  modal-dialog-centered">
                  <!-- Modal content-->
                  <div class="modal-content">
                    <div class="modal-header">
                      <h3>Enter your email and enjoy our offers</h3>
                    </div>
                    <div class="modal-body"> 
                          <div class="form-floating mb-3">
                            <input v-if="!invalidInput" v-model="subscriberEmail"  type="email" class="form-control" id="floatingInput" placeholder="name@example.com">
                            <label v-if="!invalidInput" for="floatingInput" >Email address</label>
  
                             <input v-if="invalidInput"  v-model="subscriberEmail"  type="email" class="form-control is-invalid" id="floatingInputInvalid" placeholder="name@example.com">
                             <label v-if="invalidInput" for="floatingInputInvalid">Invalid input</label>
                          </div>
                      </div>
                    <div class="modal-footer" >
                        <button @click="subscribe" type="button" class="btn btn-primary">Subscribe</button>
                        <button type="button" class="btn btn-default" data-bs-dismiss="modal">Cancel</button>
                    </div>
                  </div>   
              </div>
          </div>
  
      <div class="homepageContent" id="landingContent">
        <picture id="landingImage">
          <source media="(min-width: 900px)" srcset="@/assets/HeroMedium.png">
          <img src="@/assets/HeroMobile.png" alt="Default hero image">
        </picture>  
        <div id="landingText">
          <div>
          <div id="mainText" :class="{'blackText': scrollPosition < 200, 'whiteText': scrollPosition > 200}">
            <b>Future is here!</b> Be part of a revolution and order from our <i id="italicText">futuristic</i> app
          </div>
          <div id="subText"  ref="subText">
          <!--animation source: https://codepen.io/alvarotrigo/pen/ExvqdNa-->
           <h6>
                <!--<span v-for="word in subTextContent" :key="word">{{word}}</span>-->
                <span>Join</span>
                <span>us</span>
                <span>and </span>
                <span>tell</span>
                <span>the</span>
                <span>world</span>
                <span>all</span>
                <span>about</span>
                <span>it</span>
              
              </h6>
          </div>
        </div>
    
        <button  @click="this.$router.push('/food_list')" type="button" id="orderNowButton" class="btn btn-primary btn-circle btn-xl">
            Order now 
            <i class="fas fa-long-arrow-right" aria-hidden="true"></i>
            <!-- <i class="fa fa-arrow-right" aria-hidden="true"></i> -->
            <!-- <img src="@/assets/rightArrow.png" id="arrowIcon" alt="arrow icon"/> -->
          </button>
        </div> 
      </div>
       
      <div class="homepageContent" id="abouthomepageContent">
        <div class="header" id="descHeader">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmo
          <ul>
            <li> d tempor incididunt ut labore et dolore magna aliqua. </li>
            <li> Ut enim ad minim veniam, quis </li>
            <li> nostrud exercitation ullamco laboris </li>
          </ul>  
        </div>
      </div>
  
      <div class="homepageContent" id="abouthomepageContent2">
         <img src="@/assets/bulletMobile.svg" alt="About us image">
      </div>
  
      <div class="homepageContent" id="sliderDiv"  ref="aboutSection">
        <AboutCarousel></AboutCarousel>
        <!--collapsible-->
        <div id="checkOffersDiv" type="button" @click="toggleCollapsible()" aria-expanded="false" aria-controls="collapseExample" ref="checkOffers">
            <h5>Check out our top offers</h5>
            <img v-if="!collapsibleOpen" src="@/assets/seeMoreArrowDown.svg" alt="see more arrow">
            <img v-else src="@/assets/seeMoreArrowUp.svg" alt="see less arrow">
        </div>
        <div class="collapse" id="collapseExample">
            <div class="card card-body" id="cardBody">
              <div id="contentHolder">
                  <div class="topOffers" @click="goTo('Top offers')">
                      <img src="@/assets/discount.png" class="seeOffersIcon" alt="see offers icon">
                      <h5>Top offers</h5>
                  </div>

                  <div class="topOffers" @click="goTo('New offers')">
                    <img src="@/assets/newIcon.png" class="seeOffersIcon" alt="see offers icon">
                    <h5>New offers</h5>
                  </div>

                  <div class="topOffers" @click="goTo('Top offers')"> <!--not implemented yet-->
                    <img src="@/assets/happyHours.svg" class="seeOffersIcon" alt="see offers icon">
                    <h5>Our happy hours</h5>   
                  </div>
                
              </div>
            </div>
        </div>
  
        <img src="@/assets/PancakeMobile2.svg" alt="Pancake image" id="pancakeImg" >
      </div>
  
      <div class="homepageContent" id="subscribeDiv" ref="subscribeSection">
          <div id="subscribeContent" class="header">
              <h1>500</h1>
              <h5>Subscribers in our first month</h5>
              <small>Become one of them and recieve updates on promotions, discounts and happy hours</small>
              <button type="button" id="subscribeButton" @click="toggleModal">SUBSCRIBE</button>
            </div>
          <div id="subscribeImageDiv">
            <img src="@/assets/subscribeMobile.jpg" alt="Subscribe image">
          <div> 
  
           </div>
        </div>
      </div>
      
      <FloatingMenu />
  
      <Footer/>
    </div>
  </template>
  
  
  <script>
  import store from '@/store.js';
  import Footer from '@/components/Footer.vue';
  import FloatingMenu from '@/components/FloatingMenu.vue';
  import { Products } from '@/services';
  import AboutCarousel from '@/components/customer/InfoSlider.vue';
  import {emitter} from '@/main.js'
  
  export default {
      name: "HomeDesktop",
      components: { Footer, FloatingMenu, AboutCarousel },
  
      data() {
          return {
              store,
              scrollPosition: null,
              prevScrollPosition:0,
              subscriberEmail: '',
              invalidInput: false,
              toastMessage: '',
              collapsibleOpen: false
          };
      },
      methods: {
        updateScroll() {
          //for static color toggling its enough to do: :class="{'blackText': scrollPosition < 200, 'whiteText': scrollPosition > 200}"
       
          this.scrollPosition = window.scrollY
          const randomColor = Math.floor(Math.random()*16777215).toString(16);
  
          if (Math.abs(this.prevScrollPosition - this.scrollPosition) > 50){
              this.$refs['subText'].style.color = '#' + randomColor;
              this.prevScrollPosition = this.scrollPosition
          }
  
          if(this.scrollPosition === 0) this.$refs['subText'].style.color = '#161515';
          
        },
  
        toggleModal(){
            //refactor and use refs
              $("#staticBackdrop").modal("toggle");
        },
  
        validateEmail(email) 
        {
            //simple regex
            var re = /\S+@\S+\.\S+/;
            return re.test(email);
        },
  
        async subscribe(){
              if (this.validateEmail(this.subscriberEmail)){
  
                let success = await Products.addSubscriber(this.subscriberEmail)
                this.toggleModal();
  
                if (success) this.subscribeMessage('Thanks for subscribing');
                else this.subscribeMessage('Error while making subscription');
              }            
              else{
                this.invalidInput = true;
              }
          },
  
              subscribeMessage(message){
                this.toastMessage = message;
                let button = this.$refs['basicToast']
                new bootstrap.Toast(button).show();
              },
  
              checkForVisibility() {
                let headers = document.querySelectorAll(".header");
                headers.forEach(function(header) {
                
                if (this.isElementInViewport(header)) {
                  header.classList.add("headerVisible");
                } else {
                  header.classList.remove("headerVisible");
                }
                }, this);
              },
  
  
              isElementInViewport(el) {
                let rect = el.getBoundingClientRect();
  
                return (
                  //original: all set to 0
                  rect.top >= -200 &&
                  rect.left >= 0 &&
                  rect.bottom <=
                    (window.innerHeight || document.documentElement.clientHeight) &&
                  rect.right <= (window.innerWidth || document.documentElement.clientWidth)
                );
              },
  
  
              addEventListeners(){
                if (window.addEventListener) {
                  window.addEventListener('scroll',this.updateScroll, { passive: true} ); //Homepage text animations
                  window.addEventListener("DOMContentLoaded", this.checkForVisibility, false, { passive: true}); //About and subscribe section text animations
                  window.addEventListener("load", this.checkForVisibility, false, { passive: true}); //About and subscribe section text animations
                  document.addEventListener("scroll", this.checkForVisibility, false, { passive: true}); //About and subscribe section text animations
                }
              },
  
              removeEventListeners(){
                window.removeEventListener('scroll', this.updateScroll)
                window.removeEventListener('scroll', this.checkForVisibility)
                window.removeEventListener('DOMContentLoaded', this.checkForVisibility)
                window.removeEventListener('load', this.checkForVisibility)
              },
  
              goTo(subcategory){
                this.store.selectedSubCategory  = subcategory
                this.store.category = 'MainCourse'
                this.store.type = 'Food'
                this.$router.push('/food_list')
              },
  
              toggleCollapsible(){
                //source: https://getbootstrap.com/docs/5.0/components/collapse/
                let collapseElementList = [].slice.call(document.querySelectorAll('.collapse'))
                collapseElementList.map(function (collapseEl) {
                  return new bootstrap.Collapse(collapseEl)
                })
                
                this.collapsibleOpen = !this.collapsibleOpen;
  
                if(this.collapsibleOpen) this.$refs.checkOffers.style.top = '600px';
                else this.$refs.checkOffers.style.top = '50vh';
              }
  
  
      },
  
      mounted(){
        this.addEventListeners();
      },
  
      unmounted(){
        this.removeEventListeners();
      },
  
      created() {
        emitter.on("section-clicked", (section) => {
          this.$refs[section].scrollIntoView()
        });
      },
  
  }
  </script>
  
  <style scoped>
  
  #homepageBody{
    max-width: 100%;
    background-image: linear-gradient(0deg, rgb(31 36 30) 0%, rgb(38 42 42) 83%, rgb(40 41 45) 100%);
    /* background-color:#232626;  */
    /* background-color: #5d74697a; */
  }
  .homepageContent {
      height: 105vh;
      width: 100%;
      background-color: #262a2b;
      /* to hide strange border alike behavior - nije bas neki fiks - popraviti */
      /* top:3px;  */
  }
  
  #landingContent{
      display: flex;
      position:relative;
      top:-55px;
      left:0px;
      align-items: flex-start;
      overflow: visible;
      flex-direction: column;
      background-color:#262a2b;
  }
  
  
  
  #landingImage{
    position: absolute;
    /* top:-55px; */
    width:100%;
    z-index: 13;
    /* background-image: url('@/assets/HeroMobile.png'); */
    /* background-size: cover; */
    
  }
  
  
  picture img {
      /* object-fit: cover;  */
      width:100%;
      height: fit-content;
  }
  
  #abouthomepageContent{
    color:white; 
    font-weight: 300;
    background-color: #262a2b;
    display:flex;
    justify-content: flex-end;
    flex-direction: column;
    align-items: center;
    border-radius: 0 0 50% 50%;
    position: relative;
    top: 25vh;
    z-index:13;
  }
  
  #abouthomepageContent2{
    position:relative;
    top: -450px;
    z-index: 0;
    height: fit-content;
  }
  
  #abouthomepageContent2 > img {
      width:100%
  }
  
  
  #sliderDiv{
    height: auto;
    background-color: #222624fc;
    z-index:1;
  }
  
  
  #pancakeImg{
    width:100%;
    height: 100%;
    position: relative;
    z-index: 0;
  }
  
  
  
  
  /* subscribeDiv */
  #subscribeDiv{
    height: auto;
    width: 100%;
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
  }
  
  #subscribeImageDiv{
      /* background-image: url('@/assets/subscribe.jpg'); */
      height: fit-content;
      width: 100%;
      /* background-size:cover; */
  }
  
  #subscribeImageDiv > img {
    height: 75%;
    width: 100%;
  }
  
  #subscribeContent{
    position: absolute;
    width: 60%;
    font-family: 'Inter';
    font-style: normal;
    font-weight: 400;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }
  
  #subscribeContent > h1{
    font-size: 20vw;
    height: 22vw;
  }
  
  #subscribeContent > h5{
    font-size: 3vw;
    margin-bottom: 2vh;
  }
  
  
  #subscribeContent > small{
    font-family: 'Roboto';
    font-size: 2vw;
    line-height: 4vh;
    color: #353131;
    -webkit-backdrop-filter: blur(3px);
    backdrop-filter: blur(3px);
    margin-bottom: 4vh;
  }
  
  
  #subscribeButton{
    font-family: 'Roboto';
    font-style: normal;
    font-weight: 400;
    font-size: 3vw;
    text-align: center;
    letter-spacing: 0.095em;
    display: block;
    margin: 20px;
    background: none;
    padding: 10px 25px;
    border: 1px solid;
    -webkit-backdrop-filter: blur(1px);
    backdrop-filter: blur(1px);
  }
  
  
  
  
  
  #orderNowButton{
    position:relative;
    font-size: 2vw;
    text-align: center;
    height: fit-content;
    width: -webkit-fit-content;
    width: -moz-fit-content;
    width: fit-content;
    border-radius: 50%;
    margin: 2vw 2px 0vw 2vw;
   
  }
  
  .btn-circle.btn-xl {
    font-size: 16px;
    text-align: center;
    height: 40px;
    width: fit-content;
    border-radius: 50%;
    margin: 10px 2px 2px 2px;
  }
  
  
  #arrowIcon{
    height: 20px;
    width: 20px;
    color:white;
    margin: 0px 2px 0px 2px;
  }
  
  #mainText{
    position: relative;
    width: 40vw;
    font-family: 'Roboto';
    font-style: normal;
    font-weight: 400;
    font-size: 3vw;
    line-height: 3vw;
    color: #000000;
    -webkit-backdrop-filter: blur(2px);
    backdrop-filter: blur(2px);
  }
  
  
  #subText{
    margin-top: 5px;
    position: relative;
    width: 40vw;
    font-family: 'Inter';
    font-style: normal;
    font-weight: 400;
    font-size: 18px;
    line-height: 18px;
    color: #161515;
    -webkit-backdrop-filter: blur(2px);
  }
  
  #landingText{
    position:sticky;
    position: -webkit-sticky;
    top:35vh;
    left:5%;
    text-align: left;
    align-self: flex-start; 
    height: auto;
    z-index:13;
    
  }
  
  #italicText{
    color:white
    }
    
  
  .fa-long-arrow-right{
    padding-left:2px
  }
  
  .whiteText{
    color:white !important;
    font-weight: 100 !important;
  }
  
  .blackText{
    color:black !important;
  }
  
  
  /* Modal */
  
  .modal-footer{
      color:white; 
      width:100%;
  }
  
  .modal-footer > .btn{
      color:white; 
  }
  

  
  .modal-body > p{
      margin-bottom: 0;
  }
  
  .modal-backdrop {
     z-index: 1;
  } 
  
  
  
  /* toast */
  .toast-header{
      background-color: green;
      border-radius: 20px;
  }
  
  .toast-container {
      overflow: hidden;
      position: relative;
      z-index:4
  }
  .toast-container > .toast {
      width: fit-content;
      height:fit-content;
      text-align: right;
      animation: example1 1s linear;
      background: none;
      border-radius:20px;
      border:none; 
  }
  
  
  
  
  #descHeader{
    /* width: 85%: */
    align-items: center;
    position: relative;
    bottom: 20%;
    text-align: left;
    display: flex;
    font-size: 2rem;
    flex-direction: column;
    justify-content: center;
  }
  
  #descHeader > ul {
    margin-top: 1rem;
    line-height: 35px;
  }
  
  #checkOffersDiv{
    position:relative;
    top:50vh;
    z-index:2;
    color:white;
    text-align:center;
  }
  
  #checkOffersDiv > h5{
    font-weight:300;
    font-size: 4rem;
    backdrop-filter: blur(2px);
    width: fit-content;
    margin-left: auto;
    margin-right:auto;
  }
  
  
  .topOffers{
    z-index:2;
    color:white;
    text-align: left;
    margin: auto 2vw;
    display: flex;
    align-items: center;
    height: fit-content;
  }
  
  .topOffers > h5{
    font-weight:300;
    font-size: 2rem;
    backdrop-filter: blur(2px);
    width: fit-content;
    margin-left: auto;
    margin-right:auto;
    display:inline-block;
    margin: 10px 0;
  }
  
  #cardBody{
    width: 100%;
    position: relative;
    top:40px;
  }
  
  #contentHolder{
    width:100%; 
    height: 600px; 
    position:relative; 
    display: flex;
    flex-direction: row;
    justify-content: center;
  }

  
  
  .seeOffersIcon{
    height: 3rem;
    width: 3rem;
    margin-right: 5px;
  }
  
  
  
  /* subText animation */
  @import url("https://fonts.googleapis.com/css?family=Montserrat&display=swap");
  
  
  #subText >  h6 {
    margin-top:10px;
    font-family: "Montserrat Medium";
    max-width: fit-content;
    text-align: left;
    transform: scale(0.94);
    animation: scale 5s forwards cubic-bezier(0.5, 1, 0.89, 1);
    font-size: 2vw;
    backdrop-filter: blur(5px);

  }
  @keyframes scale {
    100% {
      transform: scale(1);
    }
  }
  
  h6 > span {
    display: inline-block;
    opacity: 0;
    filter: blur(4px);
    padding: 0px 2px;
  }
  
  span:nth-child(1) {
    animation: fade-in 3.8s 0.1s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  
  span:nth-child(2) {
    animation: fade-in 3.8s 0.2s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  
  span:nth-child(3) {
    animation: fade-in 3.8s 0.3s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  
  span:nth-child(4) {
    animation: fade-in 3.8s 0.4s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  
  span:nth-child(5) {
    animation: fade-in 3.8s 0.5s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  
  span:nth-child(6) {
    animation: fade-in 3.8s 0.6s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  
  span:nth-child(7) {
    animation: fade-in 3.8s 0.7s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  
  span:nth-child(8) {
    animation: fade-in 3.8s 0.8s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  
  span:nth-child(9) {
    animation: fade-in 3.8s 0.9s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  
  span:nth-child(10) {
    animation: fade-in 3.8s 1s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  
  span:nth-child(11) {
    animation: fade-in 3.8s 1.1s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  
  span:nth-child(12) {
    animation: fade-in 3.8s 1.2s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  
  span:nth-child(13) {
    animation: fade-in 3.8s 1.3s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  
  span:nth-child(14) {
    animation: fade-in 3.8s 1.4s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  
  span:nth-child(15) {
    animation: fade-in 3.8s 1.5s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  
  span:nth-child(16) {
    animation: fade-in 0.8s 1.6s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  
  span:nth-child(17) {
    animation: fade-in 0.8s 1.7s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  
  span:nth-child(18) {
    animation: fade-in 0.8s 1.8s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  
  
  
  /* About and subscribe section text animation source: https://codepen.io/grotandthemob/pen/bmXvzK*/
  .header {
    opacity: 0;
    transform: translateY(50px);
    transition: all 1.2s ease-out;
  }
  
  .headerVisible {
    opacity: 1;
    transform: translateY(0);
  }
  
  
  @keyframes fade-in {
    100% {
      opacity: 1;
      filter: blur(0);
    }
  }
  
  
  



  @media (min-width:767px){
   #landingText{
      left: 5vw;
      top: 25vw;
      z-index: 13;
   }
  }


  @media (min-width:1024px){
   #landingContent{
      height: 135vh;
   }
   #abouthomepageContent{
      top: 0;
   }
  }

  @media (min-width:1200px){
   #landingContent{
      height: 175vh;
   }
   #abouthomepageContent{
      top: 7vh;
   }
  }
  

  

  @media (min-width:1700px){
   #landingContent{
      height: 170vh;
   }
   #abouthomepageContent{
      top: 12vh;
   }
  }
  
  
  </style>
  #