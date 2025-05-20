<template>
  <div class="home-section">
    <div class="welcome-area">
      <div class="sections-preview">
        <div 
          class="section-card" 
          v-for="section in sections" 
          :key="section.id"
          @click="navigateToSection(section.id)"
        >
          <div class="card-icon">{{ section.icon }}</div>
          <h3 class="card-title">{{ section.title }}</h3>
          <p class="card-description">{{ section.description }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HomeSection',
  data() {
    return {
      sections: [
        {
          id: 'life',
          icon: '🌱',
          title: '生活生活好好活着',
          description: '记录日常的点点滴滴'
        },
        {
          id: 'otome',
          icon: '💕',
          title: '乙女其实是天堂吧',
          description: '分享乙女游戏的美好'
        },
        {
          id: 'create',
          icon: '🎨',
          title: '创作促进小脑成长',
          description: '我们的创作和开发记录'
        },
        {
          id: 'collect',
          icon: '📦',
          title: '收藏癖就是什么都会堆在一起',
          description: '各种有趣的收藏品'
        },
        {
          id: 'extra',
          icon: '❓',
          title: '没有想好做什么所以先放着',
          description: '待开发的神秘区域'
        }
      ]
    }
  },
  methods: {
    navigateToSection(sectionId) {
      // 向父组件发送切换页面的事件
      this.$emit('navigate', sectionId)
    }
  }
}
</script>

<style scoped>
.home-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100%;
  text-align: center;
}

.pixel-house {
  font-size: 60px;
  animation: pulse 2s ease-in-out infinite alternate;
  margin-bottom: 20px;
}

.welcome-text {
  color: #333;
  font-size: 18px;
  margin-bottom: 10px;
}

.subtitle {
  color: #666;
  font-size: 30px;
  margin-bottom: 30px;
}

.sections-preview {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
  max-width: 600px;
  width: 100%;
}

.section-card {
  background: white;
  border: 2px solid #333;
  padding: 15px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  transition: transform 0.2s;
}

.section-card:hover {
  transform: scale(1.05);
}

.card-icon {
  font-size: 30px;
}

.card-title {
  font-size: 26px;
  color: #333;
  text-align: center;
  line-height: 1.3;
}

.card-description {
  font-size: 15px;
  color: #666;
  text-align: center;
  line-height: 1.4;
}

@keyframes pulse {
  0% { transform: scale(1); }
  100% { transform: scale(1.1); }
}

.section-card {
  background: white;
  border: 2px solid #333;
  padding: 15px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  transition: transform 0.2s;
  cursor: pointer; 
}

.section-card:hover {
  transform: scale(1.05);
  background: #f0f0f0;
}
</style>