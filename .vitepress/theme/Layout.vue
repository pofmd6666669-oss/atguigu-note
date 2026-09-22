<script setup lang="ts">
import DefaultTheme from 'vitepress/theme'
import { useData, useRouter } from 'vitepress'
import { computed } from 'vue'
import { NProgress } from 'nprogress-v2'
import 'nprogress-v2/dist/index.css'
import './style.css'

NProgress.configure({ showSpinner: false })
import { inBrowser } from 'vitepress'

const { page } = useData()
const editUrl = computed(() =>
  'https://github.com/pofmd6666669-oss/atguigu-note/edit/main/' +
  page.value.relativePath.split('/').map(encodeURIComponent).join('/'),
)

// 构建期由 vite define 注入，本地构建显示 dev
declare const __COMMIT_SHA__: string
console.log('[atguigu-note] commit:', __COMMIT_SHA__)

const router = useRouter()
// SSR 下 window 不存在，只在浏览器端注册
if (inBrowser) {
  router.onBeforeRouteChange = () => {
    NProgress.start()
  }

  router.onAfterRouteChange = () => {
    NProgress.done()
  }
}

const { Layout } = DefaultTheme
</script>

<template>
  <Layout>
    <template #doc-before>
      <div class="personal-note-toolbar">
        <a :href="editUrl" target="_blank" rel="noopener noreferrer"
           title="修改并提交后，个人笔记网站会自动更新">编辑本页 ↗</a>
      </div>
    </template>
  </Layout>
</template>

<style scoped>
.personal-note-toolbar {
  display: flex;
  justify-content: flex-end;
  margin-bottom: 16px;
  font-size: 14px;
}

.personal-note-toolbar a {
  color: var(--vp-c-brand-1);
  font-weight: 500;
  text-decoration: underline;
  text-underline-offset: 3px;
}

.personal-note-toolbar a:focus-visible {
  outline: 2px solid var(--vp-c-brand-1);
  outline-offset: 4px;
  border-radius: 2px;
}
</style>
