<template>
  <v-app :dark="boolTheme">
    <v-navigation-drawer v-model="sidebar" app>
      <v-list>
        <v-list-tile
          v-for="item in menuItems"
          :key="item.title"
          :to="item.path">
          <v-list-tile-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>{{ item.title }}</v-list-tile-content>
        </v-list-tile>
        <v-list-tile v-if="isAuthenticated" @click="userSignOut">
          <v-list-tile-action>
            <v-icon> exit_to_app </v-icon>
          </v-list-tile-action>
          <v-list-tile-content> Выйти </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>

    <v-toolbar app>
      <span class="hidden-sm-and-up">
        <v-toolbar-side-icon @click="sidebar = !sidebar">
        </v-toolbar-side-icon>
      </span>
      <v-toolbar-title>
        <v-layout row align-center justify-space-between>
          <v-flex xs12>
            <router-link to="/" tag="span" style="cursor: pointer">
              {{ appTitle }}
            </router-link>
            <v-btn icon ripple @click="themeFnChange">
              <v-icon color="grey darken-1"> invert_colors </v-icon>
            </v-btn>
          </v-flex>
        </v-layout>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items class="hidden-xs-only">
        <v-btn
          flat
          v-for="item in menuItems"
          :key="item.title"
          :to="item.path">
          <v-icon left dark>{{ item.icon }}</v-icon>
          {{ item.title }}
        </v-btn>
        <v-btn flat v-if="isAuthenticated" @click="userSignOut">
          <v-icon left> exit_to_app </v-icon>
            Exit
        </v-btn>
      </v-toolbar-items>
    </v-toolbar>

    <v-content>
      <router-view></router-view>
    </v-content>

  </v-app>
</template>

<script>
  export default {
    data () {
      return {
        appTitle: 'Expire',
        sidebar: false,
        boolTheme: false
      }
    },
    mounted () {
      if (localStorage.boolTheme) {
        if (localStorage.boolTheme === 'false') {
          this.boolTheme = false
        } else {
          this.boolTheme = true
        }
      }
    },
    methods: {
      userSignOut () {
        this.$store.dispatch('userSignOut')
      },
      themeFnChange () {
        localStorage.boolTheme = !this.boolTheme
        this.boolTheme = !this.boolTheme
      }
    },
    computed: {
      isAuthenticated () {
        return this.$store.getters.isAuthenticated
      },
      menuItems () {
        if (this.isAuthenticated) {
          return [
            {title: 'Home', path: '/home', icon: 'home'}
          ]
        } else {
          return [
            {title: 'Sign Up', path: '/signup', icon: 'face'},
            {title: 'Sign In', path: '/signin', icon: 'lock_open'}
          ]
        }
      }
    }
  }
</script>
