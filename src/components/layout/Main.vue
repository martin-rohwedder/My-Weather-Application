<template>
    <main>
        <ShowCurrentWeatherComponent />
    </main>
</template>

<script>
    import ShowCurrentWeatherComponent from '../ShowCurrentWeather'

    export default {
        name: 'MainComponent',
        components: {
            ShowCurrentWeatherComponent
        }
    }
</script>

<style scoped>

</style>