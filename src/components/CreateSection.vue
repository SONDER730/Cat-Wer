<template>
  <div class="create-section">
    <!-- 标题区域 -->
    <div class="section-header">
      <h2 class="section-title">
        作品收容所
      </h2>
    </div>

    <!-- 瀑布流画廊 -->
    <div class="gallery-container" @scroll="handleScroll" ref="galleryContainer">
      <div class="masonry-grid">
        <div
          v-for="(item, index) in displayedItems"
          :key="item.id"
          class="gallery-item"
          @mouseenter="showTooltip(item, $event)"
          @mouseleave="hideTooltip"
          @mousemove="moveTooltip($event)"
        >
          <!-- 图片容器 -->
          <div class="image-container">
            <img 
              :src="item.image" 
              :alt="item.title"
              @error="handleImageError"
              @load="handleImageLoad"
            />
            
            <!-- 简单标题覆盖层 -->
            <div class="image-overlay">
              <span class="image-title">{{ item.title }}</span>
              <span class="image-date">{{ item.date }}</span>
            </div>
          </div>
        </div>
      </div>

      <!-- 加载提示 -->
      <div v-if="isLoading" class="loading-indicator">
        <div class="pixel-loader">加载中...</div>
      </div>

      <!-- 到底提示 -->
      <div v-if="hasReachedEnd" class="end-indicator">
        <div class="pixel-message">~ 已经到底啦 ~</div>
      </div>
    </div>

    <!-- 悬浮文字浮窗 -->
    <div 
      v-if="tooltip.visible" 
      class="tooltip"
      :style="{ left: tooltip.x + 'px', top: tooltip.y + 'px' }"
    >
      <div class="tooltip-content">
        <h4 class="tooltip-title">{{ tooltip.data.title }}</h4>
        <p class="tooltip-desc">{{ tooltip.data.description }}</p>
        <div class="tooltip-info">
          <span class="tooltip-date">{{ tooltip.data.date }}</span>
          <span class="tooltip-type">{{ tooltip.data.type }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CreateSection',
  data() {
    return {
      // 当前显示的作品数量
      displayedCount: 12,
      loadIncrement: 6, // 每次加载增加的数量
      isLoading: false,
      hasReachedEnd: false,

      // 浮窗数据
      tooltip: {
        visible: false,
        x: 0,
        y: 0,
        data: {}
      },

      // 作品数据 (这里用占位数据，实际可以从API获取)
      allItems: [
        {
          id: 1,
          title: '可爱小章鱼',
          description: '给各位看看它放了“血水”的样子',
          date: '2024-03-04',
          image: '/Cat-Wer/images/artwork/art1.jpg'
        },
        {
          id: 2,
          title: '雨',
          description: '人间果然太苦了，它一被创作出来就哭了',
          date: '2022-12-03',
          image: '/Cat-Wer/images/artwork/art2.jpg'
        },
                
        {
          id: 3,
          image: '/Cat-Wer/images/artwork/art3.jpg'
        },
        {
          id: 4,
          image: '/Cat-Wer/images/artwork/art4.jpg'
        },
      ]        

    }
  },
  computed: {
    displayedItems() {
      return this.allItems.slice(0, this.displayedCount)
    }
  },
  methods: {
    handleScroll(event) {
      const container = event.target
      const scrollHeight = container.scrollHeight
      const scrollTop = container.scrollTop
      const clientHeight = container.clientHeight
      
      // 当滚动到距底部50px时加载更多
      if (scrollHeight - scrollTop - clientHeight < 50 && !this.isLoading && !this.hasReachedEnd) {
        this.loadMore()
      }
    },

    loadMore() {
      if (this.displayedCount >= this.allItems.length) {
        this.hasReachedEnd = true
        return
      }

      this.isLoading = true
      
      // 模拟加载延迟
      setTimeout(() => {
        this.displayedCount = Math.min(
          this.displayedCount + this.loadIncrement, 
          this.allItems.length
        )
        this.isLoading = false
        
        if (this.displayedCount >= this.allItems.length) {
          this.hasReachedEnd = true
        }
      }, 1000)
    },

    showTooltip(item, event) {
      this.tooltip = {
        visible: true,
        x: event.clientX + 15,
        y: event.clientY - 10,
        data: item
      }
    },

    hideTooltip() {
      this.tooltip.visible = false
    },

    moveTooltip(event) {
      if (this.tooltip.visible) {
        this.tooltip.x = event.clientX + 15
        this.tooltip.y = event.clientY - 10
      }
    },

    handleImageError() {
      console.log('图片加载失败')
    },

    handleImageLoad() {
      // 图片加载完成后可以执行一些操作
    }
  }
}
</script>

<style scoped>
.create-section {
  height: 100%;
  font-family: 'WenQuanYi-Pixel', 'Courier New', monospace;
  display: flex;
  flex-direction: column;
}

/* 标题区域 */
.section-header {
  padding: 20px;
  text-align: center;
  background: #f8f9fa;
  border-bottom: 3px solid #333;
}

.section-title {
  color: #333;
  font-size: 36px;
  margin-bottom: 10px;
  display: center;
  align-items: center;
  justify-content: center;
  gap: 10px;
}

.title-icon {
  font-size: 40px;
}

.section-description {
  color: #666;
  font-size: 10px;
}

/* 画廊容器 */
.gallery-container {
  flex: 1;
  overflow-y: auto;
  padding: 20px;
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
}

/* 瀑布流网格 */
.masonry-grid {
  columns: 3;
  column-gap: 15px;
}

@media (max-width: 768px) {
  .masonry-grid {
    columns: 2;
  }
}

@media (max-width: 480px) {
  .masonry-grid {
    columns: 1;
  }
}

/* 画廊项目 */
.gallery-item {
  break-inside: avoid;
  margin-bottom: 15px;
  cursor: pointer;
  transition: transform 0.2s ease;
}

.gallery-item:hover {
  transform: translateY(-3px);
}

.image-container {
  position: relative;
  background: white;
  border: 3px solid #333;
  overflow: hidden;
}

.image-container img {
  width: 100%;
  height: auto;
  display: block;
  object-fit: cover;
}

/* 图片覆盖层 */
.image-overlay {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  background: linear-gradient(transparent, rgba(0,0,0,0.8));
  color: white;
  padding: 15px 10px 10px;
  transform: translateY(100%);
  transition: transform 0.3s ease;
}

.gallery-item:hover .image-overlay {
  transform: translateY(0);
}

.image-title {
  display: block;
  font-size: 11px;
  font-weight: bold;
  margin-bottom: 3px;
}

.image-date {
  font-size: 8px;
  opacity: 0.8;
}

/* 加载和结束提示 */
.loading-indicator,
.end-indicator {
  text-align: center;
  padding: 20px;
}

.pixel-loader,
.pixel-message {
  display: inline-block;
  padding: 10px 20px;
  border: 3px solid #333;
  background: white;
  font-size: 11px;
  color: #333;
}

.pixel-loader {
  animation: pulse 1.5s ease-in-out infinite;
}

@keyframes pulse {
  0%, 100% { background: white; }
  50% { background: #f0f0f0; }
}

/* 浮窗提示 */
.tooltip {
  position: fixed;
  z-index: 1000;
  pointer-events: none;
  max-width: 250px;
}

.tooltip-content {
  background: rgba(0, 0, 0, 0.9);
  color: white;
  padding: 12px;
  border: 2px solid #666;
  font-size: 10px;
  line-height: 1.4;
}

.tooltip-title {
  font-size: 11px;
  margin-bottom: 5px;
  color: #ffa500;
}

.tooltip-desc {
  margin-bottom: 8px;
  color: #ddd;
}

.tooltip-info {
  display: flex;
  justify-content: space-between;
  font-size: 8px;
  color: #bbb;
}

.tooltip-date {
  opacity: 0.8;
}

.tooltip-type {
  background: #333;
  padding: 2px 6px;
  border: 1px solid #666;
}

/* 响应式调整 */
@media (max-width: 768px) {
  .gallery-container {
    padding: 10px;
  }
  
  .masonry-grid {
    column-gap: 10px;
  }
  
  .gallery-item {
    margin-bottom: 10px;
  }
  
  .tooltip {
    max-width: 200px;
  }
  
  .tooltip-content {
    padding: 8px;
    font-size: 9px;
  }
}
</style>