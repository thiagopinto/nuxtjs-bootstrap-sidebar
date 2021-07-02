<template>
  <div class="wrapper">
    <!-- Sidebar -->
    <nav
      id="sidebar"
      :class="`sidebar vue-bootstrap-sidebar bg-light text-dark default-theme  ${
        [theme] && [show ? 'sidebar-visible' : 'sidebar-hidden']
      } ${align}`"
      data-color="white"
      data-active-color="danger"
    >
      <div class="sidebar-header logo" @click="onButtonClick">
        <slot name="logo" />
      </div>
      <div class="sidebar-wrapper">
        <b-list-group class="items-wrapper nav">
          <template v-for="(link, index) in links">
            <template v-if="link.href !== undefined">
              <div
                class="list-group-item list-group-item-action"
                :key="index"
                v-b-tooltip.hover.right
                :title="link.name"
              >
                <b-button
                  block
                  :to="link.href"
                  variant="info"
                  class="btn sidebar-menu-item"
                  :class="`${align}`"
                  :squared="true"
                  size="lg"
                >
                  <div class="fa-icon">
                    <component
                      v-if="link.faIcon"
                      :is="'font-awesome-icon'"
                      :icon="link.faIcon"
                    />
                  </div>
                  <div class="link-name">
                    {{ link.name }}
                  </div>
                </b-button>
              </div>
            </template>

            <template v-else>
              <div
                class="list-group-item list-group-item-action"
                :key="index"
                v-b-tooltip.hover.right
                :title="link.name"
              >
                <b-button
                  v-b-toggle="`accordion-${index + 10}`"
                  block
                  variant="info"
                  class="sidebar-menu-item dropdown-toggle rounded-0"
                  :class="`${align}`"
                  size="lg"
                >
                  <div class="fa-icon">
                    <component
                      v-if="link.faIcon"
                      :is="'font-awesome-icon'"
                      :icon="link.faIcon"
                    />
                  </div>
                  <div class="link-name">
                    {{ link.name }}
                  </div>
                </b-button>
              </div>
              <b-collapse
                :id="`accordion-${index + 10}`"
                :key="index + 10"
                accordion="my-accordion"
                role="tabpanel"
              >
                <b-list-group>
                  <div
                    class="list-group-item list-group-item-action"
                    v-for="(child, idx) in link.children"
                    :key="idx"
                  >
                    <b-button
                      block
                      :to="child.href"
                      variant="primary"
                      class="sidebar-menu-item rounded-0 child-level-1"
                      :class="`${align}`"
                      :squared="true"
                    >
                      <div class="fa-icon">
                        <component
                          v-if="child.faIcon"
                          :is="'font-awesome-icon'"
                          :icon="child.faIcon"
                        />
                      </div>
                      <div class="link-name">
                        {{ child.name }}
                      </div>
                    </b-button>
                  </div>
                </b-list-group>
              </b-collapse>
            </template>
          </template>
        </b-list-group>
      </div>
      <slot name="footer" />
      <!--/ .items-wrapper -->
      <div
        id="sidebarButton"
        class="sidebar-button"
        :class="[theme, { cross: show }, show ? 'visible' : 'hidden']"
        @click="onButtonClick"
      >
        <div class="bar1" />
        <div class="bar2" />
        <div class="bar3" />
      </div>
    </nav>
    <div class="main-panel">
      <nav
        class="
          navbar navbar-expand-lg navbar-absolute
          fixed-top
          navbar-transparent
        "
      >
        <div
          id="navbar"
          :class="`container-fluid ${[
            show ? 'sidebar' : 'no-sidebar',
          ]} ${align}`"
        >
          <slot name="navbar" />
        </div>
      </nav>

      <!-- Hamburger Menu -->
      <div
        id="content"
        :class="`content ${[show ? 'sidebar' : 'no-sidebar']} ${align}`"
      >
        <slot name="content" />
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "NuxtjsBootstrapSidebar",
  components: {},
  props: {
    links: {
      type: Array,
      default: null,
    },
    initialShow: {
      type: Boolean,
      default: false,
    },
    theme: {
      type: String,
      default: "default-theme",
    },
    header: {
      type: String,
      default: "",
    },
    align: {
      type: String,
      default: "left",
    },
  },
  data() {
    return {
      show: this.initialShow,
    };
  },
  beforeMount() {
    /*     this.$nextTick(function () {
      this.onResize();
    }); */

    window.addEventListener("resize", this.onResize);
    let width = window.document.documentElement.clientWidth;
    if (width <= 991.98) {
      this.show = false;
    } else {
      this.show = true;
    }
  },
  beforeDestroy() {
    window.removeEventListener("resize", this.onResize);
  },
  methods: {
    onButtonClick() {
      this.show = !this.show;
      this.$emit("sidebar-changed", this.show);
    },
    onResize(windowResize) {
      let targetElement = windowResize.target || windowResize.srcElement;
      if (targetElement != undefined) {
        let width = targetElement.document.documentElement.clientWidth;
        if (width <= 991.98) {
          this.show = false;
        } else {
          this.show = true;
        }
      }
    },
  },
};
</script>
<style lang="scss">
@import "../assets/scss/default-theme";
</style>
