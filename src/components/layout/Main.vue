<template>
    <main>
        <ShowCurrentWeatherComponent v-bind:isItDay="isItDay" />
    </main>
</template>

<script>
    import ShowCurrentWeatherComponent from '../ShowCurrentWeather'

    export default {
        name: 'MainComponent',
        components: {
            ShowCurrentWeatherComponent
        },
        props: {
            isItDay: Boolean
        }
    }
</script>

<style scoped>

</style>