<template>
    <!-- <hr>
      <img :src="photo" style='height: 120px'> <br>
      <p>{{name}}</p>
      <p>{{email}}</p>
      <p>{{userId}}</p>
      <hr>
      <pre>{{user}}</pre> -->
      <nav class="navbar navbar-default">
    <div class="container">
      <div class="navbar-header">
        <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar" aria-expanded="false" aria-controls="navbar">
          <span class="sr-only">Toggle navigation</span>
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
        </button>
        <router-link to="/"><a class="navbar-brand">Hotel System</a></router-link>
      </div>
      <div id="navbar" class="collapse navbar-collapse">
        <ul class="nav navbar-nav">
          <li class="ok"><router-link to="/">Home</router-link></li>
          <li><router-link to="/about">About</router-link></li>
        </ul>
        
        <ul class="nav navbar-nav navbar-right">
           
            <li role="presentation" ><a href="#"> <Book></Book></a></li>
            <li class="ok"> <span class="badge badge-primary" style="margin-top:2px;"><h4>{{name}}</h4> </span></li>
          <li class="ok"><img :src="photo" style='height: 50px'></li>
          <li role="presentation"> <a  href="#" @click='logOut'>Log out</a>
</li>
        </ul>
        
      </div><!--/.nav-collapse -->
    </div>
  </nav>

</template>

<script>
import firebase from 'firebase'
import Book from './book.vue'



export default {
  data() {
    return {
      photo: '',
      userId: '',
      name: '',
      email: '',
      user: {},


    }
  },
  created() {
    var vm = this
    firebase.auth().onAuthStateChanged(function(user) {
      if (user) {
        vm.user = user;
        vm.name = vm.user.displayName;
        vm.email = vm.user.email;
        vm.photo = vm.user.photoURL;
        vm.userId = vm.user.uid;
      }
    });


  },
    methods: {
          logOut() {
      firebase.auth().signOut();
    }

  },
  components:{
      Book
  }
};
</script>

<style>

</style>
