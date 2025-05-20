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
      <div class="content-section">
        <h3 class="section-title">今日心情</h3>
        <div class="mood-selector">
          <span 
            v-for="mood in moods" 
            :key="mood.value"
            :class="['mood-item', { 'selected': selectedMood === mood.value }]"
            @click="selectedMood = mood.value"
          >
            {{ mood.emoji }}
          </span>
        </div>
      </div>

      <div class="content-section">
        <h3 class="section-title">日常记录</h3>
        <textarea 
          v-model="dailyText"
          class="pixel-textarea"
          placeholder="记录今天发生的事情..."
          rows="8"
        ></textarea>
      </div>

      <div class="content-section">
        <h3 class="section-title">照片</h3>
        <div class="photo-grid">
          <div 
            v-for="(photo, index) in photos" 
            :key="index"
            class="photo-slot"
          >
            <img v-if="photo" :src="photo" :alt="`照片${index + 1}`" />
            <div v-else class="empty-photo">📷</div>
          </div>
          <button class="add-photo-btn pixel-btn">添加照片</button>
        </div>
      </div>

      <div class="action-buttons">
        <button class="save-btn pixel-btn" @click="saveDiary">保存日记</button>
        <button class="delete-btn pixel-btn" @click="deleteDiary">删除</button>
      </div>
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
      selectedMood: '😊',
      dailyText: '',
      photos: [null, null, null, null], // 最多4张照片
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
    }
  },
  methods: {
    goBack() {
      this.$emit('back')
    },
    saveDiary() {
      const diaryData = {
        date: this.selectedDate,
        mood: this.selectedMood,
        text: this.dailyText,
        photos: this.photos.filter(photo => photo !== null)
      }
      console.log('保存日记:', diaryData)
      // 这里可以添加保存到本地存储或发送到服务器的逻辑
      alert('日记已保存！')
    },
    deleteDiary() {
      if (confirm('确定要删除这篇日记吗？')) {
        this.selectedMood = '😊'
        this.dailyText = ''
        this.photos = [null, null, null, null]
        alert('日记已删除！')
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
  margin-bottom: 30px;
  padding-bottom: 15px;
  border-bottom: 3px solid #333;
}

.back-btn {
  font-size: 12px;
  padding: 8px 12px;
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

.content-section {
  margin-bottom: 25px;
}

.section-title {
  font-size: 14px;
  color: #333;
  margin-bottom: 10px;
  border-bottom: 2px solid #666;
  padding-bottom: 5px;
}

.mood-selector {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}

.mood-item {
  font-size: 24px;
  padding: 8px;
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

.pixel-textarea {
  width: 100%;
  font-family: 'WenQuanYi-Pixel', 'Courier New', monospace;
  font-size: 12px;
  padding: 12px;
  border: 3px solid #333;
  background: white;
  resize: vertical;
  min-height: 120px;
}

.pixel-textarea:focus {
  outline: none;
  box-shadow: inset 2px 2px 0px #666;
}

.photo-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
  max-width: 300px;
}

.photo-slot {
  aspect-ratio: 1;
  border: 2px solid #333;
  background: #f5f5f5;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  color: #999;
}

.photo-slot img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.empty-photo {
  font-size: 24px;
}

.add-photo-btn {
  grid-column: span 2;
  padding: 10px;
  font-size: 10px;
}

.action-buttons {
  display: flex;
  gap: 15px;
  margin-top: 30px;
}

.pixel-btn {
  font-family: 'WenQuanYi-Pixel', 'Courier New', monospace;
  padding: 10px 15px;
  border: 3px solid #333;
  background: white;
  color: #333;
  cursor: pointer;
  transition: all 0.1s;
  font-size: 10px;
}

.pixel-btn:hover {
  background: #333;
  color: white;
  transform: translate(-2px, -2px);
  box-shadow: 2px 2px 0px #666;
}

.pixel-btn:active {
  transform: translate(0, 0);
  box-shadow: none;
}

.save-btn {
  background: #28a745;
  border-color: #28a745;
  color: white;
}

.save-btn:hover {
  background: #218838;
}

.delete-btn {
  background: #dc3545;
  border-color: #dc3545;
  color: white;
}

.delete-btn:hover {
  background: #c82333;
}
</style>