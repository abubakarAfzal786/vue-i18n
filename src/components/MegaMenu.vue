<template>
  <div class="relative" @mouseover="showMenu" @mouseleave="hideMenu">
    <a
      href="/#features"
      class="text-copy-primary hover:text-gray-600"
      @focus="showMenu"
      @keydown.shift.tab="hideMenu"
      @keydown.esc.exact="hideMenu"
      @keydown.up.exact.prevent="startArrowKeys"
      @keydown.down.exact.prevent="startArrowKeys"
    >
      {{ this.menuItem["name_" + currentLocale] }}
    </a>

    <div class="absolute w-full">&nbsp;</div>
    <transition name="mega-menu-fade">
      <div
        v-show="isVisible"
        class="
          mega-menu
          absolute
          normal-case
          font-normal
          bg-white
          shadow-md
          rounded-lg
          overflow-hidden
          border
          mt-4
          w-full
          lg:w-160
          z-30
          lg:z-10
          left-0
          lg:-left-16
        "
      >
        <div class="flex flex-col lg:flex-row px-8 py-6 border-b -mx-4">
          <ul class="w-full lg:w-1/2 px-4">
            <li
              class="mb-8"
              v-for="(submenu, index) in this.menuItem.sub_menus"
              :key="index"
            >
              <a
                href="#"
                class="flex group"
                @keydown.esc.exact="hideMenu"
                @keydown.tab.exact="focusNext(false)"
                @keydown.down.exact.prevent="focusNext(true)"
                @keydown.up.exact.prevent=""
              >
                <span class="ml-2">
                  <span
                    class="
                      block
                      font-bold
                      text-blue-800
                      group-hover:text-blue-800
                      flex
                      items-center
                    "
                  >
                    <span>{{ submenu["name_" + currentLocale] }}</span>
                  </span>
                </span>
              </a>
            </li>
          </ul>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  mounted() {
    this.menuItems = document.querySelectorAll(".mega-menu a");
  },
  props: ["menuItem"],
  data() {
    return {
      isVisible: false,
      menuItems: null,
      focusedIndex: 0,
    };
  },

  computed: {
    currentLocale() {
      //   return this.$i18n.locale.toString();
      return this.$route.params.lang.toString().split("-")[0];
    },
  },
  methods: {
    showMenu() {
      this.isVisible = true;
    },
    hideMenu() {
      this.isVisible = false;
      this.focusedIndex = 0;
    },
    startArrowKeys() {
      this.menuItems[0].focus();
    },
    focusPrevious(isArrowKey) {
      this.focusedIndex = this.focusedIndex - 1;

      if (isArrowKey) {
        this.focusItem();
      }
    },
    focusNext(isArrowKey) {
      this.focusedIndex = this.focusedIndex + 1;

      if (isArrowKey) {
        this.focusItem();
      }
    },
    focusItem() {
      this.menuItems[this.focusedIndex].focus();
    },
  },
};
</script>

<style scoped>
.mega-menu-fade-enter-active,
.mega-menu-fade-leave-active {
  transition: all 0.1s ease-in-out;
}
.mega-menu-fade-enter,
.mega-menu-fade-leave-to {
  opacity: 0;
  transform: translateY(-12px);
}
</style>
