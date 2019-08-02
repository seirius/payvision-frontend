<template>
    <v-app :style="{
            background: $vuetify.theme.themes[theme].background
        }">
        <v-app-bar app>
            <v-toolbar-title class="headline text-uppercase">
                <span>Payvision</span>
                <span class="font-weight-light">|Fullstack challenge</span>
            </v-toolbar-title>
        </v-app-bar>

        <v-content>
            <router-view></router-view>
        </v-content>
    </v-app>
</template>

<style lang="scss">
html {
    overflow: hidden !important;
}
</style>


<script>

export default {
    name: "App",
    data: () => ({
        //
    }),
    computed: {
        theme() {
            return this.$vuetify.theme.dark ? "dark" : "light";
        }
    }
};
</script>
