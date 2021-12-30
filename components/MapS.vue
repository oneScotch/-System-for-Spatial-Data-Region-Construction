<template>

  <div>
    <v-card class="mx-auto"
            max-width="500">
      <v-container>
        <b>Constructing Meaningful Regions for Data Exploration</b>
        <br>
        <v-row>
          <v-col cols="12" sm="12" lg="12">
            <gmap-map :center="centers"
                      :zoom="11"
                      map-type-id="terrain"
                      style="width: 100%; height: 340px">
              <gmap-marker v-if="marker"
                           @dragend="updateMaker"
                           :key="index"
                           v-for="(m, index) in markers"
                           :position="m.position"
                           :clickable="true"
                           :draggable="true"
                           @click="centers=m.position">
              </gmap-marker>
              <gmap-rectangle v-if="region"
                              :bounds="regionBounds"
                              :options="{editable: true}"
                              @bounds_changed="updateRectangle('bounds', $event)">

              </gmap-rectangle>

              <!-- @click="centers=m.position" -->

            </gmap-map>
          </v-col>
        </v-row>
        <v-row align="center"
               justify="space-around">
          <v-btn color="primary"
                 @click="region = !region">
            {{ region ? 'Show the region' : 'Hide the region' }}
          </v-btn>
          <v-btn color="primary"
                 @click="region_divide = !region_divide">
            {{ region_divide ? 'Show the divided region' : 'Hide the divided region' }}
          </v-btn>
          <v-btn color="primary"
                 @click="marker = !marker">
            {{ marker ? 'Show the original region' : 'Hide the original region' }}
          </v-btn>
        </v-row>
        <v-row align="center"
               justify="space-around">
          <v-construct-api />
        </v-row>
</v-container>
    </v-card>
  </div>

</template>

<script>
  import { ConstructAPI } from '~/components/construct-regions'
  import * as VueGoogleMaps from 'vue2-google-maps'
  export default {
    components: {
      'v-construct-api': ConstructAPI,
    },
    data() {
      return {
        centers: { lat: 1.3264129, lng: 103.8077371 },
        markers: [{
          position: { lat: 1.3483, lng: 103.6831 }
        }],
        rectangleBounds: {
          north: NaN,
          south: NaN,
          east: NaN,
          west: NaN
        },
        place: null,
        region: false,
        region_divide: false,
        marker: false,
      }
    },
    async fetch() {
      const regionDetails = this.$store.state.region
      this.rectangleBounds = {
        north: regionDetails.bottomLeftLat,
        south: regionDetails.topRightLat,
        east: regionDetails.bottomLeftLng,
        west: regionDetails.topRightLat
      }
    },
    computed: {
    },

    description: 'Autocomplete Example (#164)',

    mounted() {

    },

    methods: {

      setPlace(place) {

        this.place = place

      },

      setDescription(description) {

        this.description = description;

      },

      usePlace(place) {

        if (this.place) {

          var newPostion = {

            lat: this.place.geometry.location.lat(),

            lng: this.place.geometry.location.lng(),

          };

          this.center = newPostion;

          this.markers[0].position = newPostion;

          this.place = null;

        }

      },

      updateMaker: function (event) {

        console.log('updateMaker, ', event.latLng.lat());

        this.markers[0].position = {

          lat: event.latLng.lat(),

          lng: event.latLng.lng(),

        }
      },
      createMarker(latlng, html) {
        var contentString = html;
        var marker = new google.maps.Marker({
          position: latlng,
          map: map,
          zIndex: Math.round(latlng.lat() * -100000) << 5
        });

        google.maps.event.addListener(marker, 'click', function () {
          infowindow.setContent(contentString);
          infowindow.open(map, marker);
        });

        return marker;
      },
      updateRectangle(prop, value) {
        if (prop === 'bounds') {
          this.rectangleBounds = value;
        }
      },

    }

  }

</script>








