<template>
  <div>
    <navBar></navBar>
  <div class="container">
<br>
    <div class="col-lg-3" v-for="room  in roomDetails">

      <div class="panel panel-primary">
        <div class="panel-heading">{{ room.full_name }}
          <span class="badge pull-right"> {{ room.room_number }} </span>
        </div>
        <div class="panel-body">
          <kbd>{{ room.people }} </kbd> People
          <kbd> {{ room.kids }}</kbd> Kids
          <br>
          <br>
          <kbd>{{ room.room_type }}</kbd>
          <button type="button" class="btn btn-danger pull-right" data-toggle="modal" data-target=".bd-example-modal-sm" @click="DELETE(room, room._id)">Delete</button>
        </div>
        <div class="panel-footer">
          <h4>
            <CountdownTimer :date="room.time"></CountdownTimer>
          </h4>
        </div>
      </div>
    </div>

    <div class="modal fade bd-example-modal-sm" id="myModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title" id="exampleModalLabel">Are you sure?</h1>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            Are you sure you want to delete this room? </div>
          <div class="modal-footer">

            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
            
              <button type="button" class="btn btn-danger" @click="deleteData(test_room, test_id)">Delete</button>
            
          </div>
        </div>
      </div>
    </div>
    <!-- /#sidebar-wrapper -->

    <!-- @click="deleteData(room, room._id)" -->
  </div>
  </div>
</template>

<script>
import CountdownTimer from './Timer.vue'
import Book from './book.vue'
import axios from 'axios'
import firebase from 'firebase'
import navBar from './navBar.vue'



export default {
  data() {
    return {
      photo: '',
      userId: '',
      name: '',
      email: '',
      user: {},
      roomDetails: [],
      test_room: {},
      test_id: ''
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

    axios.get('http://localhost:3000/rooms')
      .then((response) => {
        this.roomDetails = response.data;
        console.log(response);
      })
      .catch((error) => {
        consol.log(error);
      })
  },
  methods: {
    logOut() {
      firebase.auth().signOut();
    },
    DELETE(roomDetails1, id1) {
      $("#my-modal").modal('show');
      this.test_room = roomDetails1;
      this.test_id = id1;
    },
    deleteData(roomDetails, id) {
      axios.delete('http://localhost:3000/rooms/' + id)
        .then(response => this.roomDetails.splice(index, 1));
      window.location.reload();
    }
  },

  components: {
    CountdownTimer,
    Book,
    navBar
  },
};
</script>