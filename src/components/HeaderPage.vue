<template>
  <div>
  <nav
      class="bg-gradient-to-r from-gray-800 via-gray-900 to-gray-800 backdrop-blur-md p-4 shadow-lg fixed w-full z-10 transition-all duration-500 border-b border-gray-700/50"
  >
    <div class="container mx-auto flex justify-between items-center">
      <!-- Logo区域 -->
        <div class="flex items-center group nav-title-container p-1">
          <img src="@/assets/logo.png" alt="Logo" class="h-8 mr-2 transition-transform duration-300 group-hover:scale-110 group-hover:rotate-3 filter drop-shadow-lg">
          <span class="nav-title text-2xl font-bold tracking-wide bg-gradient-to-r from-blue-400 via-indigo-400 to-purple-500 bg-clip-text text-transparent md:block hidden transform transition-all duration-300 group-hover:scale-105 drop-shadow-md logo-text-glow">CyberHawk-网络鹰</span>
          <span class="nav-title text-xl font-bold tracking-wide bg-gradient-to-r from-blue-400 via-indigo-400 to-purple-500 bg-clip-text text-transparent md:hidden block transform transition-all duration-300 group-hover:scale-105 drop-shadow-md logo-text-glow">网络鹰</span>
      </div>

        <!-- 移动端汉堡菜单按钮 -->
        <button 
          @click="toggleMobileMenu" 
          class="md:hidden block text-white focus:outline-none transition-all duration-300 hover:scale-110 hover:text-blue-400"
          type="button"
        >
          <i class="ri-menu-line text-2xl" v-if="!mobileMenuOpen"></i>
          <i class="ri-close-line text-2xl" v-else></i>
        </button>

        <!-- 导航按钮区域 - 桌面端 -->
        <div class="space-x-1 relative hidden md:flex items-center">
        <!-- 未登录状态 -->
        <template v-if="!isAuthenticated">
          <router-link to="/login" v-slot="{ navigate }">
            <button
              @click="navigate"
                class="text-sm font-medium text-gray-200 hover:text-white transition-all duration-300 px-3 py-2 rounded-lg hover:bg-gray-700/50 hover:shadow-lg flex items-center group"
            >
                <i class="ri-login-box-line mr-1 group-hover:text-blue-400 transition-colors duration-300 transform group-hover:scale-110"></i>
                <span class="transform transition-all duration-300 group-hover:translate-x-1">登录</span>
            </button>
          </router-link>
          <router-link to="/setup-2fa" v-slot="{ navigate }">
            <button
              @click="navigate"
                class="text-sm font-medium text-gray-200 hover:text-white transition-all duration-300 px-3 py-2 rounded-lg hover:bg-gray-700/50 hover:shadow-lg flex items-center group"
              >
                <i class="ri-user-add-line mr-1 group-hover:text-blue-400 transition-colors duration-300 transform group-hover:scale-110"></i>
                <span class="transform transition-all duration-300 group-hover:translate-x-1">注册</span>
              </button>
            </router-link>
            
            <!-- 新增5个导航按钮（仅在未登录状态显示） -->
            <a
              href="https://xheishou.com/"
              target="_blank"
              rel="noopener noreferrer"
              class="text-sm font-medium text-gray-200 hover:text-white transition-all duration-300 px-3 py-2 rounded-lg hover:bg-gray-700/50 hover:shadow-[0_0_15px_rgba(59,130,246,0.5)] flex items-center group"
            >
              <i class="ri-rocket-line mr-1 group-hover:text-blue-400 transition-all duration-300 transform group-hover:scale-110"></i>
              <span class="transform transition-all duration-300 group-hover:translate-x-0.5">首页</span>
            </a>
            
            <a
              href="https://bbs.x10001.com/x%e9%bb%91%e6%89%8b-%e7%bd%91%e5%9d%80%e5%af%bc%e8%88%aa"
              target="_blank"
              rel="noopener noreferrer"
              class="text-sm font-medium text-gray-200 hover:text-white transition-all duration-300 px-3 py-2 rounded-lg hover:bg-gray-700/50 hover:shadow-[0_0_15px_rgba(59,130,246,0.5)] flex items-center group"
            >
              <i class="ri-lightbulb-line mr-1 group-hover:text-blue-400 transition-all duration-300 transform group-hover:scale-110"></i>
              <span class="transform transition-all duration-300 group-hover:translate-x-0.5">导航</span>
            </a>
            
            <!-- 功能特点下拉菜单 -->
            <div class="relative group inline-block">
              <button
                @click="toggleDropdown('features')"
                class="text-sm font-medium text-gray-200 hover:text-white transition-all duration-300 px-3 py-2 rounded-lg hover:bg-gray-700/50 hover:shadow-[0_0_15px_rgba(59,130,246,0.5)] flex items-center group"
              >
                <i class="ri-function-line mr-1 group-hover:text-blue-400 transition-all duration-300 transform group-hover:scale-110"></i>
                <span class="transform transition-all duration-300 group-hover:translate-x-0.5">安全工具</span>
                <i class="ri-arrow-down-s-line ml-1 transition-transform duration-300" :class="{'rotate-180': dropdowns.features}"></i>
              </button>
              <div
                v-show="dropdowns.features"
                class="absolute left-0 bg-gray-800/95 backdrop-blur-md text-white rounded-lg shadow-xl mt-2 transition-all duration-200 border border-gray-700/50 w-48 animate-dropdown overflow-hidden"
              >
                <a 
                  href="https://xftf.xheishou.top/login" 
                  target="_blank" 
                  rel="noopener noreferrer"
                  class="block" 
                  @click="mobileMenuOpen = false"
                >
                  <div class="flex items-center justify-between p-3 rounded-lg hover:bg-gray-700/60 text-white transition-all duration-300 hover:translate-x-1 group border-l-2 border-transparent hover:border-blue-500">
                    <span class="flex items-center">
                      <i class="ri-shield-check-line mr-3 text-blue-400 text-xl group-hover:text-blue-300 transition-colors"></i>
                      <span class="text-lg bg-gradient-to-r from-white to-gray-300 bg-clip-text group-hover:text-transparent group-hover:from-blue-300 group-hover:to-indigo-300 transition-all duration-300">Gophish钓鱼平台</span>
                    </span>
                    <i class="ri-external-link-line group-hover:text-blue-300 transition-colors"></i>
                  </div>
                </a>

                <a 
                  href="https://xheishou.com/hacker.html" 
                  target="_blank" 
                  rel="noopener noreferrer"
                  class="block" 
                  @click="mobileMenuOpen = false"
                >
                  <div class="flex items-center justify-between p-3 rounded-lg hover:bg-gray-700/60 text-white transition-all duration-300 hover:translate-x-1 group border-l-2 border-transparent hover:border-blue-500">
                    <span class="flex items-center">
                      <i class="ri-settings-5-line mr-3 text-blue-400 text-xl group-hover:text-blue-300 transition-colors"></i>
                      <span class="text-lg bg-gradient-to-r from-white to-gray-300 bg-clip-text group-hover:text-transparent group-hover:from-blue-300 group-hover:to-indigo-300 transition-all duration-300">渗透测试闯关</span>
                    </span>
                    <i class="ri-external-link-line group-hover:text-blue-300 transition-colors"></i>
                  </div>
                </a>

                <a 
                  href="https://xheishou.com/sql/index.html" 
                  target="_blank" 
                  rel="noopener noreferrer"
                  class="block" 
                  @click="mobileMenuOpen = false"
                >
                  <div class="flex items-center justify-between p-3 rounded-lg hover:bg-gray-700/60 text-white transition-all duration-300 hover:translate-x-1 group border-l-2 border-transparent hover:border-blue-500">
                    <span class="flex items-center">
                      <i class="ri-pie-chart-line mr-3 text-blue-400 text-xl group-hover:text-blue-300 transition-colors"></i>
                      <span class="text-lg bg-gradient-to-r from-white to-gray-300 bg-clip-text group-hover:text-transparent group-hover:from-blue-300 group-hover:to-indigo-300 transition-all duration-300">SQLMAP命令生成器</span>
                    </span>
                    <i class="ri-external-link-line group-hover:text-blue-300 transition-colors"></i>
                  </div>
                </a>

                <a
                  href="https://cyberhawk.xheishou.top/"
                  target="_blank"
                  rel="noopener noreferrer"
                  class="block"
                  @click="mobileMenuOpen = false"
                >
                  <div class="flex items-center justify-between p-3 rounded-lg hover:bg-gray-700/60 text-white transition-all duration-300 hover:translate-x-1 group border-l-2 border-transparent hover:border-blue-500">
                    <span class="flex items-center">
                      <i class="ri-link-m mr-3 text-blue-400 text-xl group-hover:text-blue-300 transition-colors"></i>
                      <span class="text-lg bg-gradient-to-r from-white to-gray-300 bg-clip-text group-hover:text-transparent group-hover:from-blue-300 group-hover:to-indigo-300 transition-all duration-300">高级情报网络搜集平台</span>
                    </span>
                    <i class="ri-external-link-line group-hover:text-blue-300 transition-colors"></i>
                  </div>
                </a>
              </div>
            </div>
            
            <a
              href="https://xheishou.com/zhanshi1.html"
              target="_blank"
              rel="noopener noreferrer"
              class="text-sm font-medium text-gray-200 hover:text-white transition-all duration-300 px-3 py-2 rounded-lg hover:bg-gray-700/50 hover:shadow-[0_0_15px_rgba(59,130,246,0.5)] flex items-center group"
            >
              <i class="ri-price-tag-3-line mr-1 group-hover:text-blue-400 transition-all duration-300 transform group-hover:scale-110"></i>
              <span class="transform transition-all duration-300 group-hover:translate-x-0.5">项目展示</span>
            </a>
            
            <a
              href="https://xheishou.com/index.html#services"
              target="_blank"
              rel="noopener noreferrer"
              class="text-sm font-medium text-gray-200 hover:text-white transition-all duration-300 px-3 py-2 rounded-lg hover:bg-gray-700/50 hover:shadow-[0_0_15px_rgba(59,130,246,0.5)] flex items-center group"
            >
              <i class="ri-team-line mr-1 group-hover:text-blue-400 transition-all duration-300 transform group-hover:scale-110"></i>
              <span class="transform transition-all duration-300 group-hover:translate-x-0.5">服务支持</span>
            </a>
            
            <!-- 新增联系反馈按钮 -->
            <a
              href="https://xheishou.com/index.html#contact"
              target="_blank"
              rel="noopener noreferrer"
              class="text-sm font-medium text-gray-200 hover:text-white transition-all duration-300 px-3 py-2 rounded-lg hover:bg-gray-700/50 hover:shadow-[0_0_15px_rgba(59,130,246,0.5)] flex items-center group"
            >
              <i class="ri-feedback-line mr-1 group-hover:text-blue-400 transition-all duration-300 transform group-hover:scale-110"></i>
              <span class="transform transition-all duration-300 group-hover:translate-x-0.5">联系反馈</span>
            </a>
            
            <!-- 新增使用教程按钮 -->
            <a
              href="https://bbs.x10001.com/shouye"
              target="_blank"
              rel="noopener noreferrer"
              class="text-sm font-medium text-gray-200 hover:text-white transition-all duration-300 px-3 py-2 rounded-lg hover:bg-gray-700/50 hover:shadow-[0_0_15px_rgba(59,130,246,0.5)] flex items-center group"
            >
              <i class="ri-book-open-line mr-1 group-hover:text-blue-400 transition-all duration-300 transform group-hover:scale-110"></i>
              <span class="transform transition-all duration-300 group-hover:translate-x-0.5">使用教程</span>
            </a>
          </template>

        <!-- 登录状态 -->
        <template v-else>
          <!-- 主页按钮 -->
          <router-link to="/" v-slot="{ navigate }">
            <button
              @click="navigate"
                class="text-sm font-medium text-gray-200 hover:text-white transition-all duration-300 px-3 py-2 rounded-lg hover:bg-gray-700/50 hover:shadow-[0_0_15px_rgba(59,130,246,0.5)] flex items-center group"
            >
                <i class="ri-home-line mr-1 group-hover:text-blue-400 transition-all duration-300 transform group-hover:scale-110"></i>
                <span class="transform transition-all duration-300 group-hover:translate-x-0.5">主页</span>
            </button>
          </router-link>

          <!-- 目标管理 -->
          <router-link to="/target-management" v-slot="{ navigate }">
            <button
              @click="navigate"
                class="text-sm font-medium text-gray-200 hover:text-white transition-all duration-300 px-3 py-2 rounded-lg hover:bg-gray-700/50 hover:shadow-[0_0_15px_rgba(59,130,246,0.5)] flex items-center group"
            >
                <i class="ri-focus-3-line mr-1 group-hover:text-blue-400 transition-all duration-300 transform group-hover:scale-110"></i>
                <span class="transform transition-all duration-300 group-hover:translate-x-0.5">目标管理</span>
            </button>
          </router-link>

          <!-- 攻击面搜集下拉菜单 -->
          <div class="relative group inline-block">
            <button
              @click="toggleDropdown('collection')"
                class="text-sm font-medium text-gray-200 hover:text-white transition-all duration-300 px-3 py-2 rounded-lg hover:bg-gray-700/50 hover:shadow-[0_0_15px_rgba(59,130,246,0.5)] flex items-center group"
            >
                <i class="ri-radar-line mr-1 group-hover:text-blue-400 transition-all duration-300 transform group-hover:scale-110"></i>
                <span class="transform transition-all duration-300 group-hover:translate-x-0.5">攻击面搜集</span>
                <i class="ri-arrow-down-s-line ml-1 transition-transform duration-300" :class="{'rotate-180': dropdowns.collection}"></i>
            </button>
            <div
              v-show="dropdowns.collection"
                class="absolute left-0 bg-gray-800/95 backdrop-blur-md text-white rounded-lg shadow-xl mt-2 transition-all duration-200 border border-gray-700/50 w-40 animate-dropdown overflow-hidden"
            >
              <router-link to="/subdomain-scan-results" v-slot="{ navigate }">
                <button
                  @click="navigate"
                    class="block w-full text-left px-4 py-2.5 text-sm hover:bg-gray-700/50 transition-all duration-200 flex items-center border-l-2 border-transparent hover:border-blue-400"
                >
                    <i class="ri-global-line mr-2 transition-all duration-300"></i>
                    <span>子域名发现</span>
                </button>
              </router-link>
              <router-link to="/port-scan-results" v-slot="{ navigate }">
                <button
                  @click="navigate"
                    class="block w-full text-left px-4 py-2.5 text-sm hover:bg-gray-700/50 transition-all duration-200 flex items-center border-l-2 border-transparent hover:border-blue-400"
                  >
                    <i class="ri-scan-2-line mr-2 transition-all duration-300"></i>
                    <span>端口扫描</span>
                  </button>
                </router-link>
                <router-link to="/under-development" v-slot="{ navigate }">
                  <button
                    @click="navigate"
                    class="block w-full text-left px-4 py-2.5 text-sm hover:bg-gray-700/50 transition-all duration-200 flex items-center border-l-2 border-transparent hover:border-blue-400"
                  >
                    <i class="ri-earth-line mr-2 transition-all duration-300"></i>
                    <span>空间测绘</span>
                </button>
              </router-link>
            </div>
          </div>

          <!-- 攻击面刻画下拉菜单 -->
          <div class="relative group inline-block">
            <button
              @click="toggleDropdown('characterization')"
                class="text-sm font-medium text-gray-200 hover:text-white transition-all duration-300 px-3 py-2 rounded-lg hover:bg-gray-700/50 hover:shadow-[0_0_15px_rgba(59,130,246,0.5)] flex items-center group"
            >
                <i class="ri-file-search-line mr-1 group-hover:text-blue-400 transition-all duration-300 transform group-hover:scale-110"></i>
                <span class="transform transition-all duration-300 group-hover:translate-x-0.5">攻击面刻画</span>
                <i class="ri-arrow-down-s-line ml-1 transition-transform duration-300" :class="{'rotate-180': dropdowns.characterization}"></i>
            </button>
            <div
              v-show="dropdowns.characterization"
                class="absolute left-0 bg-gray-800/95 backdrop-blur-md text-white rounded-lg shadow-xl mt-2 transition-all duration-200 border border-gray-700/50 w-40 animate-dropdown overflow-hidden"
            >
              <router-link to="/path-scan-results" v-slot="{ navigate }">
                <button
                  @click="navigate"
                    class="block w-full text-left px-4 py-2.5 text-sm hover:bg-gray-700/50 transition-all duration-200 flex items-center border-l-2 border-transparent hover:border-blue-400"
                >
                    <i class="ri-folders-line mr-2 transition-all duration-300"></i>
                    <span>路径扫描</span>
                </button>
              </router-link>
              <router-link to="/under-development" v-slot="{ navigate }">
                <button
                  @click="navigate"
                    class="block w-full text-left px-4 py-2.5 text-sm hover:bg-gray-700/50 transition-all duration-200 flex items-center border-l-2 border-transparent hover:border-blue-400"
                >
                    <i class="ri-fingerprint-line mr-2 transition-all duration-300"></i>
                    <span>指纹识别</span>
                </button>
              </router-link>
            </div>
          </div>

          <!-- 攻击面渗透下拉菜单 -->
          <div class="relative group inline-block">
            <button
              @click="toggleDropdown('penetration')"
                class="text-sm font-medium text-gray-200 hover:text-white transition-all duration-300 px-3 py-2 rounded-lg hover:bg-gray-700/50 hover:shadow-[0_0_15px_rgba(59,130,246,0.5)] flex items-center group"
            >
                <i class="ri-shield-keyhole-line mr-1 group-hover:text-blue-400 transition-all duration-300 transform group-hover:scale-110"></i>
                <span class="transform transition-all duration-300 group-hover:translate-x-0.5">攻击面渗透</span>
                <i class="ri-arrow-down-s-line ml-1 transition-transform duration-300" :class="{'rotate-180': dropdowns.penetration}"></i>
            </button>
            <div
              v-show="dropdowns.penetration"
                class="absolute left-0 bg-gray-800/95 backdrop-blur-md text-white rounded-lg shadow-xl mt-2 transition-all duration-200 border border-gray-700/50 w-40 animate-dropdown overflow-hidden"
            >
              <router-link to="/under-development" v-slot="{ navigate }">
                <button
                  @click="navigate"
                    class="block w-full text-left px-4 py-2.5 text-sm hover:bg-gray-700/50 transition-all duration-200 flex items-center border-l-2 border-transparent hover:border-blue-400"
                >
                    <i class="ri-bug-line mr-2 transition-all duration-300"></i>
                    <span>漏洞扫描</span>
                </button>
              </router-link>
              <router-link to="/under-development" v-slot="{ navigate }">
                <button
                  @click="navigate"
                    class="block w-full text-left px-4 py-2.5 text-sm hover:bg-gray-700/50 transition-all duration-200 flex items-center border-l-2 border-transparent hover:border-blue-400"
                >
                    <i class="ri-error-warning-line mr-2 transition-all duration-300"></i>
                    <span>漏洞利用️</span>
                </button>
              </router-link>
            </div>
          </div>

            <!-- 工具箱下拉菜单 -->
            <div class="relative group inline-block">
              <button
                @click="toggleDropdown('toolbox')"
                class="text-sm font-medium text-gray-200 hover:text-white transition-all duration-300 px-3 py-2 rounded-lg hover:bg-gray-700/50 hover:shadow-[0_0_15px_rgba(59,130,246,0.5)] flex items-center group"
              >
                <i class="ri-tools-line mr-1 group-hover:text-blue-400 transition-all duration-300 transform group-hover:scale-110"></i>
                <span class="transform transition-all duration-300 group-hover:translate-x-0.5">工具箱</span>
                <i class="ri-arrow-down-s-line ml-1 transition-transform duration-300" :class="{'rotate-180': dropdowns.toolbox}"></i>
              </button>
              <div
                v-show="dropdowns.toolbox"
                class="absolute left-0 bg-gray-800/95 backdrop-blur-md text-white rounded-lg shadow-xl mt-2 transition-all duration-200 border border-gray-700/50 w-40 animate-dropdown overflow-hidden"
              >
                <button
                  @click="toggleCryptoToolsAndCloseMenus"
                  class="block w-full text-left px-4 py-2.5 text-sm hover:bg-gray-700/50 transition-all duration-200 flex items-center border-l-2 border-transparent hover:border-blue-400"
                >
                  <i class="ri-lock-line mr-2 transition-all duration-300"></i>
                  <span>加密解密工具箱</span>
                </button>
                <button
                  @click="toggleNetworkToolsAndCloseMenus"
                  class="block w-full text-left px-4 py-2.5 text-sm hover:bg-gray-700/50 transition-all duration-200 flex items-center border-l-2 border-transparent hover:border-blue-400"
                >
                  <i class="ri-cloud-line mr-2 transition-all duration-300"></i>
                  <span>网络请求工具箱</span>
                </button>
              </div>
            </div>

            <!-- 其他导航按钮 -->
            <template v-for="(route, index) in otherRoutes" :key="index">
              <router-link :to="route.path" v-slot="{ navigate }">
            <button
              @click="navigate"
                  class="text-sm font-medium text-gray-200 hover:text-white transition-all duration-300 px-3 py-2 rounded-lg hover:bg-gray-700/50 hover:shadow-[0_0_15px_rgba(59,130,246,0.5)] flex items-center group"
            >
                  <i :class="route.icon + ' mr-1 group-hover:text-blue-400 transition-all duration-300 transform group-hover:scale-110'"></i>
                  <span class="transform transition-all duration-300 group-hover:translate-x-0.5">{{ route.name }}</span>
            </button>
          </router-link>
            </template>

            <!-- 登出按钮 -->
            <button
              @click="handleLogout"
              class="text-sm font-medium text-gray-200 hover:text-white transition-all duration-300 px-3 py-2 rounded-lg hover:bg-red-500/50 hover:shadow-[0_0_15px_rgba(239,68,68,0.5)] flex items-center group ml-2"
            >
              <i class="ri-logout-box-line mr-1 group-hover:text-red-400 transition-all duration-300 transform group-hover:scale-110"></i>
              <span class="transform transition-all duration-300 group-hover:translate-x-0.5">登出</span>
            </button>

            <!-- 添加关闭按钮 -->
            <button
              @click="closeNavigation"
              class="text-sm font-medium text-gray-200 hover:text-white transition-all duration-300 px-3 py-2 rounded-lg hover:bg-gray-700/50 hover:shadow-[0_0_15px_rgba(59,130,246,0.5)] flex items-center group ml-2"
            >
              <i class="ri-close-circle-line mr-1 group-hover:text-blue-400 transition-all duration-300 transform group-hover:scale-110"></i>
              <span class="transform transition-all duration-300 group-hover:translate-x-0.5">关闭</span>
            </button>
          </template>
        </div>
      </div>
    </nav>
    
    <!-- 移动端导航菜单 (简化版) -->
    <div v-if="mobileMenuOpen" class="fixed inset-0 bg-gray-900/95 backdrop-blur-md z-20 pt-16">
      <div class="px-4 py-4 overflow-y-auto h-full">
        <div v-if="!isAuthenticated" class="space-y-3">
          <router-link to="/login" class="block" @click="navigateAndCloseMenu">
            <div class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white">
              <span class="flex items-center">
                <i class="ri-login-box-line mr-3 text-blue-400 text-xl"></i>
                <span class="text-lg">登录</span>
              </span>
              <i class="ri-arrow-right-s-line"></i>
            </div>
          </router-link>
          
          <router-link to="/setup-2fa" class="block" @click="navigateAndCloseMenu">
            <div class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white">
              <span class="flex items-center">
                <i class="ri-user-add-line mr-3 text-blue-400 text-xl"></i>
                <span class="text-lg">注册</span>
              </span>
              <i class="ri-arrow-right-s-line"></i>
            </div>
          </router-link>
          
          <!-- 新增5个导航按钮（移动端未登录状态） -->
          <a 
            href="https://xheishou.com/" 
            target="_blank" 
            rel="noopener noreferrer" 
            class="block" 
            @click="mobileMenuOpen = false"
          >
            <div class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white">
              <span class="flex items-center">
                <i class="ri-rocket-line mr-3 text-blue-400 text-xl"></i>
                <span class="text-lg">首页</span>
              </span>
              <i class="ri-arrow-right-s-line"></i>
            </div>
          </a>
          
          <a 
            href="https://bbs.x10001.com/x%e9%bb%91%e6%89%8b-%e7%bd%91%e5%9d%80%e5%af%bc%e8%88%aa" 
            target="_blank" 
            rel="noopener noreferrer" 
            class="block" 
            @click="mobileMenuOpen = false"
          >
            <div class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white">
              <span class="flex items-center">
                <i class="ri-lightbulb-line mr-3 text-blue-400 text-xl"></i>
                <span class="text-lg">导航</span>
              </span>
              <i class="ri-external-link-line"></i>
            </div>
          </a>

          <!-- 功能特点菜单 -->
          <div class="mb-5">
            <div class="mb-2 border-b border-gray-700 pb-1">
              <h3 class="text-sm text-gray-400 px-2">安全工具</h3>
            </div>
            <div class="space-y-3">
              <a 
                href="https://xftf.xheishou.top/login" 
                target="_blank" 
                rel="noopener noreferrer" 
                class="block" 
                @click="mobileMenuOpen = false"
              >
                <div class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white">
                  <span class="flex items-center">
                    <i class="ri-shield-check-line mr-3 text-blue-400 text-xl"></i>
                    <span class="text-lg">Gophish钓鱼平台</span>
                  </span>
                  <i class="ri-external-link-line"></i>
                </div>
              </a>
              
              <a 
                href="https://xheishou.com/hacker.html" 
                target="_blank" 
                rel="noopener noreferrer" 
                class="block" 
                @click="mobileMenuOpen = false"
              >
                <div class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white">
                  <span class="flex items-center">
                    <i class="ri-settings-5-line mr-3 text-blue-400 text-xl"></i>
                    <span class="text-lg">渗透测试闯关</span>
                  </span>
                  <i class="ri-external-link-line"></i>
                </div>
              </a>
              
              <a 
                href="https://xheishou.com/sql/index.html" 
                target="_blank" 
                rel="noopener noreferrer"
                class="block" 
                @click="mobileMenuOpen = false"
              >
                <div class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white">
                  <span class="flex items-center">
                    <i class="ri-pie-chart-line mr-3 text-blue-400 text-xl"></i>
                    <span class="text-lg">SQLMAP命令生成器</span>
                  </span>
                  <i class="ri-external-link-line"></i>
                </div>
              </a>
              
              <a
                href="https://cyberhawk.xheishou.top/"
                target="_blank"
                rel="noopener noreferrer"
                class="block"
                @click="mobileMenuOpen = false"
              >
                <div class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white">
                  <span class="flex items-center">
                    <i class="ri-link-m mr-3 text-blue-400 text-xl"></i>
                    <span class="text-lg">高级情报网络搜集平台</span>
                  </span>
                  <i class="ri-external-link-line"></i>
                </div>
              </a>
            </div>
          </div>

          <a
            href="https://xheishou.com/zhanshi1.html"
            target="_blank"
            rel="noopener noreferrer"
            class="block"
            @click="mobileMenuOpen = false"
          >
            <div class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white">
              <span class="flex items-center">
                <i class="ri-price-tag-3-line mr-3 text-blue-400 text-xl"></i>
                <span class="text-lg">项目展示</span>
              </span>
              <i class="ri-external-link-line"></i>
            </div>
          </a>
          
          <a
            href="https://xheishou.com/index.html#services"
            target="_blank"
            rel="noopener noreferrer"
            class="block"
            @click="mobileMenuOpen = false"
          >
            <div class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white">
              <span class="flex items-center">
                <i class="ri-team-line mr-3 text-blue-400 text-xl"></i>
                <span class="text-lg">服务支持</span>
              </span>
              <i class="ri-external-link-line"></i>
            </div>
          </a>
          
          <!-- 新增联系反馈按钮 -->
          <a
            href="https://xheishou.com/index.html#contact"
            target="_blank"
            rel="noopener noreferrer"
            class="block"
            @click="mobileMenuOpen = false"
          >
            <div class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white">
              <span class="flex items-center">
                <i class="ri-feedback-line mr-3 text-blue-400 text-xl"></i>
                <span class="text-lg">联系反馈</span>
              </span>
              <i class="ri-external-link-line"></i>
            </div>
          </a>
          
          <!-- 新增使用教程按钮 -->
          <a
            href="https://bbs.x10001.com/shouye"
            target="_blank"
            rel="noopener noreferrer"
            class="block"
            @click="mobileMenuOpen = false"
          >
            <div class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white">
              <span class="flex items-center">
                <i class="ri-book-open-line mr-3 text-blue-400 text-xl"></i>
                <span class="text-lg">使用教程</span>
              </span>
              <i class="ri-external-link-line"></i>
            </div>
          </a>
          
          <!-- 未登录状态添加移动端关闭按钮 -->
          <div class="mt-6">
            <button
              @click="closeNavigation"
              class="w-full flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white"
            >
              <span class="flex items-center">
                <i class="ri-close-circle-line mr-3 text-blue-400 text-xl"></i>
                <span class="text-lg">关闭导航</span>
              </span>
              <i class="ri-close-line"></i>
            </button>
          </div>
        </div>
        
        <div v-else>
          <!-- 主页和目标管理 -->
          <div class="space-y-3 mb-5">
            <router-link to="/" class="block" @click="navigateAndCloseMenu">
              <div class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white">
                <span class="flex items-center">
                  <i class="ri-home-line mr-3 text-blue-400 text-xl"></i>
                  <span class="text-lg">主页</span>
                </span>
                <i class="ri-arrow-right-s-line"></i>
              </div>
            </router-link>
            
            <router-link to="/target-management" class="block" @click="navigateAndCloseMenu">
              <div class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white">
                <span class="flex items-center">
                  <i class="ri-focus-3-line mr-3 text-blue-400 text-xl"></i>
                  <span class="text-lg">目标管理</span>
                </span>
                <i class="ri-arrow-right-s-line"></i>
              </div>
            </router-link>
          </div>
          
          <!-- 攻击面搜集 -->
          <div class="mb-5">
            <div class="mb-2 border-b border-gray-700 pb-1">
              <h3 class="text-sm text-gray-400 px-2">攻击面搜集</h3>
            </div>
            <div class="space-y-3">
              <router-link to="/subdomain-scan-results" class="block" @click="navigateAndCloseMenu">
                <div class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white">
                  <span class="flex items-center">
                    <i class="ri-global-line mr-3 text-blue-400 text-xl"></i>
                    <span class="text-lg">子域名发现</span>
                  </span>
                  <i class="ri-arrow-right-s-line"></i>
                </div>
              </router-link>
              
              <router-link to="/port-scan-results" class="block" @click="navigateAndCloseMenu">
                <div class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white">
                  <span class="flex items-center">
                    <i class="ri-scan-2-line mr-3 text-blue-400 text-xl"></i>
                    <span class="text-lg">端口扫描</span>
                  </span>
                  <i class="ri-arrow-right-s-line"></i>
                </div>
              </router-link>
              
              <router-link to="/web-main" class="block" @click="navigateAndCloseMenu">
                <div class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white">
                  <span class="flex items-center">
                    <i class="ri-earth-line mr-3 text-blue-400 text-xl"></i>
                    <span class="text-lg">空间测试</span>
                  </span>
                  <i class="ri-arrow-right-s-line"></i>
                </div>
              </router-link>
            </div>
          </div>
          
          <!-- 攻击面刻画 -->
          <div class="mb-5">
            <div class="mb-2 border-b border-gray-700 pb-1">
              <h3 class="text-sm text-gray-400 px-2">攻击面刻画</h3>
            </div>
            <div class="space-y-3">
              <router-link to="/path-scan-results" class="block" @click="navigateAndCloseMenu">
                <div class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white">
                  <span class="flex items-center">
                    <i class="ri-folders-line mr-3 text-blue-400 text-xl"></i>
                    <span class="text-lg">路径扫描</span>
                  </span>
                  <i class="ri-arrow-right-s-line"></i>
                </div>
              </router-link>
              
              <router-link to="/under-development" class="block" @click="navigateAndCloseMenu">
                <div class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white">
                  <span class="flex items-center">
                    <i class="ri-fingerprint-line mr-3 text-blue-400 text-xl"></i>
                    <span class="text-lg">指纹识别</span>
                  </span>
                  <i class="ri-arrow-right-s-line"></i>
                </div>
              </router-link>
            </div>
          </div>
          
          <!-- 攻击面渗透 -->
          <div class="mb-5">
            <div class="mb-2 border-b border-gray-700 pb-1">
              <h3 class="text-sm text-gray-400 px-2">攻击面渗透</h3>
            </div>
            <div class="space-y-3">
              <router-link to="/under-development" class="block" @click="navigateAndCloseMenu">
                <div class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white">
                  <span class="flex items-center">
                    <i class="ri-bug-line mr-3 text-blue-400 text-xl"></i>
                    <span class="text-lg">漏洞扫描</span>
                  </span>
                  <i class="ri-arrow-right-s-line"></i>
                </div>
              </router-link>
              
              <router-link to="/under-development" class="block" @click="navigateAndCloseMenu">
                <div class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white">
                  <span class="flex items-center">
                    <i class="ri-error-warning-line mr-3 text-blue-400 text-xl"></i>
                    <span class="text-lg">漏洞利用</span>
                  </span>
                  <i class="ri-arrow-right-s-line"></i>
                </div>
              </router-link>
            </div>
          </div>
          
          <!-- 工具箱 -->
          <div class="mb-5">
            <div class="mb-2 border-b border-gray-700 pb-1">
              <h3 class="text-sm text-gray-400 px-2">工具箱</h3>
            </div>
            <div class="space-y-3">
              <button
                @click="toggleCryptoToolsAndCloseMenus"
                class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white w-full"
              >
                <span class="flex items-center">
                  <i class="ri-lock-line mr-3 text-blue-400 text-xl"></i>
                  <span class="text-lg">加密解密工具箱</span>
                </span>
                <i class="ri-arrow-right-s-line"></i>
              </button>
              
              <button
                @click="toggleNetworkToolsAndCloseMenus"
                class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white w-full"
              >
                <span class="flex items-center">
                  <i class="ri-cloud-line mr-3 text-blue-400 text-xl"></i>
                  <span class="text-lg">网络请求工具箱</span>
                </span>
                <i class="ri-arrow-right-s-line"></i>
              </button>
            </div>
          </div>
          
          <!-- 系统功能 -->
          <div class="mb-5">
            <div class="mb-2 border-b border-gray-700 pb-1">
              <h3 class="text-sm text-gray-400 px-2">系统功能</h3>
            </div>
            <div class="space-y-3">
              <template v-for="(route, index) in otherRoutes" :key="index">
                <router-link :to="route.path" class="block" @click="navigateAndCloseMenu">
                  <div class="flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white">
                    <span class="flex items-center">
                      <i :class="route.icon + ' mr-3 text-blue-400 text-xl'"></i>
                      <span class="text-lg">{{ route.name }}</span>
                    </span>
                    <i class="ri-arrow-right-s-line"></i>
                  </div>
                </router-link>
              </template>
            </div>
          </div>

          <!-- 登出按钮 -->
          <div class="mt-8">
            <button
              @click="handleLogout"
              class="w-full flex items-center justify-between p-3 rounded-lg bg-red-900/30 text-white"
            >
              <span class="flex items-center">
                <i class="ri-logout-box-line mr-3 text-red-400 text-xl"></i>
                <span class="text-lg">登出</span>
              </span>
              <i class="ri-arrow-right-s-line"></i>
            </button>
          </div>

          <!-- 添加移动端关闭按钮 -->
          <div class="mt-4">
            <button
              @click="closeNavigation"
              class="w-full flex items-center justify-between p-3 rounded-lg bg-gray-800 text-white"
            >
              <span class="flex items-center">
                <i class="ri-close-circle-line mr-3 text-blue-400 text-xl"></i>
                <span class="text-lg">关闭导航</span>
              </span>
              <i class="ri-close-line"></i>
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- 通知组件 -->
    <PopupNotification
      v-if="showNotification"
      :message="notificationMessage"
      :emoji="notificationEmoji"
      :type="notificationType"
      @close="showNotification = false"
    />
  </div>
</template>

<script>
import PopupNotification from "./Utils/PopupNotification.vue";
import { mapActions, mapState } from "vuex";

export default {
  name: "HeaderPage",
  components: {
    PopupNotification,
  },
  data() {
    return {
      mobileMenuOpen: false,
      showNotification: false,
      notificationMessage: "",
      notificationEmoji: "",
      notificationType: "success",
      dropdowns: {
      collection: false,
      characterization: false,
      penetration: false,
        toolbox: false,
        features: false,
      },
      otherRoutes: [
        { path: "/task-management", name: "任务管理", icon: "ri-task-line" },
        { path: "/system-configuration", name: "系统配置", icon: "ri-settings-3-line" },
        { path: "/user-management", name: "用户管理", icon: "ri-user-settings-line" },
        { path: "/under-development", name: "综合扫描", icon: "ri-rocket-line" },
      ]
    };
  },
  computed: {
    ...mapState({
      isAuthenticated: state => state.isAuthenticated
    })
  },
  methods: {
    ...mapActions(['toggleCryptoTools', 'toggleNetworkTools', 'logout']),
    
    toggleMobileMenu() {
      this.mobileMenuOpen = !this.mobileMenuOpen;
      console.log('移动菜单状态:', this.mobileMenuOpen);
      
      // 禁止背景滚动
      if (this.mobileMenuOpen) {
        document.body.style.overflow = 'hidden';
      } else {
        document.body.style.overflow = '';
      }
    },
    
    navigateAndCloseMenu() {
      // 关闭菜单
      this.mobileMenuOpen = false;
      document.body.style.overflow = '';
    },
    
    toggleDropdown(menu) {
      // 防止事件冒泡
      event?.stopPropagation();

      // 关闭其他下拉菜单
      Object.keys(this.dropdowns).forEach((key) => {
        if (key !== menu) {
          this.dropdowns[key] = false;
        }
      });
      
      // 切换当前菜单
      this.dropdowns[menu] = !this.dropdowns[menu];
    },
    
    closeAllDropdowns() {
      Object.keys(this.dropdowns).forEach((key) => {
        this.dropdowns[key] = false;
      });
    },
    
    async handleLogout() {
      // 使用映射的action而不是尝试直接访问store
      await this.logout();
      
      this.notificationMessage = "登出成功！期待您的再次访问！";
      this.notificationEmoji = "👋";
      this.notificationType = "success";
      this.showNotification = true;
      this.mobileMenuOpen = false;
      document.body.style.overflow = '';

      setTimeout(() => {
        // 使用this.$router而不是创建新的router实例
        this.$router.push({ name: "Home" });
      }, 1500);
    },

    handleClickOutside(e) {
      if (e.target.closest("button")) return;
      if (!e.target.closest(".relative.group")) {
        this.closeAllDropdowns();
      }
    },
    
    toggleCryptoToolsAndCloseMenus() {
      // 调用Vuex action来切换加密工具可见性
      this.toggleCryptoTools();
      // 关闭下拉菜单
      this.closeAllDropdowns();
      // 如果是移动端，关闭移动菜单
      if (this.mobileMenuOpen) {
        this.navigateAndCloseMenu();
      }
    },
    
    toggleNetworkToolsAndCloseMenus() {
      // 调用Vuex action来切换网络工具可见性
      this.toggleNetworkTools();
      // 关闭下拉菜单
      this.closeAllDropdowns();
      // 如果是移动端，关闭移动菜单
      if (this.mobileMenuOpen) {
        this.navigateAndCloseMenu();
      }
    },

    closeNavigation() {
      this.mobileMenuOpen = false;
      document.body.style.overflow = '';
    }
  },
  mounted() {
    document.addEventListener("click", this.handleClickOutside);
    
    // 添加ESC键监听
    document.addEventListener('keydown', (e) => {
      if (e.key === 'Escape' && this.mobileMenuOpen) {
        this.mobileMenuOpen = false;
        document.body.style.overflow = '';
      }
    });
  },
  beforeUnmount() {
    document.removeEventListener("click", this.handleClickOutside);
    document.body.style.overflow = '';
  }
};
</script>

<style scoped>
.dropdown-enter-active,
.dropdown-leave-active {
  transition: opacity 0.2s, transform 0.2s;
}

.dropdown-enter-from,
.dropdown-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}

/* 添加滚动条样式 */
::-webkit-scrollbar {
  width: 6px;
  height: 6px;
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

/* 移动菜单动画和样式 */
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideUp {
  from { 
    opacity: 0;
    transform: translateY(10px);
  }
  to { 
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes dropdownAppear {
  from {
    opacity: 0;
    transform: translateY(-5px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.animate-dropdown {
  animation: dropdownAppear 0.2s ease-out forwards;
}

.bg-gray-800 {
  transition: background-color 0.2s;
}

.bg-gray-800:active {
  background-color: rgba(75, 85, 99, 0.7);
}

router-link.block {
  animation: slideUp 0.3s ease-out;
  animation-fill-mode: both;
}

router-link.block:nth-child(2) {
  animation-delay: 0.05s;
}

router-link.block:nth-child(3) {
  animation-delay: 0.1s;
}

router-link.block:nth-child(4) {
  animation-delay: 0.15s;
}

router-link.block:nth-child(5) {
  animation-delay: 0.2s;
}

/* 增强导航效果 */
nav {
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.5);
  border-bottom: 1px solid rgba(59, 130, 246, 0.1);
}

/* 发光效果 */
@keyframes glow {
  0% {
    box-shadow: 0 0 5px rgba(59, 130, 246, 0.3);
  }
  50% {
    box-shadow: 0 0 15px rgba(59, 130, 246, 0.5);
  }
  100% {
    box-shadow: 0 0 5px rgba(59, 130, 246, 0.3);
  }
}

/* 菜单项悬停动画 */
.hover\:bg-gray-700\/50:hover {
  background-color: rgba(55, 65, 81, 0.7);
  animation: glow 1.5s infinite;
}

/* 下拉菜单中的项目缓慢增强 */
@keyframes menu-item-appear {
  from {
    opacity: 0;
    transform: translateX(-5px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

.block.w-full {
  animation: menu-item-appear 0.3s ease-out forwards;
}

.block.w-full:nth-child(2) {
  animation-delay: 0.05s;
}

.block.w-full:nth-child(3) {
  animation-delay: 0.1s;
}

/* 增强Logo和标题效果 */
.logo-text-glow {
  animation: textGlow 3s ease-in-out infinite;
  text-shadow: 0 0 5px rgba(96, 165, 250, 0.3);
  letter-spacing: 0.5px;
}

@keyframes textGlow {
  0%, 100% {
    text-shadow: 0 0 5px rgba(96, 165, 250, 0.3);
  }
  50% {
    text-shadow: 0 0 15px rgba(96, 165, 250, 0.7), 0 0 20px rgba(129, 140, 248, 0.5);
  }
}

/* 提高导航栏标题的可视性 */
.nav-title {
  position: relative;
  text-shadow: 0 0 1px rgba(255, 255, 255, 0.1), 
              0 1px 2px rgba(0, 0, 0, 0.3);
  transform-style: preserve-3d;
  perspective: 500px;
}

.nav-title::after {
  content: '';
  position: absolute;
  bottom: -3px;
  left: 0;
  width: 100%;
  height: 2px;
  background: linear-gradient(90deg, transparent, rgba(96, 165, 250, 0.6), transparent);
  transform: scaleX(0);
  transform-origin: center;
  transition: transform 0.3s ease;
}

.nav-title:hover::after {
  transform: scaleX(1);
}

/* 3D效果 */
.nav-title:hover {
  animation: tiltEffect 0.5s ease forwards;
}

@keyframes tiltEffect {
  0% { transform: perspective(500px) rotateX(0deg); }
  25% { transform: perspective(500px) rotateX(2deg); }
  50% { transform: perspective(500px) rotateX(-1deg); }
  75% { transform: perspective(500px) rotateX(1deg); }
  100% { transform: perspective(500px) rotateX(0deg); }
}

/* 背景闪光效果 */
.nav-title-container {
  position: relative;
  overflow: hidden;
  border-radius: 4px;
}

.nav-title-container::before {
  content: '';
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: radial-gradient(circle, rgba(255, 255, 255, 0.1) 0%, rgba(255, 255, 255, 0) 70%);
  opacity: 0;
  transition: opacity 0.5s;
}

.nav-title-container:hover::before {
  animation: circleMove 1.5s ease-in-out;
  opacity: 1;
}

@keyframes circleMove {
  0% { transform: translate(-30%, -30%) scale(0); opacity: 0; }
  50% { opacity: 0.5; }
  100% { transform: translate(0%, 0%) scale(1); opacity: 0; }
}
</style>
