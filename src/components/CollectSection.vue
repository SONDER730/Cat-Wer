
<!-- CollectSection.vue -->
<template>
  <div class="collect-section">
    <div class="section-header">
      <h2 class="section-title">
        <span class="title-icon">📦</span>
        收藏癖就是什么都会堆在一起
      </h2>
      <p class="section-description">我们收集的各种有趣的东西</p>
    </div>
    
    <div class="section-content">
      <div class="placeholder-content">
        <div class="placeholder-icon">📚</div>
        <p>这个部分正在建设中...</p>
        <p>即将开放收藏展示功能</p>
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
.collect-section {
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
