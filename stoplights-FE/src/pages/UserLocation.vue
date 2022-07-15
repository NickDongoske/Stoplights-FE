<template>
<div>
  <section class="ui two column left grid" style="position:relative;z-index:1;">
    <div id="main-div" class="column blue" style="width: 30%; margin-left: 0%;">
      <form class="hi" style="center">
        <div class="ui message red" v-show="error">{{error}}</div>
        <div class="ui segment">
          <div class="field">
            <div class="ui right icon input large ">
              <input 
              type="text" 
              placeholder="Enter your address" 
              v-model="address"
              id="autocomplete"
              />
              <i class="dot circle link icon" @click="locatorButtonPressed"></i>
            </div>  
          </div>
          <button class="ui button" @click="chooseAddressButtonPressed">Choose this address</button>
            <!-- on click, save data to backend and transition to next form, then save data again. -->
        </div>
        <div class="poop">What Direction? {{ whatDirection }}</div>
          <select v-model="whatDirection">
            <option disabled value="">Please select one</option>
            <option>North</option>
            <option>South</option>
            <option>East</option>
            <option>West</option>
          </select>
        <div>
         <div class="poop">What Time? {{ whatTime }}</div>
          <select v-model="whatTime">
            <option disabled value="">Please select one</option>
            <option>12:00 am</option>
            <option>12:15 am</option>
            <option>12:30 am</option>
            <option>12:45 am</option>
            <option>1:00 am</option>
            <option>1:15 am</option>
            <option>1:30 am</option>
            <option>1:45 am</option>
            <option>2:00 am</option>
            <option>2:15 am</option>
            <option>2:30 am</option>
            <option>2:45 am</option>
            <option>3:00 am</option>
            <option>3:15 am</option>
            <option>3:30 am</option>
            <option>3:45 am</option>
            <option>4:00 am</option>
            <option>4:15 am</option>
            <option>4:30 am</option>
            <option>4:45 am</option>
            <option>5:00 am</option>
            <option>5:15 am</option>
            <option>5:30 am</option>
            <option>5:45 am</option>
            <option>6:00 am</option>
            <option>6:15 am</option>
            <option>6:30 am</option>
            <option>6:45 am</option>
            <option>7:00 am</option>
            <option>7:15 am</option>
            <option>7:30 am</option>
            <option>7:45 am</option>
            <option>8:00 am</option>
            <option>8:15 am</option>
            <option>8:30 am</option>
            <option>8:45 am</option>
            <option>9:00 am</option>
            <option>9:15 am</option>
            <option>9:30 am</option>
            <option>9:45 am</option>
            <option>10:00 am</option>
            <option>10:15 am</option>
            <option>10:30 am</option>
            <option>10:45 am</option>
            <option>11:00 am</option>
            <option>11:15 am</option>
            <option>11:30 am</option>
            <option>11:45 am</option>
            <option>12:00 pm</option>
            <option>12:15 pm</option>
            <option>12:30 pm</option>
            <option>12:45 pm</option>
            <option>1:00 pm</option>
            <option>1:15 pm</option>
            <option>1:30 pm</option>
            <option>1:45 pm</option>
            <option>2:00 pm</option>
            <option>2:15 pm</option>
            <option>2:30 pm</option>
            <option>2:45 pm</option>
            <option>3:00 pm</option>
            <option>3:15 pm</option>
            <option>3:30 pm</option>
            <option>3:45 pm</option>
            <option>4:00 pm</option>
            <option>4:15 pm</option>
            <option>4:30 pm</option>
            <option>4:45 pm</option>
            <option>5:00 pm</option>
            <option>5:15 pm</option>
            <option>5:30 pm</option>
            <option>5:45 pm</option>
            <option>6:00 pm</option>
            <option>6:15 pm</option>
            <option>6:30 pm</option>
            <option>6:45 pm</option>
            <option>7:00 pm</option>
            <option>7:15 pm</option>
            <option>7:30 pm</option>
            <option>7:45 pm</option>
            <option>8:00 pm</option>
            <option>8:15 pm</option>
            <option>8:30 pm</option>
            <option>8:45 pm</option>
            <option>9:00 pm</option>
            <option>9:15 pm</option>
            <option>9:30 pm</option>
            <option>9:45 pm</option>
            <option>10:00 pm</option>
            <option>10:15 pm</option>
            <option>10:30 pm</option>
            <option>10:45 pm</option>
            <option>11:00 pm</option>
            <option>11:15 pm</option>
            <option>11:30 pm</option>
            <option>11:45 pm</option>
            
          </select>
        </div>
        <input type="submit" value="Submit" class="btn btn-block" />
        </form>
    </div>
    
    
  </section>
  <section id="map"></section>
  </div>
</template>
<script>
import axios from 'axios'
export default {

  data() {
    return {
      address: "",
      error: "",
      spinner: false,
    }
  },

  mounted() {
  let autocomplete =  new google.maps.places.Autocomplete(
      document.getElementById("autocomplete")
    )
    autocomplete.addListener("place_changed", () => {
    let place =  autocomplete.getPlace();
    this.showUserLocationOnTheMap(place.geometry.location.lat(), place.geometry.location.lng())
    })
    },

  methods: {
    locatorButtonPressed() {

      this.spinner = true;
      if(navigator.geolocation) {
         navigator.geolocation.getCurrentPosition(
           position => {
             this.getAddressFrom(
               position.coords.latitude, 
               position.coords.longitude
               );
             this.showUserLocationOnTheMap(position.coords.latitude, 
                                           position.coords.longitude)
         },
         error => {
           this.error = "locater unable to find your address"
           this.spinner = false;
         }
         );
      } else {
        this.error = error.message
        this.spinner = false;
        
      }


    },
    getAddressFrom(lat, long) {
      axios.get("https://maps.googleapis.com/maps/api/geocode/json?latlng="
      + lat + 
      ","
      + long
      + "&key=AIzaSyD2pMQXi-9tZwWDVv0oTGkiRC0R2Se94oE")
      .then(response => {
        if(response.data.error_message) {
          this.error = response.data.error_message
          console.log(response.data.error_message)
        } else {
          this.address = response.data.results[0].formatted_address
        }
        this.spinner = false;
      })
      .catch(error => {
        this.error = error.message;
        this.spinner = false;
        console.log(error.message);
        
      });

    },
    showUserLocationOnTheMap(latitude, longitude) {
      let map = new google.maps.Map(document.getElementById("map"), {
        zoom:15,
        center: new google.maps.LatLng(latitude, longitude),
        mapTypeId: google.maps.MapTypeId.ROADMAP
      });
      new google.maps.Marker({
        position: new google.maps.LatLng(latitude, longitude),
        map:map
      })
    },
    chooseAddressButtonPressed() {
      
    }
  }
}
</script>

<style>
.ui.button,
.dot.circle.icon {
  background-color: black;
  color: lavenderblush ;
}
.poop {
  color: black; 
} 
.pee {
  color: black;
}
.caca {
   color: black;
}
.hi {
  display: grid;
  justify-content: center;
}

.pac-icon {
  display: none;
}
.pac-item {
  padding: 10px;
  font-size: 16px;
  cursor: pointer;
}
.pac-item:hover {
  background-color: #ececec;
}
.pac-item-query {
  font-size: 16px;
}
#map {
  position: absolute;
  top:0;
  right: 0;
  left: 0;
  bottom:0;
  background: rgb(10, 10, 10);
}
</style>



