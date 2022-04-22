<template>
    <div id="app">
  <div class="container">
    <div class="row pen-title">
      <div class="col">
        <h3 class="title">{{title}}</h3>
      </div>
      <div class="col col-auto align-self-center text-right">
        <div class="input-group">
          <div class="input-group-prepend">
            <label class="input-group-text" for="inputGroupSelect01"># of People</label>
          </div>
          <input class="form-control" type="text" v-model='quantity'>
          <div class="input-group-append">
            <button class="btn btn-primary" v-on:click="getPeople">Get people</button>
          </div>
        </div>
      </div>
    </div>

    <transition-group name="fade" tag="div" class="row">
      <div v-for="(person, key) in people" :key="key" class="col-md-6 col-lg-4">
        <div class="person">
          <div class="person__header">
            <img v-bind:src="person.picture.large" v-bind:alt="person" class="rounded img-thumbnail">
            <div class="person__name">{{person.name.first}} {{person.name.last}}</div>
          </div>
          <div class="person__email">
            <a v-bind:href="'mailto:' + person.email">{{person.email}}</a>
          </div>
          <div class="person__address">
            <address>
              {{person.location.street.number}}
              {{person.location.city}}
              {{person.location.state}}
            </address>
          </div>
          <div class="person__map">
            <iframe width="100%" height="170" frameborder="0" scrolling="no" marginheight="0" marginwidth="0" v-bind:src="'https://maps.google.com/maps?q=' + person.location.coordinates.latitude +',' + person.location.coordinates.longitude + '&z=7&amp;output=embed'">
            </iframe>
          </div>
        </div>
      </div>
    </transition-group>

  </div>
</div>
</template>
<script >
import axios from "axios";
export default {
    name: 'home-app',
   data(){
       return{
           title:"RandomUser.me",
           people: [],
           quantity: 0
       }
   },
   methods: {
       //"https://randomuser.me/api/?results="
        
      getPeople() { 
           var endpoint = 'https://randomuser.me/api/?results='
        // ** axios requires a promise polyfill for ie11 **//
        axios.get(endpoint + this.quantity)
          .then((rsp)=>this.people = rsp.data.results)
      }
    },
    created: function () {
      this.getPeople();
    }
  

}
</script>