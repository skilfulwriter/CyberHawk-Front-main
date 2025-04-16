<template>
  <div class="bg-gradient-to-r from-gray-900 via-gray-800 to-gray-900 text-white flex flex-col min-h-screen">
    <HeaderPage />
    <div class="container mx-auto px-4 sm:px-6 pt-24 sm:pt-28 pb-16 sm:pb-20 flex-1">
      <!-- 欢迎信息 -->
      <div class="text-center mb-6 sm:mb-8 px-2">
        <h1 class="text-3xl sm:text-4xl font-bold mb-3 sm:mb-4 tracking-tight bg-gradient-to-r from-blue-400 to-purple-500 bg-clip-text text-transparent">
          CyberHawk-网络鹰
        </h1>
        <p class="text-base sm:text-lg text-gray-300 font-light tracking-wide max-w-2xl mx-auto leading-relaxed">
          高级网络情报搜集平台-简单、高效、专业的资产扫描工具
        </p>
      </div>

      <!-- 搜索组件 -->
      <div class="mb-8">
        <SearchComponent />
      </div>
      
      <!-- 功能卡片网格 -->
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4 sm:gap-6">
        <div
            v-for="(card, index) in cards"
            :key="index"
            class="bg-gray-800/40 backdrop-blur-lg p-5 sm:p-6 rounded-xl shadow-lg hover:shadow-xl transform hover:scale-[1.02] hover:bg-gray-800/50 transition-all duration-300 ease-out border border-gray-700/30 card-item"
            :style="{ animationDelay: `${index * 0.05}s` }"
        >
          <div class="flex items-center mb-3 sm:mb-4 space-x-3">
            <div class="w-8 h-8 rounded-lg bg-gray-700/50 flex items-center justify-center">
              <i :class="card.icon" class="text-xl text-blue-400"></i>
            </div>
            <h3 class="text-base font-medium tracking-wide text-white">
              {{ card.title }}
            </h3>
          </div>
          <p class="text-gray-400 text-sm leading-relaxed">
            {{ card.description }}
          </p>
        </div>
      </div>
    </div>
    <FooterPage />
  </div>
</template>

<script>
import HeaderPage from "./HeaderPage.vue";
import FooterPage from "./FooterPage.vue";
import SearchComponent from "./Search/SearchComponent.vue";

export default {
  name: "HomePage",
  components: {
    HeaderPage,
    FooterPage,
    SearchComponent
  },
  data() {
    return {
      cards: [
        {
          icon: "ri-search-line",
          title: "资产发现",
          description: "快速准确的子域名扫描，全面掌握数字资产。",
        },
        {
          icon: "ri-global-line",
          title: "端口扫描",
          description: "高效的端口检测，精准识别服务组件。",
        },
        {
          icon: "ri-folders-line",
          title: "路径扫描",
          description: "深度探索网站结构，发现隐藏资源。",
        },
        {
          icon: "ri-fingerprint-line",
          title: "指纹识别",
          description: "准确识别技术组件，提供详细分析。",
        },
        {
          icon: "ri-shield-check-line",
          title: "安全评估",
          description: "全面的漏洞检测，提供专业评估。",
        },
        {
          icon: "ri-flashlight-line",
          title: "高性能",
          description: "优化的扫描引擎，提供卓越性能。",
        },
        {
          icon: "ri-bar-chart-2-line",
          title: "可视化",
          description: "清晰的数据展示，直观的进度管理。",
        },
        {
          icon: "ri-task-line",
          title: "任务管理",
          description: "灵活的任务调度，高效资源利用。",
        },
        {
          icon: "ri-rocket-line",
          title: "快速部署",
          description: "简单的部署流程，快速开始扫描。",
        },
        {
          icon: "ri-line-chart-line",
          title: "数据分析",
          description: "实时的数据分析，清晰的安全概览。",
        },
        {
          icon: "ri-file-text-line",
          title: "安全报告",
          description: "专业的报告生成，便于安全决策。",
        },
        {
          icon: "ri-settings-3-line",
          title: "配置管理",
          description: "灵活的配置选项，满足多样需求。",
        },
        {
          icon: "ri-refresh-line",
          title: "定期更新",
          description: "持续的特征更新，保持安全防护。",
        },
        {
          icon: "ri-device-line",
          title: "多平台",
          description: "支持多种平台，保持一致体验。",
        },
        {
          icon: "ri-cloud-line",
          title: "协议支持",
          description: "支持主流协议，全面扫描覆盖。",
        },
      ],
    };
  },
};
</script>

<style scoped>
@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.card-item {
  animation: fadeInUp 0.5s ease-out forwards;
  animation-fill-mode: both;
}

/* 移动端优化样式 */
@media (max-width: 640px) {
  .container {
    padding-top: 5rem;
  }
  
  .card-item {
    transform: scale(1);
    transition: transform 0.2s, background-color 0.2s;
  }
  
  .card-item:active {
    transform: scale(0.98);
    background-color: rgba(31, 41, 55, 0.6);
  }
}

/* 滚动条样式 */
::-webkit-scrollbar {
  width: 5px;
}

::-webkit-scrollbar-track {
  background: rgba(0, 0, 0, 0.1);
}

::-webkit-scrollbar-thumb {
  background: rgba(255, 255, 255, 0.2);
  border-radius: 3px;
}

::-webkit-scrollbar-thumb:hover {
  background: rgba(255, 255, 255, 0.3);
}
</style>