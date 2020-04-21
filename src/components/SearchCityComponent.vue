<template>
    <div id="search-city-container">
        <div class="outer-shadow">
            <form class="form-day-container" v-on:submit.prevent="searchCity">
                <input v-model.trim="cityName" class="search-input-box" type="search" placeholder="F.eks. København eller København, DK" />
                <input class="search-input-btn" type="submit" value="Find By">
            </form>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'SearchCityComponent',
        data() {
            return {
                cityName: ''
            }
        },
        methods: {
            searchCity() {
                // Emit the city name to the parent (ShowCurrentWeatherComponent)
                this.$emit('searchCityName', this.cityName)

                // Reset variable so form field is reset on search
                this.cityName = ''
            }
        }
    }
</script>

<style lang="stylus" scoped>
#search-city-container
    display flex
    justify-content center
    margin-top 40px
    margin-bottom 20px

#search-city-container > div > form
    display flex
    justify-content space-around
    padding 15px
    width 600px

.outer-shadow
    -webkit-box-shadow 1px 1px 8px 0px rgba(44, 62, 80, 0.50)
    -moz-box-shadow: 1px 1px 8px 0px rgba(44, 62, 80, 0.50)
    box-shadow: 1px 1px 8px 0px rgba(44, 62, 80, 0.50)

.form-day-container
    background-color #c3d5e5
    border 1px solid #a3b5c5
    -moz-box-shadow inset 0 0 3px #fff
    -webkit-box-shadow inset 0 0 3px #fff
    box-shadow inset 0 0 3px #fff

.search-input-box
    width 400px
    height 40px
    padding 5px
    font-size 18px
    background-color #f3f9ff
    border 1px solid #71879a

.search-input-box:focus
    outline-color #71879a

.search-input-btn
    width 150px
    height 40px
    background-color #a3b5c5
    border 1px solid #8297a9
    color #e9f2fb
    font-size 0.9em
    font-weight bold
    cursor pointer
    -webkit-box-shadow 0px 3px 8px 1px rgba(44, 62, 80, 0.50)
    -moz-box-shadow: 0px 3px 8px 1px rgba(44, 62, 80, 0.50)
    box-shadow: 0px 3px 8px 1px rgba(44, 62, 80, 0.50)

.search-input-btn:hover
    background-color #8297a9
    border 1px solid #5a6d7c

.search-input-btn:focus
    outline none
</style>