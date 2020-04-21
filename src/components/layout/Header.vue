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

                // Morning 6-8
                if (hours >= 6 && hours <= 8) {
                    title = 'Vejret til morgen'
                }
                // Noon 9-11
                else if (hours >= 9 && hours <= 11) {
                    title = 'Vejret til formiddag'
                }
                // Afternoon 12-17
                else if (hours >= 12 && hours <= 17) {
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