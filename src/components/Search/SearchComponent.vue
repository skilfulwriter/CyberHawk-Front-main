<template>
  <div class="search-section mb-8 sm:mb-12">
    <!-- 搜索框 -->
    <div
      class="search-container relative w-full max-w-3xl mx-auto bg-gray-800/80 backdrop-blur-lg rounded-xl sm:rounded-2xl p-4 sm:p-6 shadow-lg border border-gray-700/30"
    >
      <div class="search-box flex items-center gap-2 sm:gap-3">
        <input
          id="target"
          v-model="searchQuery"
          type="text"
          placeholder="请输入搜索关键词..."
          class="flex-1 bg-gray-900/90 border-2 border-cyan-500/70 rounded-lg text-cyan-400 py-2 sm:py-3 px-3 sm:px-4 text-base sm:text-lg transition-all focus:outline-none focus:border-cyan-400 focus:shadow-glow"
          @keyup.enter="startSearch"
        />
        <div class="search-icons flex gap-1 sm:gap-2">
          <button
            class="search-icon bg-transparent border-none text-cyan-400 cursor-pointer text-lg sm:text-xl p-1.5 sm:p-2 transition-all hover:text-white hover:scale-110"
            @click="toggleVoiceSearch"
            title="语音搜索"
          >
            <i class="ri-mic-line"></i>
          </button>
          <button
            class="search-icon bg-transparent border-none text-cyan-400 cursor-pointer text-lg sm:text-xl p-1.5 sm:p-2 transition-all hover:text-white hover:scale-110"
            @click="toggleImageSearch"
            title="图片搜索"
          >
            <i class="ri-camera-line"></i>
          </button>
          <button
            class="search-icon bg-cyan-500/50 text-white rounded-lg cursor-pointer text-lg sm:text-xl p-1.5 sm:p-2 transition-all hover:bg-cyan-500"
            @click="startSearch"
            title="开始搜索"
          >
            <i class="ri-search-line"></i>
          </button>
        </div>
      </div>
      <div v-if="searchTypeIndicator" class="search-type-indicator absolute left-4 top-0 -translate-y-6 text-cyan-400 text-sm opacity-80">
        {{ searchTypeIndicator }}
      </div>
    </div>

    <!-- 标签页 -->
    <div class="tabs flex flex-wrap justify-center gap-2 my-6 sm:my-8 px-2">
      <button
        v-for="(tab, index) in tabs"
        :key="index"
        class="tab px-3 sm:px-4 py-1.5 sm:py-2 bg-gray-800/80 border-none rounded-lg text-sm sm:text-base text-cyan-400 cursor-pointer transition-all hover:bg-gray-700/80"
        :class="{ 'active bg-cyan-500 !text-gray-900': activeTab === tab.id }"
        @click="switchTab(tab.id)"
      >
        {{ tab.name }}
      </button>
    </div>

    <!-- 热门标签 -->
    <div class="hot-tags flex flex-wrap gap-2 justify-center my-4 sm:my-6 px-2">
      <span
        v-for="(tag, index) in hotTags"
        :key="index"
        class="hot-tag px-3 sm:px-4 py-1 sm:py-1.5 bg-cyan-500/10 border border-cyan-500/70 rounded-full text-sm text-cyan-400 cursor-pointer transition-all hover:bg-cyan-500 hover:text-gray-900 mb-1"
        @click="selectTag(tag)"
      >
        {{ tag }}
      </span>
    </div>

    <!-- 搜索卡片区域 -->
    <div 
      v-for="(searchCard, cardIndex) in searchCards" 
      :key="cardIndex"
      class="search-card-container" 
      :style="{ display: activeTab === searchCard.tabId ? 'block' : 'none' }"
    >
      <div class="search-card bg-gray-800/80 backdrop-blur-lg rounded-xl p-4 sm:p-6 shadow-lg border border-gray-700/30 mb-6 animate-fade-in">
        <h3 class="text-cyan-400 text-lg sm:text-xl font-medium mb-3 sm:mb-4 pb-2 sm:pb-3 border-b border-cyan-500/30">
          {{ searchCard.title }}
        </h3>
        <div class="card-buttons grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 gap-2 sm:gap-3">
          <button
            v-for="(btn, btnIndex) in searchCard.buttons"
            :key="btnIndex"
            class="search-btn bg-cyan-500/10 border border-cyan-500/70 rounded-lg text-cyan-400 py-2 px-2 sm:px-3 cursor-pointer transition-all flex items-center justify-center gap-1 sm:gap-2 hover:bg-cyan-500 hover:text-gray-900 text-sm sm:text-base"
            @click="executeSearch(btn.action)"
          >
            <i :class="btn.icon"></i>
            <span class="button-text truncate">{{ btn.label }}</span>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "SearchComponent",
  data() {
    return {
      searchQuery: "",
      searchTypeIndicator: "",
      currentSearchMode: "text",
      activeTab: "basic",
      tabs: [
        { id: "basic", name: "基础搜索" },
        { id: "asset", name: "资产测绘" },
        { id: "company", name: "企业查询" },
        { id: "domain", name: "IP/域名信息" },
        { id: "hack", name: "GoogleHack" },
      ],
      hotTags: [],
      defaultHotTags: ["deespeek教程", "AI变现指南", "漏洞挖掘", "渗透测试", "社会工程学", "资产收集"],
      searchHistory: [],
      searchCards: [
        {
          tabId: "basic",
          title: "基础搜索",
          buttons: [
            { label: "必应搜索", icon: "ri-windows-line", action: "bt_bing_search" },
            { label: "百度搜索", icon: "ri-search-line", action: "bt_baidu_search" },
            { label: "谷歌搜索", icon: "ri-google-line", action: "bt_google_search" },
            { label: "360搜索", icon: "ri-compass-line", action: "bt_360_search" },
            { label: "搜狗搜索", icon: "ri-search-eye-line", action: "bt_sogou_search" },
            { label: "CSDN搜索", icon: "ri-code-line", action: "bt_csdn_search" },
            { label: "GitHub搜索", icon: "ri-github-line", action: "bt_github_search" },
            { label: "Gitee搜索", icon: "ri-git-branch-line", action: "bt_gitee_search" },
            { label: "百度搜索域名资产", icon: "ri-search-line", action: "bt_baidu" },
            { label: "Google搜索域名资产", icon: "ri-search-line", action: "bt_google" },
            { label: "Google搜漏洞关键字", icon: "ri-bug-line", action: "bt_google_vul" },
            { label: "BaiDu搜漏洞关键字", icon: "ri-bug-line", action: "bt_baidu_vul" },
            { label: "Google社会工程学", icon: "ri-user-search-line", action: "bt_google_info" },
            { label: "BaiDu社会工程学", icon: "ri-user-search-line", action: "bt_baidu_info" },
          ]
        },
        {
          tabId: "asset",
          title: "资产测绘",
          buttons: [
            { label: "FoFa搜索", icon: "ri-global-line", action: "bt_fofa" },
            { label: "ZoomEye搜索", icon: "ri-eye-line", action: "bt_zoomeye" },
            { label: "Hunter搜索", icon: "ri-search-line", action: "bt_hunter" },
            { label: "Censys搜索", icon: "ri-server-line", action: "bt_censys" },
            { label: "360quake搜索", icon: "ri-flashlight-line", action: "bt_quake" },
          ]
        },
        {
          tabId: "company",
          title: "企业查询",
          buttons: [
            { label: "天眼查", icon: "ri-building-line", action: "bt_tianyancha" },
            { label: "爱企查", icon: "ri-search-line", action: "bt_aiqicha" },
            { label: "企查查", icon: "ri-bank-line", action: "bt_qichacha" },
          ]
        },
        {
          tabId: "domain",
          title: "IP/域名信息",
          buttons: [
            { label: "站长之家Ping", icon: "ri-server-line", action: "bt_ping" },
            { label: "国际测速", icon: "ri-global-line", action: "bt_alltest" },
            { label: "ip138查询", icon: "ri-search-line", action: "bt_ip138" },
            { label: "ICP备案", icon: "ri-file-list-line", action: "bt_icp" },
            { label: "WhoIS查询", icon: "ri-information-line", action: "bt_whois" },
            { label: "子域名查询", icon: "ri-sitemap-line", action: "bt_domain" },
            { label: "HTTP状态查询", icon: "ri-signal-tower-line", action: "bt_http_status" },
          ]
        },
        {
          tabId: "hack",
          title: "GoogleHack工具",
          buttons: [
            { label: "后台页面", icon: "ri-shield-keyhole-line", action: "google_it_1" },
            { label: "后台入口", icon: "ri-door-open-line", action: "google_it_2" },
            { label: "登录页面", icon: "ri-key-line", action: "google_it_3" },
            { label: "上传点", icon: "ri-upload-line", action: "google_it_4" },
            { label: "目录查找", icon: "ri-folder-line", action: "google_it_5" },
            { label: "注入点", icon: "ri-code-line", action: "google_it_6" },
            { label: "SQL报错", icon: "ri-bug-line", action: "google_it_7" },
            { label: "phpinfo", icon: "ri-information-line", action: "google_it_8" },
            { label: "社工信息", icon: "ri-user-search-line", action: "google_it_9" },
            { label: "文件查找", icon: "ri-file-search-line", action: "google_it_10" },
            { label: "后门文件", icon: "ri-virus-line", action: "google_it_11" },
            { label: "未初始化页面", icon: "ri-tools-line", action: "google_it_12" },
            { label: "重定向页面", icon: "ri-arrow-left-right-line", action: "google_it_13" },
            { label: "Struts RCE", icon: "ri-error-warning-line", action: "google_it_14" },
            { label: "MDB数据库", icon: "ri-database-line", action: "google_it_15" },
            { label: "WordPress配置", icon: "ri-wordpress-line", action: "google_it_16" },
            { label: "htaccess泄露", icon: "ri-file-code-line", action: "google_it_17" },
            { label: "子域名", icon: "ri-sitemap-line", action: "google_it_18" },
            { label: "三级域名", icon: "ri-server-line", action: "google_it_19" },
          ]
        },
      ]
    };
  },
  mounted() {
    // 初始化搜索历史
    this.loadSearchHistory();
    this.updateHotTags();
  },
  methods: {
    // 切换标签页
    switchTab(tabId) {
      this.activeTab = tabId;
    },
    
    // 加载搜索历史
    loadSearchHistory() {
      const savedHistory = localStorage.getItem('search_history');
      if (savedHistory) {
        this.searchHistory = JSON.parse(savedHistory);
      }
    },
    
    // 更新热门标签
    updateHotTags() {
      const now = Date.now();
      const UPDATE_INTERVAL = 24 * 60 * 60 * 1000; // 24小时
      const recentPeriod = now - UPDATE_INTERVAL;
      const MAX_HOT_TAGS = 6;
      
      // 筛选近期的搜索记录并按频率排序
      const hotTags = this.searchHistory
        .filter(item => item.lastSearch > recentPeriod)
        .sort((a, b) => {
          // 首先按搜索次数排序
          if (b.count !== a.count) {
            return b.count - a.count;
          }
          // 次数相同时，按最后搜索时间排序
          return b.lastSearch - a.lastSearch;
        })
        .slice(0, MAX_HOT_TAGS)
        .map(item => item.keyword);
      
      if (hotTags.length === 0) {
        this.hotTags = [...this.defaultHotTags];
      } else {
        this.hotTags = hotTags;
      }
    },
    
    // 记录搜索词
    recordSearch(keyword) {
      const now = Date.now();
      const MAX_HISTORY_ITEMS = 100; // 最大历史记录数
      const existingIndex = this.searchHistory.findIndex(item => item.keyword === keyword);
      
      if (existingIndex !== -1) {
        // 更新已存在的搜索词
        this.searchHistory[existingIndex].count++;
        this.searchHistory[existingIndex].lastSearch = now;
      } else {
        // 添加新的搜索词
        this.searchHistory.push({
          keyword,
          count: 1,
          firstSearch: now,
          lastSearch: now
        });
      }
      
      // 如果超过最大记录数，删除最旧的记录
      if (this.searchHistory.length > MAX_HISTORY_ITEMS) {
        this.searchHistory.sort((a, b) => b.lastSearch - a.lastSearch);
        this.searchHistory = this.searchHistory.slice(0, MAX_HISTORY_ITEMS);
      }
      
      // 保存到本地存储
      localStorage.setItem('search_history', JSON.stringify(this.searchHistory));
      
      // 更新热门标签显示
      this.updateHotTags();
    },
    
    // 选择标签
    selectTag(tag) {
      this.searchQuery = tag;
      this.startSearch();
    },
    
    // 开始搜索
    startSearch() {
      if (this.checkInput()) {
        this.recordSearch(this.searchQuery);
        this.executeSearch('bt_bing_search');
      }
    },
    
    // 检查输入
    checkInput() {
      if (!this.searchQuery) {
        alert("搜索框不能为空");
        return false;
      }
      return true;
    },
    
    // 执行搜索
    executeSearch(action) {
      if (!this.checkInput()) return;
      
      // 记录搜索历史
      this.recordSearch(this.searchQuery);
      
      // Google Hack 工具特殊处理
      if (action.startsWith('google_it_')) {
        const type = parseInt(action.split('_')[2]);
        this.googleHack(type);
        return;
      }
      
      // 执行对应的搜索方法
      switch(action) {
        // 基础搜索
        case 'bt_bing_search':
          window.open(`https://www.bing.com/search?q=${this.searchQuery}`);
          break;
        case 'bt_baidu_search':
          window.open(`https://www.baidu.com/s?wd=${this.searchQuery}`);
          break;
        case 'bt_google_search':
          window.open(`https://www.google.com/search?q=${this.searchQuery}`);
          break;
        case 'bt_360_search':
          window.open(`https://www.so.com/s?q=${this.searchQuery}`);
          break;
        case 'bt_sogou_search':
          window.open(`https://www.sogou.com/web?query=${this.searchQuery}`);
          break;
        case 'bt_csdn_search':
          window.open(`https://so.csdn.net/so/search?q=${this.searchQuery}`);
          break;
        case 'bt_github_search':
          window.open(`https://github.com/search?q=${this.searchQuery}`);
          break;
        case 'bt_gitee_search':
          window.open(`https://search.gitee.com/?skin=rec&type=repository&q=${this.searchQuery}`);
          break;
        case 'bt_baidu':
          this.baiduDomainSearch();
          break;
        case 'bt_google':
          this.googleDomainSearch();
          break;
        case 'bt_google_vul':
          window.open(`https://www.google.com/search?q=${this.searchQuery}+"漏洞"+"复现"+"CVE"+"CNVD"+"安全"`);
          break;
        case 'bt_baidu_vul':
          window.open(`https://www.baidu.com/s?wd=${this.searchQuery}+"漏洞"+"复现"+"CVE"+"CNVD"+"安全"`);
          break;
        case 'bt_google_info':
          this.googleSocialEngSearch();
          break;
        case 'bt_baidu_info':
          this.baiduSocialEngSearch();
          break;
          
        // 资产测绘
        case 'bt_fofa':
          this.fofaSearch();
          break;
        case 'bt_zoomeye':
          this.zoomeyeSearch();
          break;
        case 'bt_hunter':
          this.hunterSearch();
          break;
        case 'bt_censys':
          this.censysSearch();
          break;
        case 'bt_quake':
          this.quakeSearch();
          break;
          
        // 企业查询
        case 'bt_tianyancha':
          window.open(`https://www.tianyancha.com/nsearch?key=${this.searchQuery}`);
          break;
        case 'bt_aiqicha':
          window.open(`https://aiqicha.baidu.com/login?u=https://aiqicha.baidu.com/s?q=${this.searchQuery}&t=0`);
          break;
        case 'bt_qichacha':
          window.open(`https://www.qcc.com/web/search?key=${this.searchQuery}`);
          break;
          
        // IP/域名信息
        case 'bt_ping':
          this.pingDomain();
          break;
        case 'bt_alltest':
          this.speedTestDomain();
          break;
        case 'bt_ip138':
          this.ip138Search();
          break;
        case 'bt_icp':
          this.icpSearch();
          break;
        case 'bt_whois':
          this.whoisSearch();
          break;
        case 'bt_domain':
          this.domainSearch();
          break;
        case 'bt_http_status':
          window.open(`https://tool.chinaz.com/pagestatus/default.aspx?url=${this.searchQuery}`);
          break;
      }
    },
    
    // 语音搜索
    toggleVoiceSearch() {
      if ('webkitSpeechRecognition' in window) {
        const recognition = new window.webkitSpeechRecognition();
        recognition.lang = 'zh-CN';
        recognition.continuous = false;
        recognition.interimResults = false;
        
        recognition.onstart = () => {
          this.searchTypeIndicator = '正在听...';
        };
        
        recognition.onresult = (event) => {
          const transcript = event.results[0][0].transcript;
          this.searchQuery = transcript;
          this.searchTypeIndicator = '语音识别完成';
          setTimeout(() => { this.searchTypeIndicator = ''; }, 3000);
        };
        
        recognition.onerror = () => {
          this.searchTypeIndicator = '语音识别失败';
          setTimeout(() => { this.searchTypeIndicator = ''; }, 3000);
        };
        
        recognition.start();
      } else {
        alert('您的浏览器不支持语音识别功能');
      }
    },
    
    // 图片搜索
    toggleImageSearch() {
      const input = document.createElement('input');
      input.type = 'file';
      input.accept = 'image/*';
      
      input.onchange = async (e) => {
        const file = e.target.files[0];
        if (file) {
          // 创建搜索选项对话框
          const dialog = document.createElement('div');
          dialog.style.cssText = `
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(22, 33, 62, 0.95);
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0, 230, 230, 0.3);
            z-index: 1000;
            backdrop-filter: blur(5px);
            border: 1px solid #00e6e6;
          `;

          // 创建遮罩层
          const overlay = document.createElement('div');
          overlay.style.cssText = `
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.5);
            z-index: 999;
          `;

          // 添加标题
          const title = document.createElement('h3');
          title.textContent = '选择图片搜索引擎';
          title.style.cssText = `
            color: #00e6e6;
            margin-bottom: 15px;
            text-align: center;
          `;

          // 创建预览图
          const preview = document.createElement('img');
          preview.style.cssText = `
            max-width: 200px;
            max-height: 200px;
            margin: 10px auto;
            display: block;
            border-radius: 5px;
          `;

          // 读取并显示预览图
          const reader = new FileReader();
          reader.onload = (e) => {
            preview.src = e.target.result;
          };
          reader.readAsDataURL(file);

          // 创建按钮容器
          const buttonContainer = document.createElement('div');
          buttonContainer.style.cssText = `
            display: flex;
            gap: 10px;
            justify-content: center;
            margin-top: 15px;
          `;

          // 按钮样式
          const buttonStyle = `
            background: rgba(0, 230, 230, 0.1);
            border: 1px solid #00e6e6;
            border-radius: 5px;
            color: #00e6e6;
            padding: 8px 15px;
            cursor: pointer;
            transition: all 0.3s ease;
          `;

          // 创建搜索引擎按钮
          const engines = [
            { name: '百度识图', url: 'https://graph.baidu.com/pcpage/index?tpl_from=' },
            { name: '谷歌识图', url: 'https://lens.google.com/' },
            { name: '必应识图', url: 'https://www.bing.com/visualsearch' }
          ];

          engines.forEach(engine => {
            const button = document.createElement('button');
            button.textContent = engine.name;
            button.style.cssText = buttonStyle;
            button.onmouseover = () => {
              button.style.background = '#00e6e6';
              button.style.color = '#1a1a2e';
            };
            button.onmouseout = () => {
              button.style.background = 'rgba(0, 230, 230, 0.1)';
              button.style.color = '#00e6e6';
            };
            button.onclick = () => {
              window.open(engine.url);
              document.body.removeChild(overlay);
              document.body.removeChild(dialog);
            };
            buttonContainer.appendChild(button);
          });

          // 创建关闭按钮
          const closeButton = document.createElement('button');
          closeButton.textContent = '关闭';
          closeButton.style.cssText = buttonStyle;
          closeButton.onmouseover = () => {
            closeButton.style.background = '#00e6e6';
            closeButton.style.color = '#1a1a2e';
          };
          closeButton.onmouseout = () => {
            closeButton.style.background = 'rgba(0, 230, 230, 0.1)';
            closeButton.style.color = '#00e6e6';
          };
          closeButton.onclick = () => {
            document.body.removeChild(overlay);
            document.body.removeChild(dialog);
          };

          // 组装对话框
          dialog.appendChild(title);
          dialog.appendChild(preview);
          dialog.appendChild(buttonContainer);
          dialog.appendChild(document.createElement('br'));
          dialog.appendChild(closeButton);

          // 添加到页面
          document.body.appendChild(overlay);
          document.body.appendChild(dialog);
        }
      };
      
      input.click();
    },
    
    // Google Hack 工具函数
    googleHack(type) {
      let site = this.searchQuery;
      
      if (site.indexOf('://') > 0) {
        site = site.substr(site.indexOf('://') + 3);
      }
      
      let url = 'https://www.google.com/search?q=';
      url += 'site:' + site;
  
      switch(type) {
        case 1:
          url += ' intext:管理 | 后台 | 后台管理 | 登陆 | 登录 | 用户名 | 密码 | 系统 | 账号 | login | system';
          break;
        case 2:
          url += ' inurl:login | inurl:admin | inurl:manage | inurl:manager | inurl:admin_login | inurl:system | inurl:backend';
          break;
        case 3:
          url += ' intitle:管理 | 后台 | 后台管理 | 登陆 | 登录';
          break;
        case 4:
          url += ' inurl:file | upload';
          break;
        case 5:
          url += ' intitle:index.of';
          break;
        case 6:
          url += ' inurl:php?id= | inurl:asp?id= | inurl:aspx?id= | inurl:php?page= | inurl:asp?page= | inurl:aspx?page=';
          break;
        case 7:
          url += ' intext:"sql syntax near" | intext:"syntax error has occurred" | intext:"incorrect syntax near" | intext:"unexpected end of SQL command" | intext:"Warning: mysql_connect()" | intext:"Warning: mysql_query()" | intext:"Warning: pg_connect()"';
          break;
        case 8:
          url += ' ext:php intitle:phpinfo "published by the PHP Group"';
          break;
        case 9:
          url += ' intitle:账号 | 密码 | 工号 | 学号 | 身份证';
          break;
        case 10:
          url += ' ext:doc+|+ext:docx+|+ext:odt+|+ext:pdf+|+ext:rtf+|+ext:sxw+|+ext:psw+|+ext:ppt+|+ext:pptx+|+ext:pps+|+ext:csv';
          break;
        case 11:
          url += ' inurl:shell+|+inurl:backdoor+|+inurl:wso+|+inurl:cmd+|+shadow+|+passwd+|+boot.ini+|+inurl:backdoor';
          break;
        case 12:
          url += ' inurl:readme+|+inurl:license+|+inurl:install+|+inurl:setup+|+inurl:config';
          break;
        case 13:
          url += ' inurl:redir+|+inurl:url+|+inurl:redirect+|+inurl:return+|+inurl:src=http+|+inurl:r=http';
          break;
        case 14:
          url += ' ext:action+|+ext:struts+|+ext:do';
          break;
        case 15:
          url = 'https://www.google.com/search?q=site:pastebin.com+' + site;
          break;
        case 16:
          url += ' inurl:"/wp-admin/setup-config.php" intitle:"Setup Configuration File"';
          break;
        case 17:
          url += ' inurl:"/phpinfo.php"+|+inurl:".htaccess"+|+inurl:"/.git"+' + site + ' -github';
          break;
        case 18:
          url = 'https://www.google.com/search?q=site:*.' + site;
          break;
        case 19:
          url = 'https://www.google.com/search?q=site:*.*.' + site;
          break;
      }
      
      window.open(url);
    },
    
    // 域名处理辅助函数
    processDomain(url) {
      let target = url.replace(/^https?:\/\//, '');
      target = target.split('/')[0];
      
      const parts = target.split('.');
      // 列出常见的多段后缀
      const multiSuffixes = ["com.cn", "net.cn", "gov.cn", "org.cn", "edu.cn"];
      
      // 确定是否有匹配的特殊多段后缀
      let domain;
      if (parts.length > 2 && multiSuffixes.some(suffix => target.endsWith(suffix))) {
        // 对于特殊后缀，取最后三部分
        domain = parts.slice(-3).join('.');
      } else {
        // 否则，取最后两部分
        domain = parts.slice(-2).join('.');
      }
      
      return { parts, domain };
    },
    
    // 下面是各种专门的搜索功能
    baiduDomainSearch() {
      const target = this.searchQuery.replace(/^https?:\/\//, '').split('/')[0];
      window.open(`https://www.baidu.com/s?wd="${target}"`);
    },
    
    googleDomainSearch() {
      const target = this.searchQuery.replace(/^https?:\/\//, '').split('/')[0];
      window.open(`https://www.google.com/search?q=site:${target}`);
    },
    
    fofaSearch() {
      const target = this.searchQuery.replace(/^https?:\/\//, '').split('/')[0];
      const parts = target.split('.');
      const domain = parts.length > 1 ? parts.slice(-2).join('.') : target;
      const query = `host="${domain}"`;
      const base64Query = btoa(query);
      window.open(`https://fofa.info/result?qbase64=${base64Query}`);
    },
    
    zoomeyeSearch() {
      const { domain } = this.processDomain(this.searchQuery);
      window.open(`https://www.zoomeye.org/searchResult?q=site:${domain}`);
    },
    
    hunterSearch() {
      const { domain } = this.processDomain(this.searchQuery);
      const query = `domain="${domain}"`;
      const base64Query = btoa(query);
      window.open(`https://hunter.qianxin.com/list?search=${base64Query}&conditions=`);
    },
    
    censysSearch() {
      const { domain } = this.processDomain(this.searchQuery);
      window.open(`https://search.censys.io/search?resource=hosts&sort=RELEVANCE&per_page=25&virtual_hosts=EXCLUDE&q=${domain}`);
    },
    
    quakeSearch() {
      const { domain } = this.processDomain(this.searchQuery);
      window.open(`https://quake.360.net/quake/#/searchResult?searchVal=domain%3D%22${domain}%22&selectIndex=quake_service&latest=true`);
    },
    
    pingDomain() {
      const target = this.searchQuery.replace(/^https?:\/\//, '').split('/')[0];
      window.open(`https://ping.chinaz.com/${target}`);
    },
    
    speedTestDomain() {
      const target = this.searchQuery.replace(/^https?:\/\//, '').split('/')[0];
      window.open(`https://tool.chinaz.com/speedworld/${target}`);
    },
    
    ip138Search() {
      const target = this.searchQuery.replace(/^https?:\/\//, '').split('/')[0];
      window.open(`https://site.ip138.com/${target}/`);
    },
    
    icpSearch() {
      const { domain } = this.processDomain(this.searchQuery);
      window.open(`https://icp.chinaz.com/${domain}`);
    },
    
    whoisSearch() {
      const { domain } = this.processDomain(this.searchQuery);
      window.open(`http://whois.bugscaner.com/${domain}`);
    },
    
    domainSearch() {
      const { domain } = this.processDomain(this.searchQuery);
      window.open(`https://chaziyu.com/${domain}`);
    },
    
    googleSocialEngSearch() {
      const { domain } = this.processDomain(this.searchQuery);
      window.open(`https://www.google.com/search?q=site:${domain}+inbody:账号 | 密码 | 工号 | 学号 | 身份证`);
    },
    
    baiduSocialEngSearch() {
      const { domain } = this.processDomain(this.searchQuery);
      window.open(`https://www.baidu.com/s?wd=账号 密码 工号 学号 身份证 site:${domain}`);
    }
  }
}
</script>

<style scoped>
/* 添加淡入动画 */
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

.search-card-container {
  animation: fadeIn 0.5s ease-out;
}

/* 移动端按钮样式优化 */
@media (max-width: 640px) {
  .card-buttons {
    grid-template-columns: repeat(2, 1fr);
  }
  
  .search-btn {
    height: 40px;
    font-size: 12px;
  }
  
  .button-text {
    max-width: 80px;
  }
  
  .hot-tag {
    margin-bottom: 5px;
  }
  
  .tabs {
    padding: 0 0.5rem;
  }
  
  /* 触摸反馈 */
  .search-btn:active {
    transform: scale(0.97);
  }
  
  .tab:active, .hot-tag:active {
    transform: scale(0.95);
  }
}

/* 搜索框阴影效果 */
.focus\:shadow-glow:focus {
  box-shadow: 0 0 10px rgba(34, 211, 238, 0.4);
}

/* 搜索按钮悬停效果 */
.search-btn:hover i, .search-btn:hover .button-text {
  transform: scale(1.05);
  transition: transform 0.2s;
}
</style>