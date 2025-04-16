<template>
  <div class="flex relative">
    <LeftSidebarMenu :isVisible="isCryptoToolsVisible" />
    <div class="flex-1">
      <router-view />
    </div>
    <RightSidebarMenu :isVisible="isNetworkToolsVisible" />
  </div>
</template>

<script>
import LeftSidebarMenu from "./components/LeftSidebarMenu.vue";
import RightSidebarMenu from "./components/RightSidebarMenu.vue";
import { computed } from "vue";
import { useStore } from "vuex";
import { mapState, mapActions } from "vuex";

export default {
  name: "App",
  components: {
    LeftSidebarMenu,
    RightSidebarMenu,
  },
  setup() {
    const store = useStore();
    const isAuthenticated = computed(() => store.state.isAuthenticated);
    return {
      isAuthenticated,
    };
  },
  computed: {
    ...mapState(['isCryptoToolsVisible', 'isNetworkToolsVisible'])
  },
  methods: {
    ...mapActions(['toggleCryptoTools', 'toggleNetworkTools'])
  }
};
</script>

<style>
.flex {
  display: flex;
}

.relative {
  position: relative;
}

/* 可选：添加按钮按下效果 */
button:active {
  transform: scale(0.98);
}
</style>
