<template>
  <div class="life-section">
    <div v-if="!showDailyEntry" class="calendar-view">
      <div class="calendar-header">
        <button class="nav-btn pixel-btn" @click="prevMonth">◀</button>
        <h2 class="calendar-title">{{ currentMonthYear }}</h2>
        <button class="nav-btn pixel-btn" @click="nextMonth">▶</button>
      </div>

      <div class="calendar-grid">
        <!-- 星期标题 -->
        <div class="day-header">日</div>
        <div class="day-header">一</div>
        <div class="day-header">二</div>
        <div class="day-header">三</div>
        <div class="day-header">四</div>
        <div class="day-header">五</div>
        <div class="day-header">六</div>

        <!-- 日期格子 -->
        <div
          v-for="day in calendarDays"
          :key="day.key"
          :class="[
            'day-cell',
            { 
              'other-month': day.isOtherMonth,
              'today': day.isToday,
              'weekend': day.isWeekend,
              'has-entry': day.hasEntry
            }
          ]"
          @click="selectDate(day)"
        >
          <span class="day-number">{{ day.day }}</span>
          <div v-if="day.hasEntry" class="entry-indicator">●</div>
          <div v-if="day.mood" class="day-mood">{{ day.mood }}</div>
        </div>
      </div>

      <div class="calendar-legend">
        <div class="legend-item">
          <span class="legend-dot today-dot"></span>
          <span class="legend-text">今天</span>
        </div>
        <div class="legend-item">
          <span class="legend-dot entry-dot"></span>
          <span class="legend-text">有日记</span>
        </div>
      </div>
    </div>

    <!-- 日常内容页面 -->
    <DailyEntry 
      v-if="showDailyEntry"
      :selectedDate="selectedDate"
      @back="showDailyEntry = false"
    />
  </div>
</template>

<script>
import DailyEntry from './DailyEntry.vue'

export default {
  name: 'LifeSection',
  components: {
    DailyEntry
  },
  data() {
    return {
      currentDate: new Date(),
      selectedDate: '',
      showDailyEntry: false,
      // 模拟一些有日记的日期和心情
      diaryEntries: {
        '2025-01-01': { mood: '🎉' },
        '2025-01-15': { mood: '😊' },
        '2025-01-20': { mood: '💕' },
        '2025-01-28': { mood: '🤔' },
      }
    }
  },
  computed: {
    currentMonthYear() {
      const months = [
        'JANUARY', 'FEBRUARY', 'MARCH', 'APRIL', 'MAY', 'JUNE',
        'JULY', 'AUGUST', 'SEPTEMBER', 'OCTOBER', 'NOVEMBER', 'DECEMBER'
      ]
      return `${months[this.currentDate.getMonth()]} ${this.currentDate.getFullYear()}`
    },
    calendarDays() {
      const year = this.currentDate.getFullYear()
      const month = this.currentDate.getMonth()
      
      // 获取当月第一天和最后一天
      const firstDay = new Date(year, month, 1)
      const lastDay = new Date(year, month + 1, 0)
      
      // 获取日历开始日期（可能是上个月的日期）
      const startDate = new Date(firstDay)
      startDate.setDate(startDate.getDate() - firstDay.getDay())
      
      const days = []
      const today = new Date()
      
      // 生成6周的日期（42天）
      for (let i = 0; i < 42; i++) {
        const date = new Date(startDate)
        date.setDate(startDate.getDate() + i)
        
        const dateString = date.toISOString().split('T')[0]
        const isOtherMonth = date.getMonth() !== month
        const isToday = date.toDateString() === today.toDateString()
        const isWeekend = date.getDay() === 0 || date.getDay() === 6
        const hasEntry = !!this.diaryEntries[dateString]
        const mood = this.diaryEntries[dateString]?.mood || ''
        
        days.push({
          key: `${date.getFullYear()}-${date.getMonth()}-${date.getDate()}`,
          date: date,
          day: date.getDate(),
          dateString: dateString,
          isOtherMonth,
          isToday,
          isWeekend,
          hasEntry,
          mood
        })
      }
      
      return days
    }
  },
  methods: {
    prevMonth() {
      this.currentDate = new Date(this.currentDate.getFullYear(), this.currentDate.getMonth() - 1, 1)
    },
    nextMonth() {
      this.currentDate = new Date(this.currentDate.getFullYear(), this.currentDate.getMonth() + 1, 1)
    },
    selectDate(day) {
      if (day.isOtherMonth) return
      
      this.selectedDate = day.dateString
      this.showDailyEntry = true
    }
  }
}
</script>

<style scoped>
.life-section {
  padding: 20px;
  height: 100%;
  font-family: 'WenQuanYi-Pixel', 'Courier New', monospace;
}

.calendar-view {
  height: 100%;
  display: flex;
  flex-direction: column;
}

.calendar-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 20px;
  padding: 15px;
  background: #f0f0f0;
  border: 3px solid #333;
}

.calendar-title {
  font-size: 18px;
  color: #333;
  letter-spacing: 2px;
}

.pixel-btn {
  font-family: 'WenQuanYi-Pixel', 'Courier New', monospace;
  font-size: 20px;
  padding: 8px 12px;
  border: 3px solid #333;
  background: white;
  color: #333;
  cursor: pointer;
  transition: all 0.1s;
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

.calendar-grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 2px;
  background: #333;
  border: 3px solid #333;
  flex: 1;
}

.day-header {
  background: #4a90e2;
  color: white;
  padding: 8px;
  text-align: center;
  font-size: 20px;
  font-weight: bold;
}

.day-cell {
  background: white;
  min-height: 60px;
  padding: 5px;
  cursor: pointer;
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  transition: all 0.2s;
  border: 2px solid transparent;
}

.day-cell:hover {
  background: #f0f8ff;
  border-color: #4a90e2;
  transform: scale(1.05);
}

.day-cell.other-month {
  background: #f5f5f5;
  color: #ccc;
  cursor: default;
}

.day-cell.other-month:hover {
  background: #f5f5f5;
  border-color: transparent;
  transform: none;
}

.day-cell.today {
  background: #ffeb3b;
  border-color: #ff9800;
  font-weight: bold;
}

.day-cell.weekend {
  background: #ffe6e6;
}

.day-cell.weekend.today {
  background: #ffeb3b;
}

.day-cell.has-entry {
  background: #e8f5e8;
  border-color: #4caf50;
}

.day-number {
  font-size: 14px;
  font-weight: bold;
}

.entry-indicator {
  color: #4caf50;
  font-size: 20px;
  margin-top: 2px;
}

.day-mood {
  font-size: 30px;
  margin-top: 2px;
}

.calendar-legend {
  display: flex;
  gap: 20px;
  margin-top: 15px;
  padding: 10px;
  background: #f9f9f9;
  border: 2px solid #ddd;
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 5px;
}

.legend-dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  border: 1px solid #333;
}

.today-dot {
  background: #ffeb3b;
}

.entry-dot {
  background: #4caf50;
}

.legend-text {
  font-size: 20px;
  color: #333;
}

/* 响应式设计 */
@media (max-width: 768px) {
  .calendar-header {
    padding: 10px;
  }
  
  .calendar-title {
    font-size: 20px;
  }
  
  .day-cell {
    min-height: 45px;
    padding: 3px;
  }
  
  .day-number {
    font-size: 20px;
  }
  
  .day-mood {
    font-size: 20px;
  }
}
</style>