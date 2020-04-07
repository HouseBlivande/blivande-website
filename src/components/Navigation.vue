<template>
  <header>
    <div class="menu md:pr-2">
      <span class="nav-item logo"></span>
      <scrollactive
        class="nav"
        active-class="active"
        :offset="80"
        :duration="800"
        bezier-easing-value=".5,0,.35,1"
      >
        <a
          class="nav-item scrollactive-item"
          :href="'#' + item.id"
          v-for="item in data"
          :key="item.id"
          >
          <span v-if="item.nav">{{ item.nav }}</span>
          <span v-else>{{ item.title }}</span>
        </a>
      </scrollactive>
    </div>
  </header>
</template>

<script>
/**
 * Used as main page navigation in templates.
 */
export default {
  name: "Navigation",
  props: ["data"]
};
</script>

<style lang="scss" scoped>
.menu {
  background-color: #000;
  display: flex;
  position: fixed;
  top: 0;
  border-bottom: 1px solid rgba(0, 0, 0, 0.05) !important;
  z-index: 11;
  justify-content: space-between;
  width: 100%;
  align-items: center;
}

.nav {
  @apply font-display font-bold;
  white-space: nowrap;
  overflow-x: scroll;
  width: screen;
  overflow-y: hidden;
  -webkit-overflow-scrolling: touch;
  -ms-overflow-style: -ms-autohiding-scrollbar;
  &::-webkit-scrollbar {
    display: none;
  }
}

.nav-item {
  font-family: "Raleway";
  font-weight: 300;
  height: 45px;
  display: inline-flex;
  cursor: pointer;
  padding: 16px 16px 14px;
  text-transform: capitalize;
  align-items: center;
  color: white !important;
  border-right: 1px solid rgba(0, 0, 0, 0.05) !important;
  &:last-child {
    border-right: none !important;
  }
  &.active {
    font-weight: 700;
    text-decoration: underline;
  }
  &:not(:last-child) {
    border-right: 0.5px solid rgba(255, 255, 255, 0.2);
  }
}

.logo {
  min-width: 65px;
  height: 60px;
  padding: 0 30px 0 70px;
  border-right: 1px solid rgba(255, 255, 255, 0.28);
  background: url("https://forum.blivande.com/uploads/blivande/original/1X/2abfc727dad7a3f2c48a94686fff362d49feaafe.png")
    no-repeat center #fff;
  background-size: 45%;
  transition: none;
  box-shadow: none;
  left: 0;
  filter: invert(1);
  z-index: 99999;
}
</style>
