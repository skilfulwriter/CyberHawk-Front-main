<template>
  <footer class="bg-gray-800/80 backdrop-blur-sm text-center py-6 sm:py-8 border-t border-gray-700/30">
    <div class="font-sans px-4">
      <p class="tracking-wide text-gray-200 text-xs sm:text-sm font-light mb-2 flex flex-wrap items-center justify-center gap-1">
        &copy; 2025 CyberHawk
        <img src="@/assets/logo.png" alt="Logo" class="h-6 sm:h-8 mx-1">
        X黑手. 保留所有权利.
      </p>

      <p class="tracking-wide text-gray-400 text-xs font-light leading-relaxed flex items-center justify-center">
        致力于保护您的网络安全与隐私
        <i class="ri-shield-keyhole-line ml-1 transform hover:scale-105 transition-transform duration-200"></i>
      </p>
    </div>
  </footer>
</template>

<script>
export default {
  name: "FooterPage",
};
</script>

<style scoped>
@media (max-width: 640px) {
  footer {
    padding-bottom: env(safe-area-inset-bottom, 1rem);
  }
}
</style>