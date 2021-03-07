<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated class="one-bar-color">
      <q-toolbar>
        <q-avatar>
          <img src="~assets/One Studio.svg" />
        </q-avatar>
        <q-toolbar-title class="one-font-color">
          CSGO HUD Generator
        </q-toolbar-title>
        <q-tabs align="left" class="one-font-color">
          <q-route-tab to="/" label="主页" />
          <q-route-tab to="/deathnotice" label="击杀生成" />
          <q-route-tab to="/crosshair" label="准星生成" />
          <q-route-tab to="/other" label="其他" />
        </q-tabs>
        <q-btn flat round dense icon="whatshot" />
      </q-toolbar>
    </q-header>
    <q-footer class="one-bar-color q-mb-md">
      <div class="footer">
        <span style="margin:auto" class="one-font-color">
          - Powered by <a href="https://github.com/One-Studio">One Studio</a> -
        </span>
      </div>
    </q-footer>
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
const linksData = [
  {
    title: "Docs",
    caption: "quasar.dev",
    icon: "school",
    link: "https://quasar.dev"
  },
  {
    title: "Github",
    caption: "github.com/quasarframework",
    icon: "code",
    link: "https://github.com/quasarframework"
  },
  {
    title: "Discord Chat Channel",
    caption: "chat.quasar.dev",
    icon: "chat",
    link: "https://chat.quasar.dev"
  },
  {
    title: "Forum",
    caption: "forum.quasar.dev",
    icon: "record_voice_over",
    link: "https://forum.quasar.dev"
  },
  {
    title: "Twitter",
    caption: "@quasarframework",
    icon: "rss_feed",
    link: "https://twitter.quasar.dev"
  },
  {
    title: "Facebook",
    caption: "@QuasarFramework",
    icon: "public",
    link: "https://facebook.quasar.dev"
  },
  {
    title: "Quasar Awesome",
    caption: "Community Quasar projects",
    icon: "favorite",
    link: "https://awesome.quasar.dev"
  }
];

export default {
  name: "MainLayout",
  data() {
    return {
      leftDrawerOpen: false,
      essentialLinks: linksData
    };
  }
};
</script>

<style lang="scss" scoped>
.footer {
  clear: both;
  text-align: center;
  vertical-align: middle;
  // margin: 30px auto 30px auto;
}
</style>
