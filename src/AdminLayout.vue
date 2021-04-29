<template>
  <div class="container-fluid">
    <div class="row">
      <div class="d-flex w-100">
        <admin-menu
          v-if="isLoggedIn"
          :is-visible-sidebar="isVisibleSidebar"
          :menu-items="menuItems"
          :dashboard-name="dashboardName"
          :current-version="currentVersion"
        />
        <div v-if="isLoggedIn || currentPage === 'login'" class="w-100">
          <div v-if="isLoggedIn" class="content-header bg-dark mb-3">
            <div class="d-flex">
              <div class="col-2">
                <b-button v-b-toggle.sidebar-navigation class="toggleBtn text-light" variant="link">
                  <font-awesome-icon icon="bars" />
                </b-button>
              </div>
              <div class="col-10 text-right text-light">
                <small>Üdv, {{ name }}!</small>
                <b-button
                  class="text-light"
                  variant="link"
                  @click="logout"
                >
                  <font-awesome-icon icon="sign-out-alt" />
                </b-button>
              </div>
            </div>
          </div>
          <div class="col-12 content">
            <router-view />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import AdminMenu from './AdminMenu/AdminMenu.vue'
import { library } from '@fortawesome/fontawesome-svg-core'
import { faBars, faSignOutAlt } from '@fortawesome/free-solid-svg-icons'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

library.add(faBars, faSignOutAlt)

export default {
  name: 'AdminLayout',
  components: {
    AdminMenu,
    FontAwesomeIcon,
  },
  props: {
    /**
     * Dasboard neve
     */
    dashboardName: {
      type: String,
      default: ''
    },
    /**
     * Aktuális verzió
     */
    currentVersion: {
      type: String,
      default: '',
    },
    /**
     * Menüpontok
     *
     * [{
     *  to: '/profile', // required
     *  linkText: 'menu', // required
     *  permission: 'PERMISSION_USER_ADMIN',
     * }]
     */
    menu: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      /**
       * sidebar megjelenítése
       */
      isVisibleSidebar: true,
    }
  },
  computed: {
    /**
     * Filterezzük azokat a menüpontokat amikhez nincs jog
     */
    menuItems() {
      return this.menu.filter(item => !item.permission || this.$store.getters['Auth/$can'](item.permission))
    },
    /**
     * Bejelentkezett felhasználó neve
     *
     * @return {*}
     */
    name() {
      return this.$store.getters['Auth/getUser'].name
    },
    /**
     * Be van e jelentkezve
     *
     * @return {any}
     */
    isLoggedIn() {
      return this.$store.getters['Auth/isLoggedIn']
    },
    /**
     * Aktuális oldal neve
     *
     * @return {string}
     */
    currentPage() {
      return this.$route.name
    },
  },
  watch: {
    /**
     * Becsukja a sidebart a szélességnek megfelelően
     */
    $mq: {
      immediate: true,
      handler() {
        this.visibleSidebar = this.$mq === 'xl' || this.$mq === 'lg'
      },
    },
  },
  methods: {
    /**
     * Kijelentkezés
     */
    logout() {
      this.$store.dispatch('Auth/logout')
        .then(() => {
          this.$root.$emit('showMessageSuccess', 'Sikeres kijelentkezés!')
          this.$router.replace('login')
        })
    },
  },
}
</script>

<style scoped lang="scss" src="./AdminLayout.scss" />
