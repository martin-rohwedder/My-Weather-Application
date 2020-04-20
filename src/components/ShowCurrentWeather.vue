<template>
    <div>
        <section class="section-center" v-if="errored">
            <p>Sorry, but something happened when we tried to get the data. Please try again.</p>
        </section>

        <section class="section-center" v-else>
            <div v-if="loading">
                The data is loading ...
            </div>

            <div v-else>
                <div id="weather-content" v-for="(value, index) in this.currentWeather" v-bind:key="index">
                    <div v-for="(weather, propertyName, index) in value.weather" v-bind:key="index">
                        <!-- Clear -->
                        <div class="weather-icon-container" v-if="weather.id === 800">
                            <img v-bind:src="iconUrlClearDay" class="weather-icon-image" alt="">
                        </div>
                        <!-- Clouds -->
                        <div class="weather-icon-container" v-else-if="weather.main === 'Clouds'">
                            <img v-bind:src="iconUrlCloud" class="weather-icon-image" alt="">
                        </div>
                        <!-- Rain -->
                        <div class="weather-icon-container" v-else-if="weather.main === 'Rain'">
                            <img v-bind:src="iconUrlRain" class="weather-icon-image" alt="">
                        </div>
                        <!-- Snow -->
                        <div class="weather-icon-container" v-else-if="weather.main === 'Snow'">
                            <img v-bind:src="iconUrlSnow" class="weather-icon-image" alt="">
                        </div>
                        <!-- Thunderstorm -->
                        <div class="weather-icon-container" v-else-if="weahter.main === 'Thunderstorm'">
                            <img v-bind:src="iconUrlThunder" class="weather-icon-image" alt="">
                        </div>
                    </div>

                    <div class="weather-details-container">
                        <div v-for="(main, propertyName, index) in value.main" v-bind:key="index">
                            <p v-if="propertyName === 'temp'">{{ main }}</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'ShowCurrentWeatherComponent',
    data() {
        return {
            currentWeather: {},
            loading: true,
            errored: false,
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
        }
    },
    mounted() {
        this.getCurrentWeather('Slagelse', 'dk', 'da', 'metric')
    },
    methods: {
        // get the current weather by cityname and countrycode. Define the language and units (Celsius, Fahrenheit) the data will be returned as
        getCurrentWeather(cityName, country, language, units) {
            axios
                .get('http://api.openweathermap.org/data/2.5/weather?q=' + cityName + ',' + country + '&appid=becea41c15a8e7e9c71432a09c2b2432&lang=' + language + '&units=' + units)
                .then(response => (this.currentWeather = response))
                .catch(error => {
                    console.log(error)
                    this.errored = true
                })
                .finally(() => {
                    this.loading = false
                })
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
    min-width 400px

.weather-details-container
    min-width 250px
    background yellowgreen

.weather-icon-image
    width: 500px

</style>