<template>
   <div class="wrapper">
            <div class="left">
                <div class="container">
                    <div class="container__logo">
                        <img src="../assets/img/logo.png" alt="myTask">
                    </div>
                    <div class="container__form">
                        <div class="container__form--input"> 
                            <!-- <input type="text" 
                            ref="autocomplete"
                            id="input"
                            class="search-location" 
                            placeholder="Enter Location"
                            onfocus="value = ''"
                            v-model="query"
                            @keypress="getWeather">  -->
                            <VueGoogleAutocomplete 
                            types="" 
                            id="input"
                            class="search-location" 
                            placeholder="Enter Location"
                            onfocus="value = ''"
                            v-on:placechanged="getLoc"
                            v-on:error="handleError"
                            v-model="query"
                            @keypress="getWeather">
                            </VueGoogleAutocomplete>
                        </div>
                    </div>
                    <div class="container__weather" v-if="typeof weather.main != 'undefined'">
                        <div class="container__weather-box1">
                             <div class="container__weather-location"><h4>{{ weather.name }}, {{ weather.sys.country }}</h4></div>
                             <div class="container__weather-date"><h5>{{ moment().format('MMMM Do YYYY, h:mm a') }}</h5></div>
                        </div>
                        <div class="container__weather-box2">
                            <div class="container__weather-temprature">
                                <div class="container__logo-2">
                                    <img :src= "'http://openweathermap.org/img/w/'+ weather.weather[0].icon +'.png'" alt="myTask">
                                </div>
                                <h2>{{ Math.round(weather.main.temp) }}&#8451;</h2>
                                </div>
                            <div class="container__weather-weather"><h3>{{ weather.weather[0].main }}</h3></div>
                        </div>
                    </div>
                    <!-- <div class="container__weather" v-show v-else-if="typeof weather.main == 'undefined'">
                        <h1>Invalid Location</h1>
                    </div> -->
                    <div class="container__weather" v-else>
                        <h1 style="color: #3F3D56;">... provide a valid location</h1>
                    </div>
                </div>
            </div>
            <div class="right">
                <div class="showcase">
                    <div class="showcase__content">
                        <img src="../assets/img/task1.svg" alt="task">
                    </div>
                </div>
            </div>
        </div>
</template>

<script>
import VueGoogleAutocomplete from 'vue-google-autocomplete'
export default {
  name: 'Weather',
  components: { VueGoogleAutocomplete },
  data (){
    return {
        query: "",
        key: '5d9fc3940502c964d13f250cf2b7a3a7',
        api_url: "https://api.openweathermap.org/data/2.5/",
        weather: {},
        url: "'http://openweathermap.org/img/w/' + weather.weather[0].icon +'.png'",
        
    }
  },
  methods: {
        getWeather (e) {
           
            if (e.key == "Enter") {
                // fetch(`${this.api_url}weather?q=${this.query}&units=metric&APPID=${this.key}`)
                // .then(res => {
                //     return res.json();
                // }).then(this.getData)
                console.log(this.query)
                
            }
        },
        getData (data) {
        this.weather = data;
        },
        getLoc(address, placeResultData, id) {
            this.query = placeResultData.address_components[0]["short_name"]
            console.log(address)
            console.log(id)

            fetch(`${this.api_url}weather?q=${this.query}&units=metric&APPID=${this.key}`)
            .then(res => {
                return res.json()
            }).then(data => {
                this.weather = data
            })
        },
        handleError(error){
            console.log(error)
        }
    },
  mounted() {
      
        this.getLoc()

        // fetch(`${this.api_url}weather?q=${this.query}&units=metric&APPID=${this.key}`)
        // .then(res => {
        //     return res.json()
        // }).then(data => {
        //     this.weather = data
        // })
        // console.log(`The user picked ${city} with the coordinates ${lat}, ${lon}`);
      
    
    },
    created() {
        // const placeApi = document.createElement('script');
        // placeApi.setAttribute('src', "https://maps.googleapis.com/maps/api/js?key=AIzaSyBp7Epn-II7apphaqz_0bUKPQmZfApuaDo&libraries=places");
        // document.head.appendChild(placeApi);
    } 

  
}
</script>
<style lang="scss">

@import url('https://fonts.googleapis.com/css?family=Ubuntu:300i,400,400i,500,700&display=swap');

$rich-black: #02111b;
$arsenic: #3f4045;
$charston-green: #30292f;
$paynes-gray: #5d737e;
$primary-light: #fcfcfc;


$bp-largest: 75em;     // 1200px
$bp-large: 68.75em;    // 1100px
$bp-medium: 56.25em;   // 900px
$bp-small: 37.5em;     // 600px
$bp-smallest: 31.25em; // 500px


* {
    margin: 0;
    padding: 0;
}

*,
*::before,
*::after {
    box-sizing: inherit;
}

html {
    box-sizing: border-box;
    font-size: 62.5%; //1rem = 10px => 10px/16px * 100 = 62.5%


    @media only screen and (max-width: $bp-large){
        font-size: 50%;
    }
}

body {
    font-family: 'Ubuntu', sans-serif;
    font-weight: 400;
    line-height: 1.6;
    background-color: $primary-light;
    background-size: cover;
    background-repeat: no-repeat;
    min-height: 100vh;
    color: $rich-black;
    overflow: hidden;
}

[v-cloak]  {display:none}


// components
.btn {
    &,
    &:link,
    &:visited {
        text-transform: uppercase;
        text-decoration: none;
        padding: 1.5rem 4rem;
        display: inline-block;
        border-radius: 10rem;
        transition: all .2s;
        position: relative;
        font-size: 1.6rem;

        //style for button inside form
        border: none;
        cursor: pointer;
        margin-bottom: 2.5rem;
    }

    &:hover {
        transform: translateY(-3px);
        box-shadow: 0 1rem 1rem rgba(#000, .3);

        &::after {
            transform: scaleX(1.4) scaleY(1.6);
            opacity: 0;
        }
    }

    &:active, 
    &:focus {
        outline: none;
        transform: translateY(-1px);
        box-shadow: 0 .5rem 1rem rgba(#000, .2);
    }

    &--primary {
        background-color: #6c63ff;
        color: $primary-light;

        &::after {
           background-color: #6c63ff;
        }
    }

    &--secondary {
        background-color: #3F3D56;
        color: $primary-light;

        &::after {
           background-color: #3F3D56;
        }
    }

    &::after {
        content: "";
        display: inline-block;
        height: 100%;
        width: 100%;
        border-radius: 10rem;
        position: absolute;
        top: 0;
        left: 0;
        z-index: -1;
        transition: all .4s;
        font-size: 1.6rem;
    }


    &--animated {
        animation: moveInBottom .5s ease-out .75s;
        animation-fill-mode: backwards;
    }
}

// layouts

$bp-largest: 75em;     // 1200px
$bp-medium: 56.25em;   // 900px
$bp-small: 37.5em;     // 600px

.wrapper {
    display: flex;
    flex-direction: row;
    background-image: linear-gradient(to right bottom, 
    rgba(#B3B557, .55), rgba(#81823E, .65)), 
    url(../assets/img/background.jpg);
    background-size: cover;
}

.left {
    // background-image: linear-gradient(to right bottom, 
    // rgba(#B3B557, .55), rgba(#81823E, .65)), 
    // url(./assets/img/background.jpg);
    // background-size: cover;
    display: flex;
    flex-direction: column;
    flex: 1;
    align-items: center;
    height: 100vh;
    // justify-content: center;

    @media only screen and (min-width: $bp-largest){
        flex: 6;
        
    }

    @media only screen and (max-width: $bp-small){
        height: 500vh;
    }
}

.right {
    flex: 1;
    // background: linear-gradient(to right bottom, 
    // rgba(#81823E, .90), rgba(#B3B557, .95));

    @media only screen and (min-width: $bp-largest){
        flex: 4;
    }

    @media only screen and (max-width: $bp-medium){
        display: none;
    }
    
}

.container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 80%;
    padding-bottom: 1rem; 
    padding-top: 5rem;

    @media only screen and (max-width: $bp-small){
        width: 100%;
    }

    &__logo {
        img {
            width: 10rem;
        }

        &-2 {
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;

            img {
                width: 7rem;
            }
        }
    }

    &__form {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        // width: 20rem;

        &--input {
            width: 200%;
            margin: 2rem 0 2rem;
            text-align: center;

            @media only screen and (max-width: $bp-small){
                width: 140%;
            }

            #input {  
                width: 70%;
                font-size: 2.5rem;
                font-family: inherit;
                color: inherit;
                padding: 1rem 2rem;
                border-radius: 2px;
                background-color: rgba($primary-light, .8);
                border: none;
                border-bottom: 3px solid transparent;
                margin: 0 auto;
                display: block; 
                transition: all .3s;

                &:focus {
                    outline: none;
                    box-shadow: 0 1rem 2rem rgba(#3F3D56, .5);
                    border-bottom: 3px solid #3F3D56; 
                }

                // &:focus:invalid {
                //     border-bottom: 3px solid $color-secondary;
                // }

                // &::-webkit-input-placeholder {
                //     color: $color-gray-light;
                // }
            }

        }
    }
    

    &__weather {
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-top: 3rem;

        &-box1 {
            display: flex;
            flex-direction: column;
            align-items: center; 
        }

        &-location {
            h4 {
                font-size: 3rem;
                color: #fff;
                text-shadow: rgba(#000, .55);
                letter-spacing: 3px;
            }
        }

        &-date {

            h5 {
                font-size: 2rem;
                color: #fff;
                font-weight: 300i;
                text-shadow: rgba(#000, .5);
            }
        }

        &-box2 {
            display: flex;
            flex-direction: column;
            align-items: center; 
            margin-top: 3.5rem;
            
        }

        &-temprature {
            padding: 2rem 2.5rem;
            border-radius: .6rem;
            background: linear-gradient(to right bottom, 
            rgba(#81823E, .90), rgba(#B3B557, .95));
            box-shadow: 0 1rem 2rem rgba(#3F3D56, .8);

            h2 {
                font-size: 7rem;
                color: #fff;
                font-weight: 700;
                text-shadow: rgba(#000, .5); 
            }
        }

        &-weather {
            h3 {
                margin-top: 2.5rem;
                font-size: 3.8rem;
                color: #fff;
                font-weight: 500;
                text-shadow: rgba(#000, .9);
                letter-spacing: 4.5px;
            }
        }
    }
  
   
}

.visual {
    opacity: 1 !important;
}

.showcase {
    display: flex;
    flex-direction: colum;
    justify-content: center;
    align-items: center;
    padding-top: 11rem;

    &__content {
        width: 80%;
        height: auto;

        img {
            max-width: 100%;
        }
    }
}
</style>

