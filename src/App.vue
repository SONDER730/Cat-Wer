<template>
  <div id="app" class="pixel-container">
    <!-- 添加背景动画层 -->
    <div class="background-animation">
      <!-- 左区域滚动 -->
      <div class="left-scroll">
        <div class="scroll-images" :style="leftScrollStyle">
          <div v-for="(image, index) in backgroundImages" :key="'left-' + index" 
               class="bg-image" 
               :style="{ backgroundImage: `url('/Cat-Wer/images/${image}')` }">
          </div>
        </div>
      </div>
      <!-- 中间条纹分割 -->
      <div class="center-stripes"></div>
      
      <!-- 右区域滚动 -->
      <div class="right-scroll">
        <div class="scroll-images" :style="rightScrollStyle">
          <div v-for="(image, index) in backgroundImages" :key="'left-' + index" 
              class="bg-image" 
              :style="{ backgroundImage: `url('/Cat-Wer/images/${image}')` }">
          </div>
        </div>
      </div>
    </div>



    <div class="cyber-home-layout">
      <!-- 顶部标题区域 -->
      <header class="top-header">
        <div class="logo-area">
          <li>
            <h1 class="pixel-title">  喵喵喵 嗷嗷嗷 喵喵喵  </h1>
            <h1 class="pixel-title">  嗷嗷嗷 喵喵喵 嗷嗷嗷  </h1>
          </li>
          <div class="status-info">
            <span class="date">{{ currentDate }}</span>
            <span class="weather">☀️ 晴朗</span>
          </div>
        </div>
      </header>

      <!-- 主要内容区域 -->
      <main class="main-content">
        <!-- 左侧边栏 -->
        <aside class="left-sidebar">
          <div class="avatar-section">
            <div class="pixel-avatar me">
              <div class="avatar-img">👨‍💻</div>
              <span class="avatar-name">来自远古时代的暗黑忍者</span>
            </div>
          </div>
          <div class="quick-stats">
            <div class="stat-item">
              <span class="stat-label">今日心情</span>
              <span class="stat-value">😊</span>
            </div>
          </div>
        </aside>

        <!-- 中央内容区 - 动态组件 -->
        <section class="center-content">
            <component 
              :is="currentComponent"
              @navigate="handleNavigation"
            ></component>
        </section>

        <!-- 右侧边栏 -->
        <aside class="right-sidebar">
          <div class="avatar-section">
            <div class="pixel-avatar her">
              <div class="avatar-img">👩‍🎨</div>
              <span class="avatar-name">来自银河纪元的夺命杀手</span>
            </div>
          </div>
          <div class="quick-stats">
            <div class="stat-item">
              <span class="stat-label">今日心情</span>
              <span class="stat-value">🌸</span>
            </div>
          </div>
        </aside>
      </main>

      <!-- 底部导航栏 - 6个连续的块 -->
      <footer class="bottom-nav">
        <div class="nav-container">
          <button 
            v-for="(nav, index) in navItems" 
            :key="index"
            :class="['nav-block', { 'active': currentSection === nav.id }]"
            @click="switchSection(nav.id)"
          >
            <span class="nav-icon">{{ nav.icon }}</span>
            <span class="nav-text">{{ nav.text }}</span>
          </button>
        </div>
      </footer>
    </div>
  </div>
</template>

<script>
// 导入组件
import HomeSection from './components/HomeSection.vue'
import LifeSection from './components/LifeSection.vue'
import OtomeSection from './components/OtomeSection.vue'
import CreateSection from './components/CreateSection.vue'
import CollectSection from './components/CollectSection.vue'
import ExtraSection from './components/ExtraSection.vue'
import DailyEntry from './components/DailyEntry.vue'

export default {
  name: 'App',
  components: {
    HomeSection,
    LifeSection,
    OtomeSection,
    CreateSection,
    CollectSection,
    ExtraSection,
    DailyEntry,
  },
  data() {
    return {
      currentDate: new Date().toLocaleDateString('zh-CN'),
      currentSection: 'home',
      navItems: [
        { id: 'home', icon: '🏠', text: '首页' },
        { id: 'life', icon: '🌱', text: '生活生活好好活着' },
        { id: 'otome', icon: '💕', text: '乙女其实是天堂吧' },
        { id: 'create', icon: '🎨', text: '创作促进小脑成长' },
        { id: 'collect', icon: '📦', text: '收藏癖就是什么都会堆在一起' },
        { id: 'extra', icon: '❓', text: '没有想好做什么所以先放着' }
      ],
      // 背景图片数组
      backgroundImages: Array.from({length: 21}, (_, i) => `bg${i}.jpg`),
      // 当前滚动位置
      scrollPosition: 0,
      // 总循环时间
      totalCycleTime: 150000, // 30秒完整循环
      // 动画定时器
      animationTimer: null
    }
  },
  computed: {
    currentComponent() {
      const componentMap = {
        'home': 'HomeSection',
        'life': 'LifeSection',
        'otome': 'OtomeSection',
        'create': 'CreateSection',
        'collect': 'CollectSection',
        'extra': 'ExtraSection'
      }
      return componentMap[this.currentSection] || 'HomeSection'
    },
    leftScrollStyle() {
      const translateY = -this.scrollPosition * 600;
      
      return {
        transform: `translateY(${translateY}%)`,
        opacity: 1
      }
    },
    rightScrollStyle() {

      const delayedPosition = (this.scrollPosition + 0.5) % 1; // 延迟10%
      const translateY = -delayedPosition * 600;
      
      return {
        transform: `translateY(${translateY}%)`,
        opacity: 1
      }
    }
  },
  mounted() {
    this.startAnimation()
  },
  beforeUnmount() {
    if (this.animationTimer) {
      clearInterval(this.animationTimer)
    }
  },
  methods: {
    switchSection(sectionId) {
      this.currentSection = sectionId
    },
    startAnimation() {
      // 每100ms更新一次滚动位置
      this.animationTimer = setInterval(() => {
        this.scrollPosition += 100 / this.totalCycleTime
        
        // 重置循环
        if (this.scrollPosition >= 1) {
          this.scrollPosition = 0
        }
      }, 100)
    },
      switchSection(sectionId) {
        this.currentSection = sectionId
      },
      handleNavigation(sectionId) {
        // 处理从 HomeSection 传来的导航事件
        this.currentSection = sectionId
      },
  }
}
</script>

<style>
/* 定义像素字体 */
@font-face {
  font-family: 'WenQuanYi-Pixel';
  src: url('/fonts/WenQuanYi-Bitmap-12px.ttf') format('truetype');
  font-weight: normal;
  font-style: normal;
  font-display: swap;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'WenQuanYi-Pixel', 'Courier New', monospace;
  min-height: 100vh;
  overflow-x: hidden;
  font-smooth: never;
  -webkit-font-smoothing: none;
  -moz-osx-font-smoothing: unset;
  text-rendering: optimizeSpeed;
}


.background-animation {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
  overflow: hidden;
  border-left: 25px solid black;
  border-right: 25px solid black;
}


.right-scroll {
  position: absolute;
  width: 30%;
  height: 100%;
  overflow: hidden;
  border-right: 5px solid rgb(255, 133, 154);
  box-sizing: border-box;
}

.left-scroll {
  position: absolute;
  width: 30%;
  height: 100%;
  overflow: hidden;
  border-left: 5px solid rgb(255, 133, 154);
  box-sizing: border-box;
}

.left-scroll {
  left: 0;
}

.right-scroll {
  right: 0;
}

/* 滚动图片容器 */
.scroll-images {
  position: relative;
  width: 100%;
  height: 600%; /* 双倍高度用于滚动 */
  transition: transform 0.1s linear, opacity 0.5s ease;
}

/* 单个背景图片 */
.bg-image {
  width: 100%;
  height: 33.33%; /* 每张图占1/3高度，总共200%，所以是66.66%/2 */
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

/* 中间条纹分割区 */
.center-stripes {
  position: absolute;
  left: 30%;
  width: 40%;
  height: 100%;
  background: repeating-linear-gradient(
    45deg,
  #6a5acd 0px,
  #6a5acd 4px,
    #000 4px,
    #000 8px
  );
  opacity: 0.8;
  /* 添加一些动画效果 */
  animation: stripeShift 4s linear infinite;
}

@keyframes stripeShift {
  0% { background-position: 0 0; }
  100% { background-position: 8px 8px; }
}


.pixel-container {
  min-height: 100vh;
  padding: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
}

.cyber-home-layout {
  width: 100%;
  max-width: 1150px;
  height: 90vh;
  background: rgba(255, 255, 255, 0.85);
  display: grid;
  grid-template-rows: auto 1fr auto;
  grid-template-areas: 
    "header"
    "main"
    "footer";
  position: relative;
  z-index: 1;
  
}

/* 顶部标题区域 */
.top-header {
  grid-area: header;
  padding: 20px;
  padding-bottom: 40px;
  background: linear-gradient(90deg, #afccef, #e3c2de);
  width: 115%;
  margin-left: -7.5%;
  border: 8px solid #d17fd1;
}

.logo-area {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.pixel-title {
  color: white;
  font-size: 28px;
  font-weight: bold;
  padding: 5px 0;
  font-family: 'WenQuanYi-Pixel', 'Courier New', monospace;
  text-shadow: 2px 2px 0px #333;
}

.status-info {
  display: flex;
  gap: 20px;
  color: white;
  font-size: 30px;
  font-family: 'WenQuanYi-Pixel', 'Courier New', monospace;
}

/* 主要内容区域 */
.main-content {
  grid-area: main;
  display: grid;
  grid-template-columns: 200px 1fr 200px;
  grid-template-areas: "left center right";
  min-height: 0;
  border: 8px solid #ecc4d1;
}

/* 左右侧边栏 */
.left-sidebar, .right-sidebar {
  border-right: 8px solid #e4a5ad;
  padding: 20px;
  background: #f8f9fa;
  display: flex;
  flex-direction: column;

}

.right-sidebar {
  border-right: none;
  border-left: 4px solid #212529;
}

.avatar-section {
  text-align: center;
}

.pixel-avatar {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}

.avatar-img {
  font-size: 48px;
  animation: float 3s ease-in-out infinite;
}

.avatar-name {
  font-size: 25px;
  color: #333;
  font-weight: bold;
  font-family: 'WenQuanYi-Pixel', 'Courier New', monospace;
}

.quick-stats {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.stat-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 5px;
}

.stat-label {
  padding-top: 30px;
  font-size: 20px;
  color: #666;
  font-family: 'WenQuanYi-Pixel', 'Courier New', monospace;
}

.stat-value {
  font-size: 20px;
}

/* 中央内容区 */
.center-content {
  grid-area: center;
  padding: 20px;
  overflow-y: auto;
}

/* 底部导航栏 - 6个连续的块 */
.bottom-nav {
  grid-area: footer;
  border-top: 4px solid #212529;
  background: #000000;
  padding: 0;
  width: 107%;
  margin-left: -3.5%;
  border: 8px solid #eeeded;
}

.nav-container {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  height: 80px;
}

.nav-block {
  background: #e7cef1;
  border: none;
  border-right: 2px solid #212529;
  color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 5px;
  cursor: pointer;
  transition: all 0.2s;
  font-family: 'WenQuanYi-Pixel', 'Courier New', monospace;
}

.nav-block:last-child {
  border-right: none;
}

.nav-block:hover {
  background: #6c757d;
  transform: translateY(-2px);
}

.nav-block.active {
  background: #94c0ef;
  color: white;
}

.nav-icon {
  font-size: 20px;
}

.nav-text {
  font-size: 20px;
  text-align: center;
  line-height: 1.2;
  font-family: 'WenQuanYi-Pixel', 'Courier New', monospace;
}

/* 动画效果 */
@keyframes float {
  0% { background-position: 0 0; }
  100% { background-position: 16px 16px; } 
}

/* 响应式设计 */
@media (max-width: 768px) {
  .cyber-home-layout {
    height: 90vh;
  }
  
  .main-content {
    grid-template-columns: 1fr;
    grid-template-areas: "center";
  }
  
  .left-sidebar, .right-sidebar {
    display: none;
  }
  
  .nav-text {
    font-size: 8px;
  }
  
  .nav-container {
    grid-template-columns: repeat(6, 1fr);
  }
}
</style>