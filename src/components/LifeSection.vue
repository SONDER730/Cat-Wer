<!-- LifeSection.vue -->
<template>
  <div class="life-section">
    <div class="section-header">
      <h2 class="section-title">
        <span class="title-icon">🌱</span>
        生活生活好好活着
      </h2>
      <p class="section-description">记录我们每天的生活点滴</p>
    </div>
    
    <div class="section-content">
      <div class="placeholder-content">
        <div class="placeholder-icon">🚧</div>
        <p>这个部分正在建设中...</p>
        <p>即将开放日常生活记录功能</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CollectSection'
}
</script>

<style scoped>
.life-section {
  padding: 20px;
  height: 100%;
  display: flex;
  flex-direction: column;
}

.section-header {
  text-align: center;
  margin-bottom: 30px;
}

.section-title {
  color: #333;
  font-size: 16px;
  margin-bottom: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
}

.title-icon {
  font-size: 20px;
}

.section-description {
  color: #666;
  font-size: 10px;
}

.section-content {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}

.placeholder-content {
  text-align: center;
  color: #999;
}

.placeholder-icon {
  font-size: 48px;
  margin-bottom: 20px;
}

.placeholder-content p {
  font-size: 10px;
  margin-bottom: 10px;
}
</style>