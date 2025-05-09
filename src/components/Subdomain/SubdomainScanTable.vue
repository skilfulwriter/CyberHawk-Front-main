<template>
  <div class="space-y-6">
    <!-- 表格 -->
    <div v-if="subdomainScanResults?.length > 0">
      <div class="relative overflow-x-auto rounded-xl">
        <table class="w-full">
          <thead>
            <tr class="border-b border-gray-700/50">
              <th class="py-4 px-6 text-left">
                <input
                  type="checkbox"
                  @change="toggleSelectAll"
                  v-model="selectAll"
                  class="rounded border-gray-700/50 bg-gray-900/50 text-blue-500/50 focus:ring-blue-500/30"
                />
              </th>
              <th
                v-for="header in tableHeaders"
                :key="header"
                class="py-4 px-6 text-left text-sm font-medium text-gray-400"
              >
                {{ header }}
              </th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="result in subdomainScanResults"
              :key="result.id"
              class="border-b border-gray-700/30 hover:bg-gray-700/20 transition-colors duration-200"
            >
              <td class="py-4 px-6">
                <input
                  type="checkbox"
                  v-model="selectedResults"
                  :value="result.id"
                  class="rounded border-gray-700/50 bg-gray-900/50 text-blue-500/50 focus:ring-blue-500/30"
                />
              </td>
              <td class="py-4 px-6 text-sm text-gray-200">{{ result.id }}</td>
              <td class="py-4 px-6 text-sm text-gray-200">
                {{ result.target }}
              </td>
              <td class="py-4 px-6 text-sm text-gray-200">
                {{ formatDate(result.timestamp) }}
              </td>
              <td class="py-4 px-6 text-sm text-gray-200">
                {{ getSubdomainCount(result) }} 个
              </td>
              <td class="py-4 px-6">
                <span
                  class="px-2 py-1 rounded-full text-xs font-medium"
                  :class="
                    result.is_read
                      ? 'bg-green-500/20 text-green-300'
                      : 'bg-yellow-500/20 text-yellow-300'
                  "
                >
                  {{ result.is_read ? "已读" : "未读" }}
                </span>
              </td>
              <td class="py-4 px-6">
                <div class="flex space-x-2">
                  <button
                    @click="handleViewDetails(result.id)"
                    class="action-button bg-blue-500/50 text-blue-100 flex items-center justify-center"
                  >
                    <i class="ri-eye-line mr-1"></i>
                    查看
                  </button>
                  <button
                    @click="handleToggleRead(result)"
                    class="action-button flex items-center justify-center"
                    :class="
                      result.is_read
                        ? 'bg-gray-700/50 text-gray-300'
                        : 'bg-green-500/50 text-green-100'
                    "
                  >
                    <i
                      :class="[
                        result.is_read ? 'ri-eye-off-line' : 'ri-eye-line',
                        'mr-1',
                      ]"
                    ></i>
                    {{ result.is_read ? "标为未读" : "标为已读" }}
                  </button>
                  <button
                    @click="handleDelete(result.id)"
                    class="action-button bg-red-500/50 text-red-100 flex items-center justify-center"
                  >
                    <i class="ri-delete-bin-line mr-1"></i>
                    删除
                  </button>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <!-- 所有扫描页面统一使用这个样式 -->
      <div class="flex space-x-3 mt-6">
        <button
          @click="handleBatchRead"
          :disabled="!hasSelected"
          class="batch-button flex items-center justify-center"
          :class="[
            !hasSelected
              ? 'bg-gray-700/50 text-gray-400'
              : 'bg-gray-700/50 hover:bg-gray-600/50 text-gray-200',
          ]"
        >
          <i class="ri-eye-line mr-2"></i>
          标记选中项为已读
        </button>
        <button
          @click="handleBatchDelete"
          :disabled="!hasSelected"
          class="batch-button flex items-center justify-center"
          :class="[
            !hasSelected
              ? 'bg-gray-700/50 text-gray-400'
              : 'bg-red-500/50 hover:bg-red-600/50 text-red-100',
          ]"
        >
          <i class="ri-delete-bin-line mr-2"></i>
          删除选中项
        </button>
      </div>
    </div>

    <div
      v-else
      class="flex flex-col items-center justify-center py-24 my-8 bg-gray-800/40 backdrop-blur-xl rounded-2xl border border-gray-700/30"
    >
      <div class="text-gray-600 text-6xl mb-6">
        <i class="ri-search-2-line"></i>
      </div>
      <span class="text-xl text-gray-400 mb-4">暂无扫描结果</span>
      <p class="text-gray-500 mb-6 text-center max-w-md">
        当前还没有子域名扫描结果，请在目标管理中创建扫描任务
      </p>
      <router-link to="/target-management">
        <button
          class="px-4 py-2.5 rounded-xl text-sm font-medium bg-gray-700/50 hover:bg-gray-600/50 text-gray-200 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-gray-600/50 flex items-center"
        >
          <i class="ri-add-line mr-2"></i>
          前往创建扫描任务
        </button>
      </router-link>
    </div>
  </div>
</template>

<script>
import { ref, computed } from "vue";

export default {
  name: "SubdomainScanTable",
  props: {
    subdomainScanResults: {
      type: Array,
      required: true,
      default: () => [],
    },
  },
  setup(props, { emit }) {
    const selectedResults = ref([]);
    const selectAll = ref(false);
    const tableHeaders = [
      "扫描ID",
      "目标地址",
      "时间戳",
      "子域名数量",
      "状态",
      "操作",
    ];

    const hasSelected = computed(() => selectedResults.value.length > 0);

    const toggleSelectAll = () => {
      selectedResults.value = selectAll.value
        ? props.subdomainScanResults.map((result) => result.id)
        : [];
    };

    const getSubdomainCount = (result) => {
      if (!result || !result.data || !Array.isArray(result.data)) {
        return 0;
      }
      const subdomainGroup = result.data.find(
        (group) => group.Key === "subdomains"
      );
      return subdomainGroup?.Value?.length || 0;
    };

    const formatDate = (timestamp) => {
      return new Date(timestamp).toLocaleString("zh-CN", {
        year: "numeric",
        month: "2-digit",
        day: "2-digit",
        hour: "2-digit",
        minute: "2-digit",
      });
    };

    // 操作处理方法
    const handleViewDetails = (id) => emit("view-details", id);
    const handleToggleRead = (result) =>
      emit("toggle-read-status", result.id, !result.is_read);
    const handleDelete = (id) => emit("delete-result", id);
    const handleBatchRead = () =>
      emit("mark-selected-read", selectedResults.value);
    const handleBatchDelete = () =>
      emit("delete-selected", selectedResults.value);

    return {
      selectedResults,
      selectAll,
      tableHeaders,
      hasSelected,
      toggleSelectAll,
      getSubdomainCount,
      formatDate,
      handleViewDetails,
      handleToggleRead,
      handleDelete,
      handleBatchRead,
      handleBatchDelete,
    };
  },
};
</script>

<style scoped>
.action-button {
  @apply px-3 py-1.5 rounded-xl text-xs font-medium
  transition-all duration-200
  focus:outline-none focus:ring-2 focus:ring-opacity-50
  disabled:opacity-50 disabled:cursor-not-allowed;
}

.batch-button {
  @apply px-4 py-2.5 rounded-xl text-sm font-medium
  transition-all duration-200
  focus:outline-none focus:ring-2 focus:ring-opacity-50
  disabled:opacity-50 disabled:cursor-not-allowed;
}

/* 优化按钮点击效果 */
.batch-button:active:not(:disabled) {
  transform: scale(0.98);
}

/* 自定义复选框样式 */
input[type="checkbox"] {
  @apply rounded-md border-gray-700/50 bg-gray-900/50;
}
</style>
