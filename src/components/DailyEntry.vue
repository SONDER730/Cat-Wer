<template>
  <div class="daily-entry">
    <div class="entry-header">
      <button class="back-btn pixel-btn" @click="goBack">
        ← 返回
      </button>
      <h2 class="entry-date">{{ formattedDate }}</h2>
      <div class="entry-day">{{ dayOfWeek }}</div>
    </div>

    <div class="entry-content">
      <!-- 已保存的时间线内容 -->
      <div v-if="savedEntries.length > 0" class="timeline-section">
        <h3 class="timeline-title">今日记录</h3>
        <div class="timeline">
          <div 
            v-for="(entry, index) in savedEntries" 
            :key="index"
            class="timeline-item"
          >
            <div class="timeline-time">{{ entry.time }}</div>
            <div class="timeline-content">
              <div class="timeline-mood" v-if="entry.mood">{{ entry.mood }}</div>
              <div class="timeline-text" v-if="entry.text">{{ entry.text }}</div>
            </div>
            <button class="delete-entry-btn" @click="deleteEntry(index)">×</button>
          </div>
        </div>
      </div>

      <!-- 分割线 -->
      <div v-if="savedEntries.length > 0" class="divider"></div>

      <!-- 新输入区域 -->
      <div class="input-section">
        <h3 class="input-title">添加新记录</h3>
        
        <!-- 心情选择 -->
        <div class="mood-section">
          <span class="mood-label">心情：</span>
          <div class="mood-selector">
            <span 
              v-for="mood in moods" 
              :key="mood.value"
              :class="['mood-item', { 'selected': currentMood === mood.value }]"
              @click="currentMood = currentMood === mood.value ? '' : mood.value"
            >
              {{ mood.emoji }}
            </span>
          </div>
        </div>

        <!-- 主要输入区：专注文字 -->
        <div class="main-input">
          <textarea 
            v-model="currentText"
            class="pixel-textarea"
            placeholder="写点什么..."
          ></textarea>
        </div>

        <!-- 发布按钮 -->
        <div class="publish-section">
          <button 
            class="publish-btn pixel-btn" 
            @click="publishEntry"
            :disabled="!canPublish"
          >
            发布记录
          </button>
          <button class="clear-btn pixel-btn" @click="clearInput">清空</button>
        </div>
      </div>
    </div>

    <!-- 图片查看模态框 -->
    <div v-if="viewingPhoto" class="photo-modal" @click="closePhotoView">
      <img :src="viewingPhoto" alt="查看图片" class="modal-photo" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'DailyEntry',
  props: {
    selectedDate: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      // 当前输入的内容
      currentMood: '',
      currentText: '',
      
      // 已保存的记录
      savedEntries: [],
      
      // 查看图片
      viewingPhoto: null,
      
      moods: [
        { value: '😊', emoji: '😊' },
        { value: '😢', emoji: '😢' },
        { value: '😴', emoji: '😴' },
        { value: '🤔', emoji: '🤔' },
        { value: '🎉', emoji: '🎉' },
        { value: '😅', emoji: '😅' },
        { value: '😰', emoji: '😰' },
        { value: '🥰', emoji: '🥰' }
      ]
    }
  },
  computed: {
    formattedDate() {
      const date = new Date(this.selectedDate)
      return `${date.getFullYear()}年${date.getMonth() + 1}月${date.getDate()}日`
    },
    dayOfWeek() {
      const days = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六']
      const date = new Date(this.selectedDate)
      return days[date.getDay()]
    },
    canPublish() {
      return this.currentText.trim() || this.currentMood
    }
  },
  mounted() {
    this.loadEntries()
  },
  methods: {
    goBack() {
      this.$emit('back')
    },
    
    publishEntry() {
      if (!this.canPublish) return
      
      const newEntry = {
        time: new Date().toLocaleTimeString('zh-CN', { hour: '2-digit', minute: '2-digit' }),
        mood: this.currentMood,
        text: this.currentText.trim(),
        timestamp: Date.now()
      }
      
      this.savedEntries.push(newEntry)
      this.saveEntries()
      this.clearInput()
      
      // 滚动到底部显示新添加的内容
      this.$nextTick(() => {
        const timeline = this.$el.querySelector('.timeline-section')
        if (timeline) {
          timeline.scrollIntoView({ behavior: 'smooth', block: 'end' })
        }
      })
    },
    
    clearInput() {
      this.currentMood = ''
      this.currentText = ''
    },
    
    deleteEntry(index) {
      if (confirm('确定要删除这条记录吗？')) {
        this.savedEntries.splice(index, 1)
        this.saveEntries()
      }
    },
    
    viewPhoto(photo) {
      this.viewingPhoto = photo
    },
    
    closePhotoView() {
      this.viewingPhoto = null
    },
    
    saveEntries() {
      // 保存到本地存储
      const key = `diary_${this.selectedDate}`
      localStorage.setItem(key, JSON.stringify(this.savedEntries))
    },
    
    loadEntries() {
      // 从本地存储加载
      const key = `diary_${this.selectedDate}`
      const saved = localStorage.getItem(key)
      if (saved) {
        this.savedEntries = JSON.parse(saved)
      }
    }
  }
}
</script>

<style scoped>
.daily-entry {
  padding: 20px;
  height: 100%;
  overflow-y: auto;
  font-family: 'WenQuanYi-Pixel', 'Courier New', monospace;
}

.entry-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 20px;
  padding-bottom: 15px;
  border-bottom: 3px solid #333;
}

.back-btn {
  font-size: 12px;
  padding: 8px 12px;
}

.pixel-btn {
  font-family: 'WenQuanYi-Pixel', 'Courier New', monospace;
  padding: 8px 12px;
  border: 3px solid #333;
  background: white;
  color: #333;
  cursor: pointer;
  transition: all 0.1s;
  font-size: 15px;
}
.entry-date {
  font-size: 18px;
  color: #333;
  text-align: center;
}

.entry-day {
  font-size: 12px;
  color: #666;
}

/* 时间线区域 */
.timeline-section {
  margin-bottom: 30px;
}

.timeline-title {
  font-size: 14px;
  color: #333;
  margin-bottom: 15px;
  padding-bottom: 5px;
  border-bottom: 2px solid #666;
}

.timeline {
  max-height: 300px;
  overflow-y: auto;
  border: 2px solid #ddd;
  background: #fafafa;
  padding: 10px;
}

.timeline-item {
  background: white;
  border: 2px solid #333;
  margin-bottom: 15px;
  padding: 12px;
  position: relative;
}

.timeline-time {
  font-size: 10px;
  color: #666;
  margin-bottom: 8px;
}

.timeline-content {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.timeline-mood {
  font-size: 18px;
}

.timeline-text {
  font-size: 12px;
  color: #333;
  line-height: 1.4;
  white-space: pre-wrap;
}

.timeline-photos {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
}

.timeline-photo {
  width: 60px;
  height: 60px;
  object-fit: cover;
  border: 2px solid #333;
  cursor: pointer;
  transition: transform 0.2s;
}

.timeline-photo:hover {
  transform: scale(1.1);
}

.timeline-photos {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
}

.timeline-photo {
  width: 60px;
  height: 60px;
  object-fit: cover;
  border: 2px solid #333;
  cursor: pointer;
  transition: transform 0.2s;
}

.timeline-photo:hover {
  transform: scale(1.1);
}

.delete-entry-btn {
  position: absolute;
  top: 5px;
  right: 5px;
  background: #dc3545;
  color: white;
  border: 1px solid #333;
  width: 20px;
  height: 20px;
  font-size: 12px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

/* 分割线 */
.divider {
  height: 3px;
  background: repeating-linear-gradient(
    90deg,
    #333 0px,
    #333 10px,
    transparent 10px,
    transparent 20px
  );
  margin: 20px 0;
}

/* 输入区域 */
.input-section {
  background: #f9f9f9;
  border: 3px solid #333;
  padding: 15px;
}

.input-title {
  font-size: 14px;
  color: #333;
  margin-bottom: 15px;
}

.mood-section {
  display: flex;
  align-items: center;
  gap: 15px;
  margin-bottom: 15px;
}

.mood-label {
  font-size: 12px;
  color: #333;
}

.mood-selector {
  display: flex;
  gap: 8px;
}

.mood-item {
  font-size: 18px;
  padding: 5px;
  border: 2px solid #ddd;
  background: white;
  cursor: pointer;
  transition: all 0.2s;
}

.mood-item:hover {
  border-color: #333;
  transform: scale(1.1);
}

.mood-item.selected {
  border-color: #007bff;
  background: #e7f3ff;
  transform: scale(1.15);
}

.main-input {
  margin-bottom: 15px;
}

.pixel-textarea {
  width: 100%;
  height: 150px;
  font-family: 'WenQuanYi-Pixel', 'Courier New', monospace;
  font-size: 12px;
  padding: 12px;
  border: 3px solid #333;
  background: white;
  resize: vertical;
}

.pixel-textarea:focus {
  outline: none;
  box-shadow: inset 2px 2px 0px #666;
}

/* 响应式设计 */
@media (max-width: 768px) {
  .mood-section {
    flex-direction: column;
    align-items: flex-start;
    gap: 10px;
  }
  
  .pixel-textarea {
    height: 120px;
  }
}
</style>