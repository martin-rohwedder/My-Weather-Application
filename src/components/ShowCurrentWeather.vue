<template>
    <div>
        <section class="section-center" v-if="errored">
            <p>Der skete desværre en fejl, da jeg prøvede at hente vejrdata. Prøv venligst igen.</p>
        </section>

        <section class="section-center" v-else>
            <div v-if="loading">
                <img v-show="isItDay()" v-bind:src="iconUrlSpinnerDay" alt="">
                <img v-show="!isItDay()" v-bind:src="iconUrlSpinnerNight" alt="">
            </div>

            <div v-else>
                <div v-for="(value, index) in this.currentWeather" v-bind:key="index">
                    <div id="weather-content">
                        <div v-for="(weather, propertyName, index) in value.weather" v-bind:key="index">
                            <!-- Clear -->
                            <div class="weather-icon-container" v-if="weather.id === 800">
                                <img v-show="isItDay()" v-bind:src="iconUrlClearDay" class="weather-icon-image" alt="">
                                <img v-show="!isItDay()" v-bind:src="iconUrlClearNight" class="weather-icon-image" alt="">
                            </div>
                            <!-- Few Clouds -->
                            <div class="weather-icon-container" v-else-if="weather.id >= 801 && weather.id <= 802">
                                <img v-show="isItDay()" v-bind:src="iconUrlCloudDay" class="weather-icon-image" alt="">
                                <img v-show="!isItDay()" v-bind:src="iconUrlCloudNight" class="weather-icon-image" alt="">
                            </div>
                            <!-- Many clouds -->
                            <div class="weather-icon-container" v-else-if="weather.id >= 803 && weather.id <= 804">
                                <img v-bind:src="iconUrlCloud" class="weather-icon-image" alt="">
                            </div>
                            <!-- Light Rain -->
                            <div class="weather-icon-container" v-else-if="weather.id === 500">
                                <img v-show="isItDay()" v-bind:src="iconUrlRainAndSun" class="weather-icon-image" alt="">
                                <img v-show="!isItDay()" v-bind:src="iconUrlRain" class="weather-icon-image" alt="">
                            </div>
                            <!-- Rain -->
                            <div class="weather-icon-container" v-else-if="weather.id > 500 && weather.id <= 531">
                                <img v-bind:src="iconUrlRain" class="weather-icon-image" alt="">
                            </div>
                            <!-- Snow -->
                            <div class="weather-icon-container" v-else-if="weather.main === 'snow'">
                                <img v-bind:src="iconUrlSnow" class="weather-icon-image" alt="">
                            </div>
                            <!-- Thunderstorm -->
                            <div class="weather-icon-container" v-else-if="weahter.main === 'Thunderstorm'">
                                <img v-bind:src="iconUrlThunder" class="weather-icon-image" alt="">
                            </div>
                        </div>

                        <div class="weather-details-container">
                            <div v-for="(main, propertyName, index) in value.main" v-bind:key="index">
                                <p class="main-temperature-text" v-if="propertyName === 'temp'">{{ Math.round(main) }}&deg;</p>
                                <div>
                                    <div class="min-max-temp-container">
                                        <p v-show="propertyName === 'temp'">{{ getMaxTemp(value.main) }}&deg; / {{ getMinTemp(value.main) }}&deg;</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="weather-details-lower-container" v-for="(sys, propertyName, index) in value.sys" v-bind:key="index">
                        <p v-show="propertyName === 'country'" class="city-name-text">{{ value.name }}, {{ getCountryCode(value.sys) }}</p>
                    </div>
                </div>

                <SearchCityComponent v-on:searchCityName="doSearch" />
            </div>
        </section>
    </div>
</template>

<script>
import axios from 'axios'
import SearchCityComponent from './SearchCityComponent'

export default {
    name: 'ShowCurrentWeatherComponent',
    components: {
        SearchCityComponent
    },
    data() {
        return {
            currentWeather: {},
            loading: true,
            errored: false,
            cityName: '',

            // List of weather Condition Codes: https://openweathermap.org/weather-conditions
            iconUrlClearDay: require('../assets/weather-icons/day.svg'),
            iconUrlClearNight: require('../assets/weather-icons/night.svg'),
            iconUrlCloudDay: require('../assets/weather-icons/cloudy-day-2.svg'),
            iconUrlCloudNight: require('../assets/weather-icons/cloudy-night-2.svg'),
            iconUrlCloud: require('../assets/weather-icons/cloudy.svg'),
            iconUrlRainAndSun: require('../assets/weather-icons/rainy-3.svg'),
            iconUrlRain: require('../assets/weather-icons/rainy-5.svg'),
            iconUrlSnow: require('../assets/weather-icons/snowy-6.svg'),
            iconUrlThunder: require('../assets/weather-icons/thunder.svg'),

            // Spinner icons
            iconUrlSpinnerDay: require('../assets/spinner-icons/spinner-day.svg'),
            iconUrlSpinnerNight: require('../assets/spinner-icons/spinner-night.svg')
        }
    },
    mounted() {
        this.getCurrentWeather('København, dk', 'da', 'metric')
    },
    methods: {
        // get the current weather by cityname and countrycode. Define the language and units (Celsius, Fahrenheit) the data will be returned as
        getCurrentWeather(cityName, language, units) {
            axios
                .get('http://api.openweathermap.org/data/2.5/weather?q=' + cityName + '&appid=becea41c15a8e7e9c71432a09c2b2432&lang=' + language + '&units=' + units)
                .then(response => (this.currentWeather = response))
                .catch(error => {
                    console.log(error)
                    this.errored = true
                })
                .finally(() => {
                    // Time out for test purposes
                    setTimeout(() => {
                        this.loading = false
                    }, 300)
                    // this.loading = false
                })
        },
        // Determine if it is day or night
        isItDay() {
            let hours = new Date().getHours()
            let isItDay = true

            // If hours is between 6 and 20 exclusive it is day
            if (hours > 5 && hours < 20) {
                isItDay = true
            } else {
                isItDay = false
            }

            return isItDay
        },
        // Get rounded Max temperature
        getMaxTemp(main) {
            let tempMax = main.temp_max
            return Math.round(tempMax)
        },
        // Get rounded Min temperature
        getMinTemp(main) {
            let tempMin = main.temp_min
            return Math.round(tempMin)
        },
        // Get the country code
        getCountryCode(sys) {
            let countryCode = sys.country
            return countryCode
        },
        // Do search
        doSearch(value) {
            this.cityName = value
            this.loading = true

            this.getCurrentWeather(this.cityName, 'da', 'metric')
        }
    }
}
</script>

<style lang="stylus" scoped>
.section-center
    display flex
    flex-direction column
    width 100vw
    align-items center

#weather-content
    display flex
    flex-direction row
    justify-content space-evenly
    align-items center
    width 50vw

.weather-icon-container
    flex 1
    width 350px
    min-width 100px

.weather-details-container
    width 250px
    min-width 100px

.weather-icon-image
    width: 450px

.main-temperature-text
    font-size 4em

.min-max-temp-container > p
    width 100px
    text-align center
    font-size 1.25em

.weather-details-lower-container
    display flex
    flex-direction column
    align-items center
    width 50vw

.city-name-text
    font-size 3em

</style>