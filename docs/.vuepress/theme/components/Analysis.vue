<template>
    <div class="busuanzi-stats" v-if="loaded">
      <!-- 站点统计（host） -->
      <div class="stats-line">
        <span id="busuanzi_container_site_pv">
          站点访问量：
          <span id="busuanzi_value_site_pv"></span>
        </span>
  
        <span class="divider">|</span>
  
        <span id="busuanzi_container_site_uv">
          站点访客数：
          <span id="busuanzi_value_site_uv"></span>
        </span>

        <span class="divider">|</span>

        <span id="busuanzi_container_page_pv">
          本页访问量：
          <span id="busuanzi_value_page_pv"></span>
        </span>
      </div>
  
    </div>
  </template>
  
  <script setup lang="ts">
    import { onMounted, watch, ref, nextTick } from 'vue'
    import { useRoute } from 'vue-router'
    
    const loaded = ref(false)
    const route = useRoute()
    
    function insertBusuanzi() {
      if (typeof window === 'undefined') return
    
      // 删除旧脚本（关键）
      const oldScript = document.getElementById('busuanzi-script')
      if (oldScript) {
        oldScript.remove()
      }
    
      const script = document.createElement('script')
      script.id = 'busuanzi-script'
      script.async = true
      script.src = '//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js'
    
      script.onload = () => {
        loaded.value = true
      }
    
      document.body.appendChild(script)
    }
    
    onMounted(() => {
      insertBusuanzi()
    })
    
    // 路由切换后重新加载
    watch(
      () => route.path,
      async () => {
        loaded.value = false
        await nextTick()        // 等 DOM 更新完成
        insertBusuanzi()        // 重新扫描 DOM
      }
    )
    </script>
    
  <style scoped>
  .busuanzi-stats {
    font-size: 14px;
    color: #666;
  }
  
  .stats-line {
    margin-bottom: 4px;
  }
  
  .divider {
    margin: 0 8px;
  }
  </style>
  