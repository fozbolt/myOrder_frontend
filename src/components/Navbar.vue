<template>
 <nav v-if="$route.path!== '/login' && $route.path!== '/register' && store.userType !== 'manager'" class="navbar navbar-expand-lg navbar-light" :style="{backgroundColor:getColor()}">
      <div class="container-fluid">
        <button class="navbar-toggler collapsed" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavDropdown"
          aria-controls="navbarNavDropdown" aria-expanded="false" aria-label="Toggle navigation">
          <span class="icon-bar top-bar"></span>
          <span class="icon-bar middle-bar"></span>
          <span class="icon-bar bottom-bar"></span>
        </button>
        <div v-if="store.userType !== 'manager'" class="collapse navbar-collapse" id="navbarNavDropdown" @focusin="toggleMenu" :style="{backgroundColor:getColor()}">
          <ul class="navbar-nav">
            <li class="nav-item active" data-bs-toggle="collapse" data-bs-target=".navbar-collapse.show">
              <a @click="this.$router.push('/food_list')"  href="" class="nav-link nav-link-ltr">Order now</a>
            </li>
            <li class="nav-item" data-bs-toggle="collapse" data-bs-target=".navbar-collapse.show">
              <a v-if="store.userType=== 'customer'" @click="this.$router.push('/checkout')" href=""  class="nav-link nav-link-ltr">My order</a>
              <a v-if="store.userType=== 'waiter'" @click="this.$router.push('/checkout')" href=""  class="nav-link nav-link-ltr">Checkout</a>
            </li>
             <li class="nav-item" data-bs-toggle="collapse" data-bs-target=".navbar-collapse.show">
              <a  @click="goTo('Top offers')" class="nav-link nav-link-ltr" href="#">Top offers</a>
            </li>
             <li class="nav-item" data-bs-toggle="collapse" data-bs-target=".navbar-collapse.show">
              <a  @click="goTo('New offers')" class="nav-link nav-link-ltr" >New offers</a>
            </li>
            <li v-if="store.userType === 'customer'" class="nav-item" data-bs-toggle="collapse" data-bs-target=".navbar-collapse.show">
              <a @click="scrollTo('aboutSection')" class="nav-link  nav-link-ltr">About us</a>
            </li>
             <li v-if="store.userType === 'customer'" class="nav-item" data-bs-toggle="collapse" data-bs-target=".navbar-collapse.show">
              <a @click="scrollTo('subscribeSection')" class="nav-link nav-link-ltr" href="#">Subscribe</a>
            </li>
            <span v-if="auth.authenticated">
              <a @click="logout" class="btn btn-info my-2 my-sm-0 mr-2 p-1" id="logoutBtn">Logout</a>
            </span>
          </ul>
        </div>
        <a class="navbar-brand ms-auto" @click="$router.push({ path: `/` });">
          <img id="logoNav" alt="navbar logoNav" src="@/assets/logo-main.svg" />
        </a>
      </div>
  </nav>
</template>

<script>
import {Auth} from "@/services/index.js";
import store from '@/store.js';
import {emitter} from '@/main.js'


export default {
  name: 'Navbar',

  data(){
      return{
        store,
        auth: Auth.state,
      }
  },

  methods: {
      logout() {
          Auth.logout();
          //refresh
          this.$router.go();
      },

      getColor(){
        //a little bit hard coded for now
        if (this.$route.path === '/'  && this.store.userType === 'waiter') return '#e9ecef'
        else if (this.$route.path === '/' && this.store.userType !== 'manager')  return ''
        else if (this.store.userType === 'manager') return '#e9ecef'
        else return 'white'

        
      
      },

      goTo(subcategory){
        this.store.selectedSubCategory  = subcategory
        this.store.category = 'MainCourse'
        this.store.type = 'Food'
        this.$router.push('/food_list')
      },

      scrollTo(section){
        //temporary solution because it can't scroll to section if we arent at needed view, for now
        if(this.$route.path !== '/') this.$router.push('/')


        emitter.emit("section-clicked", section);
      }

  },
      
}

</script>


<style lang="scss">

nav {
  position: relative;
  top:0;
  left:0;
  background: rgba(255, 255, 255, 0.6);
  z-index: 14;
  

  a {
    font-weight: bold;
    color: black;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
.navbar-nav{
  padding-left: 50px;
  align-items: center;
  max-width: 200px;
  backdrop-filter: blur(2px);
}

.navbar{
  padding-left: 0px 0px 0px 30px;
  --bs-navbar-padding-x: none;
}

.container-fluid{
  --bs-gutter-x: 0rem;
}

.logoNav{
  position: absolute;
  top: 0;
  right: 0;
}

.navbar-collapse {
    position:absolute;
    //hardcoded to begin after navbar height - not best solution
    left:0;
    width: 80%;
}

.navbar-nav a {
  color:black;
}

.navbar-toggler{
   outline:none; 
   box-shadow: none !important;
   border:none !important;
}

#logoutBtn{
  margin-left: 5px;
  border-radius: 10px;
  background: azure;
  padding: 0px !important;
}


@media (max-width:991px){
 .navbar-collapse {
    position:absolute;
    top:0;
    left:0;
    max-width: 200px;
    background: transparent;
    top:40px;
    z-index: 13;
    border-radius: 0 0 10px 0;
}

.navbar-nav{
  padding-left: 0px;
  background: rgba(255, 255, 255, 0.6);
}


#logoNav{
  margin-right: -5px;
}

}


@media (min-width:992px){
//Desktop navbar on hover animation 

  .nav-link {
    text-decoration: none;
    color: #B8A929;
    margin: 0px 1px;
    display: inline-block;
    position: relative;
  }

  .nav-link:hover {
    opacity: 1;
  }

  .nav-link::before {
    transition: 300ms;
    height: 2.5px;
    content: "";
    position: absolute;
    background-color: #B8A929;
  }

  .nav-link-ltr::before {
    width: 0%;
    bottom: 10px;
  }

  .nav-link-ltr:hover::before {
    width: 85%;
  }

}

//animated navbar toggle / hamburger menu
.navbar-toggler span {
  display: block;
  background-color: black;
  height: 2px;
  width: 25px;
  margin-top: 6px;
  margin-bottom: 5px;
  position: relative;
  left: 0;
  opacity: 1;
  transition: all 0.35s ease-out;
  transform-origin: center left;
}


.navbar-toggler span:nth-child(1) {
  transform: translate(0%, 0%) rotate(0deg);
}

.navbar-toggler span:nth-child(2) {
  opacity: 1;
}

.navbar-toggler span:nth-child(3) {
  transform: translate(0%, 0%) rotate(0deg);
}

.navbar-toggler span:nth-child(1) {
  margin-top: 0.3em;
}

.navbar-toggler:not(.collapsed) span:nth-child(1) {
  transform: translate(15%, -33%) rotate(45deg);
}

.navbar-toggler:not(.collapsed) span:nth-child(2) {
  opacity: 0;
}

.navbar-toggler:not(.collapsed) span:nth-child(3) {
  transform: translate(15%, 33%) rotate(-45deg);
}



//remove border from hamburger menu
.btn:focus {
  outline: none;
  box-shadow: none;
  }



@media (min-width:992px){
  .navbar-nav a {
    font-size: 1.25em;
  }
.navbar>.container-fluid{
  height: 50px;
  }
  
  #logoNav{
    height:32px;
    width:180px;
  }

  .navbar-expand-lg .navbar-nav .nav-link {
    width: max-content;
    font-size: 22px;
}
}


@media (min-width:1600px){
  .navbar-nav a {
    font-size: 1.5em;
  }
.navbar>.container-fluid{
  height: 50px;
  }
  
  #logoNav{
    height:50px;
    width:200px;
  }

  .navbar-expand-lg .navbar-nav .nav-link {
    padding-right: 10px;
    padding-left: 10px;
}
}


//loading indicator source: https://codepen.io/sethdavis512/pen/vJxNdq


</style>