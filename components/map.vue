<template>

  <div>

    <gmap-map :center="centers"
              :zoom="11"
              map-type-id="terrain"
              style="width: 100%; height: 340px">

      <gmap-marker @dragend="updateMaker"
                   :key="index"
                   v-for="(m, index) in markers"
                   :position="m.position"
                   :clickable="true"
                   :draggable="true"
                   @click="centers=m.position"></gmap-marker>

      <!-- @click="centers=m.position" -->

    </gmap-map>

  </div>

</template>

<script>
  import * as VueGoogleMaps from 'vue2-google-maps'
  export default {

    data() {

      return {

        centers: { lat: 1.3264129, lng: 103.8077371 },

        markers: [{

          position: { lat: 1.3483, lng: 103.6831 }

        }],

        place: null,

      }

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

    }

  }

</script>








