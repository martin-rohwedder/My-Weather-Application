<template>
    <header>
        <h1>{{ title }}</h1>
    </header>
</template>

<script>
    export default {
        name: 'HeaderComponent',
        data() {
            return {
                title: this.getTitle()
            }
        },
        methods: {
            getTitle() {
                let title = 'Vejret i dag'
                let hours = new Date().getHours()

                // Morning 6-9
                if (hours >= 6 && hours <= 9) {
                    title = 'Vejret til morgen'
                }
                // Noon 10-12
                else if (hours >= 10 && hours <= 12) {
                    title = 'Vejret til formiddag'
                }
                // Afternoon 13-17
                else if (hours >= 13 && hours <= 17) {
                    title = 'Vejret til eftermiddag'
                }
                // Evening 18-23
                else if (hours >= 18 && hours <= 23) {
                    title = 'Vejret i aften'
                }
                // Night 0-5
                else if (hours >= 0 && hours <= 5) {
                    title = 'Vejret i nat'
                }

                return title
            }
        }
    }
</script>

<style lang="stylus" scoped>
h1
    margin-top 60px
    margin-bottom 20px
    text-align center
    font-size 3.5em
</style>