<template>
  <nav>
    <!-- <p>text makes the toolbar transparent, app makes the toolbar fixed on the top</p> -->
    <v-app-bar flat app clipped-left color="white" elevation="1">
      <!-- <p>app-bar-nav-icon is the burger button</p> -->
      <v-app-bar-nav-icon class="grey--text" @click="drawer = !drawer" />
      <v-toolbar-title>
        <span style="color: #0052c1">{{ greeting() }}</span>
      </v-toolbar-title>

      <!-- <p>things before spacer are aligned to left and things after spacer are aligned to right</p> -->
      <v-space />
      <p class="grey--text mt-4 mr-2 hidden-sm-and-down">
        {{ getDate }}
      </p>
      <digital-clock :blink="true" class="grey--text mr-2 hidden-sm-and-down" />

      <v-menu offset-y>
        <v-list>
          <v-list-item v-for="item in Menu"
                       :key="item.text"
                       router
                       :to="item.route">
            <v-list-item-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title>
                {{ item.text }}
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-menu>
    </v-app-bar>

    <v-navigation-drawer v-model="drawer" app clipped :hide-overlay="mini" :width="270">
      <v-list>
        <div v-for="link in navLinks" :key="link.text">
          <v-list-item v-if="!link.subLinks"
                       :key="link.text"
                        :to="link.route"
                     :color="link.color">
          <v-list-item-icon>
            <v-icon>{{ link.icon }}</v-icon>
          </v-list-item-icon>
          <v-list-item-title>
            {{ link.text }}
          </v-list-item-title>
          </v-list-item>
        </div>
      </v-list>
    </v-navigation-drawer>
  </nav>
</template>

<script>
import DigitalClock from 'vue-digital-clock'
import * as moment from 'moment-timezone'

export default {
  components: {
    DigitalClock,
  },
  data() {
    return {

      drawer: true,
      navLinks: [],
      Menu: [],
      messages: 2,
      notifications: 3,
    }
  },

  fetch() {
    this.setMenu()
    this.setNavLinks()
  },

  computed: {
    mini() {
      return this.$vuetify.breakpoint.mdAndDown
    },
    getDate() {
      const nowDate = moment().format('ddd Do MMM')
      return nowDate
    },
  },

  methods: {
    greeting() {
      if (new Date().getHours() < 12) {
        return 'Good Morning  '
      } else if (new Date().getHours() < 18) {
        return 'Good Afternoon  '
      }
      return 'Good Evening  '
    },
    setNavLinks() {
      this.navLinks = [
        {
          icon: 'mdi-view-dashboard',
          text: 'Master plan',
          route: '/',
        },
        {
          icon: 'mdi-devices',
          text: 'k-Means',
          route: '/connect+',
        },
        {
          icon: 'mdi-calendar',
          text: 'DBSCAN',
          route: '/DBSCAN',
        },
      ]
    },
    setMenu() {
      this.Menu = [
        {
          icon: 'mdi-account-outline',
          text: 'Profile',
          route: '/profile',
        },
        {
          icon: 'mdi-cog-outline',
          text: 'Settings',
          route: '/settings',
        },
      ]
    },
  },
}
</script>
