<template>
  <div class="bg-gray-900 text-white flex flex-col min-h-screen">
    <HeaderPage />

    <div class="container mx-auto px-6 py-8 flex-1 mt-16">
      <!-- 系统运行信息卡片 -->
      <div
        class="bg-gray-800/40 backdrop-blur-xl p-8 rounded-2xl shadow-2xl border border-gray-700/30 mb-8"
      >
        <!-- 添加 mb-8 底部间距 -->
        <div class="flex items-center justify-between mb-6">
          <h2 class="text-xl font-medium tracking-wide text-gray-200">
            <i class="ri-dashboard-3-line mr-2"></i>
            系统状态
          </h2>
          <button
            @click="fetchSystemInfo"
            class="px-4 py-2.5 rounded-xl text-sm font-medium bg-gray-700/50 hover:bg-gray-600/50 text-gray-200 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-gray-600/50"
          >
            <i class="ri-refresh-line mr-2"></i>
            刷新信息
          </button>
        </div>

        <div
          v-if="systemInfo"
          class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6"
        >
          <StatusCard
            v-for="(info, key) in systemInfoCards"
            :key="key"
            :title="info.title"
            :value="systemInfo[info.key]"
            class="bg-gray-900/50 backdrop-blur-sm border border-gray-700/30 rounded-xl p-6 transition-all duration-200 hover:bg-gray-900/70"
          />
        </div>

        <div
          v-else
          class="flex items-center justify-center py-12 text-sm text-gray-400"
        >
          <svg
            class="animate-spin -ml-1 mr-3 h-5 w-5 text-gray-400"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
          >
            <circle
              class="opacity-25"
              cx="12"
              cy="12"
              r="10"
              stroke="currentColor"
              stroke-width="4"
            ></circle>
            <path
              class="opacity-75"
              fill="currentColor"
              d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
            ></path>
          </svg>
          加载中...
        </div>
      </div>

      <!-- 工具状态卡片 -->
      <div
        class="bg-gray-800/40 backdrop-blur-xl p-8 rounded-2xl shadow-2xl border border-gray-700/30 mb-8"
      >
        <div class="flex items-center justify-between mb-6">
          <h2 class="text-xl font-medium tracking-wide text-gray-200">
            <i class="ri-tools-line mr-2"></i>
            工具状态
          </h2>
          <button
            @click="fetchToolsStatus"
            class="px-4 py-2.5 rounded-xl text-sm font-medium bg-gray-700/50 hover:bg-gray-600/50 text-gray-200 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-gray-600/50"
          >
            <i class="ri-search-eye-line mr-2"></i>
            检查工具
          </button>
        </div>

        <div
          v-if="toolsInfo"
          class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6"
        >
          <div
            v-for="(status, tool) in toolsInfo.installedStatus"
            :key="tool"
            class="bg-gray-900/50 backdrop-blur-sm border border-gray-700/30 rounded-xl p-6 transition-all duration-200 hover:bg-gray-900/70"
          >
            <div class="flex flex-col space-y-2">
              <div class="flex items-center justify-between">
                <span class="text-gray-300 font-medium">
                  <i class="ri-terminal-box-line mr-2"></i>
                  {{ tool }}
                </span>
                <span :class="status ? 'text-green-400' : 'text-red-400'">
                  {{ status ? "已安装 ✓" : "未安装 ✗" }}
                </span>
              </div>
              <div
                v-if="toolsInfo.versions && toolsInfo.versions[tool]"
                class="text-sm text-gray-400"
              >
                <i class="ri-code-line mr-2"></i>
                版本: {{ toolsInfo.versions[tool] }}
              </div>
            </div>
          </div>
        </div>

        <div
          v-else
          class="flex items-center justify-center py-12 text-sm text-gray-400"
        >
          <svg
            class="animate-spin -ml-1 mr-3 h-5 w-5 text-gray-400"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
          >
            <circle
              class="opacity-25"
              cx="12"
              cy="12"
              r="10"
              stroke="currentColor"
              stroke-width="4"
            ></circle>
            <path
              class="opacity-75"
              fill="currentColor"
              d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
            ></path>
          </svg>
          加载中...
        </div>
      </div>

      <!-- 关于本系统卡片 -->
      <div
        class="bg-gray-800/40 backdrop-blur-xl p-8 rounded-2xl shadow-2xl border border-gray-700/30"
      >
        <div class="flex items-center mb-6">
          <h2 class="text-xl font-medium tracking-wide text-gray-200">
            <i class="ri-information-line mr-2"></i>
            关于本系统
          </h2>
        </div>

        <div class="bg-gray-900/50 backdrop-blur-sm border border-gray-700/30 rounded-xl p-6">
          <div class="space-y-4">
            <div class="flex items-start">
              <i class="ri-shield-keyhole-line text-xl text-cyan-400 mt-1 mr-3"></i>
              <p class="text-gray-300 leading-relaxed">
                本平台的核心思路是从攻击者视角出发，对企业的网络资产进行全面、深入的审视与管理。
              </p>
            </div>
            
            <div class="flex items-start">
              <i class="ri-radar-line text-xl text-cyan-400 mt-1 mr-3"></i>
              <p class="text-gray-300 leading-relaxed">
                通过自动化手段，实现对网络空间的快速测绘，精准识别资产，将资产、漏洞、任务等进行统一管理，以便企业安全团队能够清晰、直观地了解网络安全状态，并迅速做出响应。
              </p>
            </div>
            
            <div class="flex items-start">
              <i class="ri-sword-line text-xl text-cyan-400 mt-1 mr-3"></i>
              <p class="text-gray-300 leading-relaxed">
                旨在改变传统安全防护的被动局面，主动出击，提前发现并解决可能被攻击者利用的安全问题，最大程度降低企业网络安全风险。
              </p>
            </div>
            
            <div class="mt-6 pt-6 border-t border-gray-700/30">
              <div class="flex items-center justify-between">
                <span class="text-gray-400 text-sm">
                  <i class="ri-github-fill mr-2"></i>
                  CyberHawk-网络鹰平台
                </span>
                <span class="text-cyan-400 text-sm font-medium">
                  v{{ version }}
                </span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <FooterPage />

    <PopupNotification
      v-if="showNotification"
      :message="notificationMessage"
      :type="notificationType"
      @close="showNotification = false"
    />
  </div>
</template>

<script>
import { ref, onMounted } from "vue";
import api from "../../api/axiosInstance";
import HeaderPage from "../HeaderPage.vue";
import FooterPage from "../FooterPage.vue";
import PopupNotification from "../Utils/PopupNotification.vue";
import StatusCard from "../Utils/StatusCard.vue";
import { useNotification } from "../../composables/useNotification";

export default {
  name: "SystemStatus",
  components: {
    HeaderPage,
    FooterPage,
    PopupNotification,
    StatusCard,
  },
  setup() {
    const systemInfo = ref(null);
    const toolsInfo = ref(null); // 改名以匹配后端响应
    const version = ref("1.0.0"); // 添加系统版本

    const {
      showNotification,
      notificationMessage,
      notificationType,
      showSuccess,
      showError,
    } = useNotification();

    const systemInfoCards = {
      currentDirectory: { title: "程序运行目录", key: "currentDirectory" },
      localIP: { title: "本机 IP", key: "localIP" },
      publicIP: { title: "外网 IP", key: "publicIP" },
      kernelVersion: { title: "系统内核", key: "kernelVersion" },
      osDistribution: { title: "系统版本", key: "osDistribution" },
      privileges: { title: "运行权限", key: "privileges" },
    };

    const fetchSystemInfo = async () => {
      try {
        const response = await api.get("/system/info");
        if (response.data?.data?.systemInfo) {
          systemInfo.value = response.data.data.systemInfo;
          showSuccess("系统信息已更新");
        }
      } catch (error) {
        showError("获取系统信息失败");
      }
    };

    const fetchToolsStatus = async () => {
      try {
        const response = await api.get("/system/tools"); // 修改为新的API路径
        if (
          response.data?.status === "success" &&
          response.data?.data?.toolsInfo
        ) {
          toolsInfo.value = response.data.data.toolsInfo;
          showSuccess("工具状态已更新");
        }
      } catch (error) {
        showError(error.response?.data?.message || "获取工具状态失败");
      }
    };

    onMounted(() => {
      fetchSystemInfo();
      fetchToolsStatus();
    });

    return {
      systemInfo,
      toolsInfo,
      systemInfoCards,
      fetchSystemInfo,
      fetchToolsStatus,
      showNotification,
      notificationMessage,
      notificationType,
      version, // 导出版本变量
    };
  },
};
</script>

<style scoped>
.backdrop-blur-xl {
  backdrop-filter: blur(24px);
  -webkit-backdrop-filter: blur(24px);
}

/* 优化按钮点击效果 */
button:active {
  transform: scale(0.98);
}

/* 自定义滚动条 */
::-webkit-scrollbar {
  width: 6px;
  height: 6px;
}

::-webkit-scrollbar-track {
  background: transparent;
}

::-webkit-scrollbar-thumb {
  background: rgba(156, 163, 175, 0.3);
  border-radius: 3px;
}

::-webkit-scrollbar-thumb:hover {
  background: rgba(156, 163, 175, 0.5);
}
</style>
