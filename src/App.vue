<template>
  <v-app>
    <v-navigation-drawer temporary v-model="sideNav">
      <v-list>
        <v-list-tile
          v-for="item in menuItems"
          :key="item.title"
          :to="item.link">
          <v-list-tile-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>{{ item.title }}</v-list-tile-content>
        </v-list-tile>
        <v-list-tile
          v-if="userIsAuthenticated"
          @click="onLogout">
          <v-list-tile-action>
            <v-icon>exit_to_app</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>Logout</v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-toolbar dark class="primary">
      <v-toolbar-side-icon
        @click.stop="sideNav = !sideNav"
        class="hidden-sm-and-up "></v-toolbar-side-icon>
      <v-toolbar-title>
        <router-link to="/" tag="span" style="cursor: pointer">DLeague Events</router-link>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items class="hidden-xs-only">
        <v-btn
          flat
          v-for="item in menuItems"
          :key="item.title"
          :to="item.link">
          <v-icon left dark>{{ item.icon }}</v-icon>
          {{ item.title }}

        </v-btn>
        <v-btn
          v-if="userIsAuthenticated"
          flat
          @click="onLogout">
          <v-icon left dark>exit_to_app</v-icon>
          ออกจากระบบ

        </v-btn>
      </v-toolbar-items>
    </v-toolbar>
    <main>
      <router-view></router-view>
    </main>
  </v-app>
</template>

<script>
  export default {
    data () {
      return {
        sideNav: false
      }
    },
    computed: {
      menuItems () {
        let menuItems = [
          {icon: 'face', title: 'สมัครสมาชิก', link: '/signup'},
          {icon: 'lock_open', title: 'เข้าสู่ระบบ', link: '/signin'}
        ]
        if (this.userIsAuthenticated && this.userIsAdmin) {
          menuItems = [
            {icon: 'supervisor_account', title: 'ดูกิจกรรมทั้งหมด', link: '/meetups'},
            {icon: 'room', title: 'สร้างกิจกรรมใหม่', link: '/meetup/new'},
            {icon: 'person', title: 'ข้อมูลส่วนตัว', link: '/profile'}
          ]
        } else if (this.userIsAuthenticated && !this.userIsAdmin) {
          menuItems = [
            {icon: 'supervisor_account', title: 'ดูกิจกรรมทั้งหมด', link: '/meetups'},
            {icon: 'person', title: 'ข้อมูลส่วนตัว', link: '/profile'}
          ]
        }
        return menuItems
      },
      userIsAuthenticated () {
        return this.$store.getters.user !== null && this.$store.getters.user !== undefined
      },
      userIsAdmin () {
        let user = this.$store.getters.user
        if (user.userType === 'admin') {
          return true
        } else {
          return false
        }
      }
    },
    methods: {
      onLogout () {
        this.$store.dispatch('logout')
      }
    }
  }
</script>

<style lang="stylus">
  @import './stylus/main'
</style>
