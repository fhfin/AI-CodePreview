<script setup lang="ts">
import { ref } from 'vue'

const features = ref([
  {
    title: '极速构建',
    description: '基于 Vite 驱动，冷启动速度飞快，模块热更新毫秒级响应。',
    icon: '⚡️'
  },
  {
    title: 'Vue 3 强力驱动',
    description: '使用 Composition API，享受更灵活的代码组织方式和更好的 TypeScript 支持。',
    icon: '💚'
  },
  {
    title: '现代化设计',
    description: '默认集成的现代化 UI 设计风格，响应式布局，适配各种设备屏幕。',
    icon: '🎨'
  }
])

const handleStart = () => {
  console.log('Get Started Clicked')
  alert('欢迎开始您的旅程！🚀')
}
</script>

<template>
  <div class="app-container">
    <!-- Navbar -->
    <nav class="navbar">
      <div class="logo">AI CodePreview</div>
      <div class="nav-links">
        <a href="#features">特性</a>
        <a href="#about">关于</a>
        <a href="https://github.com" target="_blank" class="github-link">GitHub</a>
      </div>
    </nav>

    <!-- Hero Section -->
    <header class="hero">
      <div class="hero-content">
        <h1 class="title">
          打造下一代 <span class="gradient-text">Web 体验</span>
        </h1>
        <p class="subtitle">
          简单、快速、优雅。为您提供最佳的前端开发基础模板。
          <br>即刻开始构建您的梦想应用。
        </p>
        <div class="cta-group">
          <button class="btn primary" @click="handleStart">立即开始</button>
          <button class="btn secondary">了解更多</button>
        </div>
      </div>
      <div class="hero-decoration">
        <div class="blob blob-1"></div>
        <div class="blob blob-2"></div>
      </div>
    </header>

    <!-- Features Section -->
    <section id="features" class="features">
      <div class="section-header">
        <h2>为什么选择我们？</h2>
        <p>我们致力于提供最优质的开发体验和用户界面。</p>
      </div>
      <div class="feature-grid">
        <div v-for="(feature, index) in features" :key="index" class="feature-card">
          <div class="feature-icon">{{ feature.icon }}</div>
          <h3>{{ feature.title }}</h3>
          <p>{{ feature.description }}</p>
        </div>
      </div>
    </section>

    <!-- Footer -->
    <footer class="footer">
      <p>© 2024 AI CodePreview. All rights reserved.</p>
    </footer>
  </div>
</template>

<style>
/* Global Resets */
:root {
  --primary-color: #6366f1;
  --primary-hover: #4f46e5;
  --bg-color: #f8fafc;
  --text-main: #1e293b;
  --text-muted: #64748b;
  --card-bg: #ffffff;
}

body {
  margin: 0;
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  background-color: var(--bg-color);
  color: var(--text-main);
  line-height: 1.6;
}
</style>

<style scoped>
.app-container {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

/* Navbar */
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.5rem 2rem;
  max-width: 1200px;
  margin: 0 auto;
  width: 100%;
  box-sizing: border-box;
  position: relative;
  z-index: 10;
}

.logo {
  font-size: 1.5rem;
  font-weight: 800;
  background: linear-gradient(135deg, #6366f1 0%, #a855f7 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  cursor: pointer;
}

.nav-links {
  display: flex;
  gap: 2rem;
}

.nav-links a {
  text-decoration: none;
  color: var(--text-muted);
  font-weight: 500;
  transition: color 0.3s;
}

.nav-links a:hover {
  color: var(--primary-color);
}

/* Hero Section */
.hero {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 6rem 2rem;
  position: relative;
  overflow: hidden;
}

.hero-content {
  max-width: 800px;
  position: relative;
  z-index: 5;
}

.title {
  font-size: 4rem;
  font-weight: 900;
  line-height: 1.1;
  margin-bottom: 1.5rem;
  letter-spacing: -0.02em;
}

.gradient-text {
  background: linear-gradient(135deg, #6366f1 0%, #ec4899 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.subtitle {
  font-size: 1.25rem;
  color: var(--text-muted);
  margin-bottom: 3rem;
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
}

.cta-group {
  display: flex;
  gap: 1rem;
  justify-content: center;
}

.btn {
  padding: 0.8rem 2rem;
  border-radius: 9999px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  border: none;
}

.btn.primary {
  background-color: var(--primary-color);
  color: white;
  box-shadow: 0 4px 14px 0 rgba(99, 102, 241, 0.39);
}

.btn.primary:hover {
  background-color: var(--primary-hover);
  transform: translateY(-2px);
  box-shadow: 0 6px 20px 0 rgba(99, 102, 241, 0.23);
}

.btn.secondary {
  background-color: white;
  color: var(--text-main);
  border: 1px solid #e2e8f0;
}

.btn.secondary:hover {
  background-color: #f8fafc;
  border-color: #cbd5e1;
}

/* Background Decorations */
.hero-decoration {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
  pointer-events: none;
}

.blob {
  position: absolute;
  filter: blur(80px);
  opacity: 0.4;
  border-radius: 50%;
}

.blob-1 {
  top: -10%;
  left: -10%;
  width: 500px;
  height: 500px;
  background: #c084fc;
}

.blob-2 {
  bottom: -10%;
  right: -10%;
  width: 400px;
  height: 400px;
  background: #6366f1;
}

/* Features Section */
.features {
  padding: 6rem 2rem;
  background-color: white;
}

.section-header {
  text-align: center;
  margin-bottom: 4rem;
}

.section-header h2 {
  font-size: 2.5rem;
  font-weight: 800;
  margin-bottom: 1rem;
}

.section-header p {
  color: var(--text-muted);
  font-size: 1.1rem;
}

.feature-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
  max-width: 1200px;
  margin: 0 auto;
}

.feature-card {
  padding: 2rem;
  border-radius: 1rem;
  background-color: #f8fafc;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  border: 1px solid transparent;
}

.feature-card:hover {
  transform: translateY(-5px);
  background-color: white;
  box-shadow: 0 10px 30px -10px rgba(0, 0, 0, 0.1);
  border-color: #e2e8f0;
}

.feature-icon {
  font-size: 2.5rem;
  margin-bottom: 1.5rem;
}

.feature-card h3 {
  font-size: 1.25rem;
  font-weight: 700;
  margin-bottom: 0.5rem;
}

.feature-card p {
  color: var(--text-muted);
  font-size: 0.95rem;
}

/* Footer */
.footer {
  padding: 2rem;
  text-align: center;
  color: var(--text-muted);
  font-size: 0.9rem;
  border-top: 1px solid #e2e8f0;
  margin-top: auto;
}

/* Responsive */
@media (max-width: 768px) {
  .title {
    font-size: 2.5rem;
  }
  
  .navbar {
    flex-direction: column;
    gap: 1rem;
  }
  
  .nav-links {
    gap: 1rem;
    font-size: 0.9rem;
  }
}
</style>
