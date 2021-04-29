<template>
  <div class="position-relative bg-dark">
    <b-sidebar id="sidebar-navigation"
      :visible="isVisibleSidebar"
      shadow
      bg-variant="dark"
      text-variant="light"
      aria-labelledby="sidebar-no-header-title"
      no-header
      no-close-on-route-change
      class="position-relative h-100"
    >
      <div class="pt-3">
        <h4 id="sidebar-no-header-title" class="text-center pb-2 m-0">
          {{ dashboardName }}
        </h4>
        <hr>
        <nav class="mb-3">
          <b-nav vertical pills>
            <router-link
              v-for="item in menuItems"
              :key="item.to"
              v-slot="{ href, navigate, isActive }"
              :to="item.to"
              custom
            >
              <b-nav-item
                :active="isActive"
                :href="href"
                @click="navigate"
              >
                {{ item.linkText }}
              </b-nav-item>
            </router-link>
          </b-nav>
        </nav>
      </div>
      <template #footer>
        <div class="bg-dark text-black-50 px-3 py-2 version text-left">
          <small>{{currentVersion}}</small>
        </div>
      </template>
    </b-sidebar>
  </div>
</template>

<script>
export default {
  name: 'Admin',
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
     * Sidebar megjelenítése
     */
    isVisibleSidebar: {
      type: Boolean,
      required: true,
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
    menuItems: {
      type: Array,
      required: true,
    },
  },
}
</script>

<style scoped lang="scss" src="./AdminMenu.scss" />
