<template>
    <div>
        <v-toolbar app color='transparent' flat absolute>
            <v-toolbar-side-icon v-if='isMobile' @click.native='drawer = true'></v-toolbar-side-icon>
        </v-toolbar>


        <v-navigation-drawer :permanent='!isMobile' v-model='drawer' floating app :class='{"elevation-5": !isMobile}' class="pb-0">
            <v-list>
                <v-subheader>Tools</v-subheader>
                <v-list-tile to='/theme' ripple>
                    <v-list-tile-avatar>
                        <v-icon>palette</v-icon>
                    </v-list-tile-avatar>
                    <v-list-tile-title>Theme generator</v-list-tile-title>
                </v-list-tile>
                <v-list-tile to='/layout' ripple>
                    <v-list-tile-avatar>
                        <v-icon>view_quilt</v-icon>
                    </v-list-tile-avatar>
                    <v-list-tile-title>Layout generator</v-list-tile-title>
                </v-list-tile>
                <v-divider></v-divider>
                <v-subheader>Settings</v-subheader>
                <v-list-tile ripple @click='confirmCacheClearing = true; drawer = isMobile ? false : drawer'>
                    <v-list-tile-avatar>
                        <v-icon>delete</v-icon>
                    </v-list-tile-avatar>
                    <v-list-tile-title>Clear cache</v-list-tile-title>
                </v-list-tile>
                <v-list-tile ripple @click='$store.dispatch("toggleDark"); drawer = isMobile ? false : drawer'>
                    <v-list-tile-avatar>
                        <v-icon>{{darkModeIcon}}</v-icon>
                    </v-list-tile-avatar>
                    <v-list-tile-title>Dark mode: {{!$store.state.dark ? 'Off' : 'On'}}</v-list-tile-title>
                </v-list-tile>
                <v-divider></v-divider>
                <v-list-tile ripple href='https://github.com/hkh12/vuetify-sandbox'>
                    <v-list-tile-title>GitHub</v-list-tile-title>
                </v-list-tile>
            </v-list>
            <v-footer class="pl-3 pr-1 caption" fixed>
                Copyright &nbsp;
                <a href="https://hkh12.github.io" target="_blank" class="secondary--text">@hkh12</a>
                &nbsp;&mdash; {{(new Date()).getFullYear()}}
            </v-footer>
        </v-navigation-drawer>


        <v-content class="pr-0 pt-0">
            <v-jumbotron :color='"grey " + (dark ? "darken-4" : "lighten-3")' height='256px'>
                <v-container fill-height>
                    <v-layout align-center>
                        <v-flex class="pt-4">
                            <h3 :class="`display-${isMobile ? 2 : 3}`">Vuetify Sandbox</h3>
                            <div class="subheading grey--text">A tool for creating and testing Vuetify themes and layouts with ease.</div>
                        </v-flex>
                    </v-layout>
                </v-container>
            </v-jumbotron>

            <v-tabs :fixed-tabs='isMobile' :color="`grey ${dark ? 'darken-4' : 'lighten-4'}`" :slider-color="`accent${!dark ? ' darken-2': ''}`">
                <v-tab :active-class="tabActiveClass">Introduction</v-tab>
                <v-tab :active-class="tabActiveClass">theme tool</v-tab>
                <v-tab :active-class="tabActiveClass">layout tool</v-tab>
                <v-tab-item class="pa-3">
                    <p class="body-2">
                        Vuetify sandbox is a <b>PWA</b> (progressive web app) that helps you to create and test themes and layouts based on <a href="https://material.io/">Material design guidelines</a> and <a href="https://vuetifyjs.com/en">Vuetify</a> with no pain.
                    </p>
                    <v-container fluid class="pa-0" grid-list-sm>
                        <v-layout row wrap>
                            <feature-card icon='brightness_3' title-text="Dark mode">
                                Are you a night coder? hit <code>Ctrl + alt + d</code> to let app have night mood.
                            </feature-card>
                            <feature-card icon='save' title-text="Cache">
                                Every change you make to themes and layouts, gets saved automatically in local storage and will be available for the next time that you come.
                            </feature-card>
                            <feature-card icon='widgets' title-text="PWA">
                                Vuetify sandbox is a PWA, so it can work offline or appear on your homescreen.
                                it has a clean, simple and easy-to-work layout built with Vuetify.
                            </feature-card>
                        </v-layout>
                    </v-container>
                </v-tab-item>
                <v-tab-item class="pa-3">
                    <div v-once>
                        <p class="subheading mb-1">
                            <b>
                                Theme tool is a powerful tool with a minimalist design to help you generate themes, view and export them.
                            </b>
                        </p>
                        <v-divider class="my-2 mx-1"></v-divider>
                        <div class="headline my-1">
                            Sidebar
                        </div>
                        <p class="body-1">
                            Colors used in theme are listed in the right sidebar. If you want to change them, just click on them. Color picker will be opened <br>
                            And even if you want to view the code, click the <b>EXPORT</b> button.
                        </p>
                        <div class="headline my-1">
                            Color picker
                        </div>
                        <p class="body-1">
                            Once opened, a wide variety of <a href="https://material.io/design/color/the-color-system.html" target="_blank">Material design colors</a> is showed to you. click on a color group (colored squares). then, select a shade of that color and done!
                        </p>
                        <div class="headline my-1">
                            Preview Section
                        </div>
                        <p class="body-1">
                            The preview is available on the left side. Every change you make to the colors, will be reflected into it. <br>
                            <i>There's no action belonged to this section; it just provides a visual example for you.</i>
                        </p>
                    </div>
                </v-tab-item>
                <v-tab-item class="pa-3">
                    <p class="subheading mb-1">
                        <b>
                            Layout tool provides an easy-to-work-with workspace to play out with Vuetify layout feauters.
                        </b>
                        <br>
                        There are settings for each key component. Combine them to make something great!
                    </p>
                </v-tab-item>
            </v-tabs>
        </v-content>


        <v-speed-dial bottom right fixed direction="top" :open-on-hover="!isTouch">
            <v-btn fab color="secondary" slot='activator' class='mt-2'>
                <v-icon>launch</v-icon>
            </v-btn>
            <v-btn fab dark small color="indigo" to='/layout'>
                <v-icon>view_quilt</v-icon>
            </v-btn>
            <v-btn fab dark small color="deep-purple" to='/theme'>
                <v-icon>palette</v-icon>
            </v-btn>
        </v-speed-dial>

            
        <v-dialog v-model="confirmCacheClearing" max-width="480px" transition="slide-y-reverse-transition">
            <v-card>
                <v-card-title class="headline pb-0">Clear Cache</v-card-title>
                <v-card-text>
                    Are you sure you want to clear app cache?
                    <br>
                    this will delete your works on layouts and themes and <b>cannot be undone</b>.
                </v-card-text>
                <v-card-actions class="pt-0 justify-end">
                    <v-btn color="grey" flat @click='confirmCacheClearing = false'>cancel</v-btn>
                    <v-btn color="secondary" flat @click='clearCache'>clear</v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog> 
    </div>
</template>
<script>
import FeatureCard from '../components/FeatureCard.vue'

export default {
    components: {FeatureCard},
    name: 'home',
    data() {
        return {
            confirmCacheClearing: false,
            drawer: false,
            wikiDialogOpen: false,
            wikiLoaded: false,
            wikiText: '',
        }
    },
    computed: {
        isMobile(){
            return this.$vuetify.breakpoint.smAndDown
        },
        isTouch: () => 'ontouchstart' in window,
        dark(){
            return this.$store.state.dark
        },
        darkModeIcon(){
            return !this.$store.state.dark ? 'brightness_3' : 'wb_sunny'
        },
        tabActiveClass(){
            let suffix = this.dark ? '' : ' text--darken-2'
            return `tabs__item--active accent--text${suffix}`
        } 
    },
    watch: {
        wikiDialogOpen(e){
            if (e && !this.wikiLoaded) setTimeout(() => {this.wikiLoaded = true}, 3000)
        }
    },
    methods: {
        clearCache(){
            localStorage.clear();
            this.$root.$forceUpdate()
            this.confirmCacheClearing = false
        }
    }
}
</script>
<style lang="stylus">
.jumbotron__wrapper
    overflow visible !important
</style>

