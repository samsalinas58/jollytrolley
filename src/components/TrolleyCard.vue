<template>
    <md-card>
      <div class="card-flair"><img class="beacon" alt="Beacon" src="../assets/beacon.svg"></div>
      <div class="trolley-info-nugget">There are currently <div class="num-trolleys callout-text"> {{count}} </div> trolleys in service.</div>
      <div class="trolley-info-nugget">The next trolley will arrive in &nbsp; <div id="min" class="next-trolley callout-text"> {{minutes}} </div> &nbsp;  minutes.</div>
    </md-card>
</template>
<script id="google" src="https://maps.googleapis.com/maps/api/js?key=AIzaSyAryDLbPlrqQZ8leDk6EMBgZ851bbV-c30"></script>
<script>
import axios from "axios"
import Vue from "vue"

var TrolleyCard = {
  name: 'TrolleyCard',
  data () {
    return {
      count: 0,
      trolleys: {},
      minutes: null,
    }
  },
  methods: {
    updateMinutes: function(min) {
      // let data = {
      //   ...TrolleyCard.data(),
      //   minutes: min
      // }
      // this.minutes = min
      // this.$nextTick(function () {
      //   console.log(this.$el.textContent)
      // })

      if(min < TrolleyCard.data().minutes){
        console.log(TrolleyCard.data().minutes)
        console.log(min)
      }
    },
  },
  mounted () {
    initMap()
  },
  created () {
    axios
      .get('https://track.mata.org:7170/allCars')
      .then(response => {
        const trolleyIds = Object.keys(response.data)
        for(const id of trolleyIds) {
          this.id = id.length
          if(id.length == 3){
            this.count++;
            this.trolleys[id] = response.data[id];
          }
        }

        const keys = Object.keys(this.trolleys)
        for(const key of keys) {
          // Trolley Coordinates
          var start = new google.maps.LatLng({lat: this.trolleys[key][0], lng: this.trolleys[key][1]});
          // All of the trolley stops hardcoded
          const stops = 
            [{lat: 32.805563, lng: -96.794098},
            //{lat: -96.79652, lng: 32.807625},
            {lat: 32.808372, lng: -96.798183},
            //{lat: -96.799503, lng: 32.807318},
            {lat: 32.806827, lng: -96.799558},
            //{lat: -96.796641, lng: 32.809314},
            {lat: 32.808573, lng: -96.795981},
            //{lat: -96.795907, lng: 32.807081},
            {lat: 32.805214, lng: -96.800322},
            //{lat: -96.801347, lng: 32.803658},
            {lat: 32.802114, lng: -96.802347},
            //{lat: -96.801253, lng: 32.798572},
            {lat: 32.798294, lng: -96.801143},
            //{lat: -96.801438, lng: 32.797759},
            {lat: 32.797526, lng: -96.801336},
            //{lat: -96.801775, lng: 32.796592},
            {lat: 32.79608, lng: -96.801732},
            //{lat: -96.80215700000001, lng: 32.79531},
            {lat: 32.794988, lng: -96.80208},
            //{lat: -96.802496, lng: 32.793783},
            {lat: 32.79265, lng: -96.802902},
            //{lat: -96.803331, lng: 32.791818},
            {lat: 32.790949, lng: -96.803616},
            //{lat: -96.803342, lng: 32.788714},
            {lat: 32.787816, lng: -96.802329},
            //{lat: -96.800439, lng: 32.786228},
            {lat: 32.784585, lng: -96.79854},
            //{lat: -96.797209, lng: 32.784873},
            {lat: 32.785722, lng: -96.796267},
            //{lat: -96.797253, lng: 32.786688},
            {lat: 32.787539, lng: -96.798288},
            //{lat: -96.799342, lng: 32.788152},
            {lat: 32.788978, lng: -96.800558},
            //{lat: -96.80138, lng: 32.789661},
            {lat: 32.790867, lng: -96.802679},
            //{lat: -96.802848, lng: 32.793216},
            {lat: 32.794203, lng: -96.802517},
            //{lat: -96.801013, lng: 32.799602},
            {lat: 32.799533, lng: -96.800831},
            //{lat: -96.800884, lng: 32.801867},
            {lat: 32.802856, lng: -96.800232},
            //{lat: -96.799703, lng: 32.803633},
            {lat: 32.805589, lng: -96.798486},
            {lat: 32.80668, lng: -96.797764}];

          //TODO: Let the user choose the stop instead of using the last
          var end = new google.maps.LatLng(stops.pop());

          const waypoints = makeWaypoints(stops);

          var request = {
            origin: start,
            destination: end,
            waypoints: waypoints,
            travelMode: 'DRIVING',
            drivingOptions: {
              departureTime: new Date(),
              trafficModel: 'bestguess'
            }
          };
          directionsService.route(request, function(result, status) {
            if (status == 'OK') {
              let seconds = 0;
              result.routes[0].legs.forEach(function(step){
                seconds += step.duration.value;
              })
              const minutes = Math.ceil(seconds / 60);
              this.minutes = minutes;

              //TODO: Make this work
              //TrolleyCard.methods.updateMinutes(minutes);
              if(document.getElementById("min").innerHTML == "  " || document.getElementById("min").innerHTML > minutes)
                document.getElementById("min").innerHTML = minutes;
            }
          });
        }
      });
  },
}

export default TrolleyCard

// Turn an array of latlngs into a waypoints object
function makeWaypoints(stops) {
  let waypoints = [];
  stops.forEach(function(stop){
     waypoints.push({location: stop, stopover: true});
  });
  return waypoints;
}


var directionsService, directionsRenderer, map; 
function initMap() {
  directionsService = new google.maps.DirectionsService();
  directionsRenderer = new google.maps.DirectionsRenderer();
  var dallas = new google.maps.LatLng(-96.794098, 32.805563);
  var mapOptions = {
    zoom:7,
    center: dallas
  }
  map = new google.maps.Map(document.getElementById('map'), mapOptions);
  directionsRenderer.setMap(map);
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.md-card {
  width: 90%;
  min-height: 240px;
  margin: 0 5% 0 5%;
  padding: 32px 8px 32px 8px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  background: #fff;
  position: relative;
  border-radius: 8px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  z-index: 4;
}
.trolley-info-nugget {
  display: flex;
  align-items: center;
  color: #3F3F3F;
  font-size: 36px;
  font-family: Roboto;
  line-height: 40px;
  font-weight: 600;
}
.num-trolleys {
  color: #19A7E0;
  border-bottom: 4px solid #19A7E0;
}
.next-trolley {
  color: #EF596F;
  border-bottom: 4px solid #EF596F;
}
.callout-text {
  padding: 0 4px 1px 4px;
  margin: 0 8px 0 8px;
}
.card-flair {
  position: absolute;
  left: 0;
  z-index: 1;
  transform: matrix(0.90, 0, 0, 0.90, 0, -160);
  margin: 16px 0 0 0;
}
</style>

