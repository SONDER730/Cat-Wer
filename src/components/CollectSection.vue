<template>
  <div class="collect-section">
    <!-- 标题区域 -->
    <div class="section-header">
      <h2 class="section-title">
        <span class="title-icon">📚</span>
        收藏癖就是什么都会堆在一起
      </h2>
      <div class="bookshelf-stats">
        <span class="stat-item">🎬 {{ totalBooks }} 个作品</span>
        <span class="stat-item">🏷️ {{ totalCategories }} 个分类</span>
        <span class="stat-item">⭐ {{ favoriteBooks }} 个收藏</span>
      </div>
    </div>

    <!-- 书柜主体 -->
    <div class="bookshelf-container">
      <!-- 书柜框架 -->
      <div class="bookshelf-frame">
        <!-- 第一层书架 -->
        <div class="shelf-row">
          <div class="shelf-board"></div>
          <div class="books-row">
            <div
              v-for="book in shelf1Books"
              :key="book.id"
              :class="['book', `book-${book.color}`, { 'favorite': book.isFavorite }]"
              :style="{ height: book.height + 'px' }"
              @click="selectBook(book)"
              @mouseenter="showBookTooltip(book, $event)"
              @mouseleave="hideTooltip"
            >
              <div class="book-spine">
                <span class="book-title">{{ book.title }}</span>
                <span class="book-author">{{ book.author }}</span>
              </div>
              <div v-if="book.isFavorite" class="favorite-star">⭐</div>
            </div>
          </div>
        </div>

        <!-- 第二层书架 -->
        <div class="shelf-row">
          <div class="shelf-board"></div>
          <div class="books-row">
            <div
              v-for="book in shelf2Books"
              :key="book.id"
              :class="['book', `book-${book.color}`, { 'favorite': book.isFavorite }]"
              :style="{ height: book.height + 'px' }"
              @click="selectBook(book)"
              @mouseenter="showBookTooltip(book, $event)"
              @mouseleave="hideTooltip"
            >
              <div class="book-spine">
                <span class="book-title">{{ book.title }}</span>
                <span class="book-author">{{ book.author }}</span>
              </div>
              <div v-if="book.isFavorite" class="favorite-star">⭐</div>
            </div>
          </div>
        </div>

        <!-- 第三层书架 -->
        <div class="shelf-row">
          <div class="shelf-board"></div>
          <div class="books-row">
            <div
              v-for="book in shelf3Books"
              :key="book.id"
              :class="['book', `book-${book.color}`, { 'favorite': book.isFavorite }]"
              :style="{ height: book.height + 'px' }"
              @click="selectBook(book)"
              @mouseenter="showBookTooltip(book, $event)"
              @mouseleave="hideTooltip"
            >
              <div class="book-spine">
                <span class="book-title">{{ book.title }}</span>
                <span class="book-author">{{ book.author }}</span>
              </div>
              <div v-if="book.isFavorite" class="favorite-star">⭐</div>
            </div>
            <!-- 书架装饰品 -->
            <div class="shelf-decoration">🎨</div>
          </div>
        </div>

        <!-- 第四层书架 -->
        <div class="shelf-row">
          <div class="shelf-board"></div>
          <div class="books-row">
            <div
              v-for="book in shelf4Books"
              :key="book.id"
              :class="['book', `book-${book.color}`, { 'favorite': book.isFavorite }]"
              :style="{ height: book.height + 'px' }"
              @click="selectBook(book)"
              @mouseenter="showBookTooltip(book, $event)"
              @mouseleave="hideTooltip"
            >
              <div class="book-spine">
                <span class="book-title">{{ book.title }}</span>
                <span class="book-author">{{ book.author }}</span>
              </div>
              <div v-if="book.isFavorite" class="favorite-star">⭐</div>
            </div>
            <!-- 更多装饰品 -->
            <div class="shelf-decoration">🎭</div>
            <div class="shelf-decoration">🎮</div>
          </div>
        </div>
      </div>

      <!-- 书柜底部 -->
      <div class="bookshelf-base">
        <div class="base-decorations">
          <span class="decoration-item">🎬</span>
          <span class="decoration-item">🎨</span>
          <span class="decoration-item">🎮</span>
          <span class="decoration-item">📚</span>
        </div>
      </div>
    </div>

    <!-- 书籍详情面板 -->
    <div v-if="selectedBook" class="book-detail-panel">
      <div class="detail-header">
        <h3 class="detail-title">{{ selectedBook.title }}</h3>
        <button class="close-btn pixel-btn" @click="closeBookDetail">×</button>
      </div>
      <div class="detail-content">
        <div class="detail-info">
          <p><strong>作者:</strong> {{ selectedBook.author }}</p>
          <p><strong>分类:</strong> {{ selectedBook.category }}</p>
          <p><strong>状态:</strong> {{ selectedBook.status }}</p>
          <p><strong>收藏时间:</strong> {{ selectedBook.addedDate }}</p>
        </div>
        <div class="detail-description">
          <h4>简介</h4>
          <p>{{ selectedBook.description }}</p>
        </div>
        <div class="detail-actions">
          <button 
            class="pixel-btn" 
            :class="{ 'favorite-btn': selectedBook.isFavorite }"
            @click="toggleFavorite(selectedBook)"
          >
            {{ selectedBook.isFavorite ? '❤️ 已收藏' : '🤍 加入收藏' }}
          </button>
        </div>
      </div>
    </div>

    <!-- 书籍浮窗提示 -->
    <div 
      v-if="tooltip.visible" 
      class="book-tooltip"
      :style="{ left: tooltip.x + 'px', top: tooltip.y + 'px' }"
    >
      <div class="tooltip-content">
        <h4>{{ tooltip.data.title }}</h4>
        <p>{{ tooltip.data.author }}</p>
        <span class="tooltip-category">{{ tooltip.data.category }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CollectSection',
  data() {
    return {
      selectedBook: null,
      tooltip: {
        visible: false,
        x: 0,
        y: 0,
        data: {}
      },
      
      // 收藏作品数据
      books: [
        // 第一层书架 - 漫画
        // { id: 1, title: '进击的巨人', author: '谏山创', category: '漫画', status: '已看完', color: 'red', height: 180, addedDate: '2024-01-15', isFavorite: true, shelf: 1,
        //   description: '人类与巨人的生存战争，充满震撼的黑暗奇幻漫画。' },

      ]
    }
  },
  computed: {
    shelf1Books() {
      return this.books.filter(book => book.shelf === 1)
    },
    shelf2Books() {
      return this.books.filter(book => book.shelf === 2)
    },
    shelf3Books() {
      return this.books.filter(book => book.shelf === 3)
    },
    shelf4Books() {
      return this.books.filter(book => book.shelf === 4)
    },
    totalBooks() {
      return this.books.length
    },
    totalCategories() {
      return [...new Set(this.books.map(book => book.category))].length
    },
    favoriteBooks() {
      return this.books.filter(book => book.isFavorite).length
    }
  },
  methods: {
    selectBook(book) {
      this.selectedBook = book
      this.hideTooltip()
    },
    
    closeBookDetail() {
      this.selectedBook = null
    },
    
    toggleFavorite(book) {
      book.isFavorite = !book.isFavorite
    },
    
    showBookTooltip(book, event) {
      this.tooltip = {
        visible: true,
        x: event.clientX + 10,
        y: event.clientY - 10,
        data: book
      }
    },
    
    hideTooltip() {
      this.tooltip.visible = false
    }
  }
}
</script>

<style scoped>
.collect-section {
  height: 100%;
  font-family: 'WenQuanYi-Pixel', 'Courier New', monospace;
  background: linear-gradient(135deg, #1e3a8a 0%, #0ea5e9 50%, #87b6eb 100%);
  position: relative;
  overflow: hidden;
}

/* 标题区域 */
.section-header {
  padding: 20px;
  text-align: center;
  background: rgba(30, 58, 138, 0.9);
  border-bottom: 4px solid #1e40af;
  color: #e0f2fe;
}

.section-title {
  font-size: 16px;
  margin-bottom: 15px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
}

.title-icon {
  font-size: 20px;
}

.bookshelf-stats {
  display: flex;
  justify-content: center;
  gap: 20px;
  font-size: 10px;
}

.stat-item {
  background: rgba(30, 64, 175, 0.7);
  padding: 5px 10px;
  border: 2px solid #1e40af;
}

/* 书柜容器 */
.bookshelf-container {
  flex: 1;
  padding: 20px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

/* 书柜框架 */
.bookshelf-frame {
  width: 100%;
  max-width: 800px;
  background: linear-gradient(90deg, #1e40af, #84afe4);
  border: 6px solid #1e3a8a;
  padding: 10px;
  border-radius: 10px;
  box-shadow: inset 0 0 20px rgba(0,0,0,0.3);
}

/* 书架层 */
.shelf-row {
  margin-bottom: 15px;
  position: relative;
}

.shelf-board {
  height: 8px;
  background: linear-gradient(90deg, #1e3a8a, #1e40af);
  border: 2px solid #0f172a;
  border-radius: 4px;
  margin-bottom: 5px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.3);
}

.books-row {
  display: flex;
  align-items: flex-end;
  gap: 8px;
  padding: 0 10px;
  min-height: 240px;
}

/* 书籍样式 */
.book {
  width: 45px;
  position: relative;
  cursor: pointer;
  transition: all 0.3s ease;
  border: 2px solid #333;
  border-radius: 0 0 3px 3px;
  box-shadow: 2px 2px 8px rgba(0,0,0,0.3);
}

.book:hover {
  transform: translateY(-5px) scale(1.05);
  z-index: 10;
}

.book-spine {
  height: 100%;
  padding: 8px 4px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  writing-mode: vertical-rl;
  text-orientation: mixed;
}

.book-title {
  font-size: 9px;
  font-weight: bold;
  color: white;
  text-shadow: 1px 1px 2px rgba(0,0,0,0.7);
  overflow: hidden;
  text-overflow: ellipsis;
}

.book-author {
  font-size: 7px;
  color: rgba(255,255,255,0.8);
  margin-top: 5px;
}

.favorite-star {
  position: absolute;
  top: -5px;
  right: -5px;
  font-size: 12px;
  animation: sparkle 2s ease-in-out infinite;
}

@keyframes sparkle {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.2); }
}

/* 书籍颜色 */
.book-blue { background: linear-gradient(135deg, #2563eb, #1d4ed8); }
.book-pink { background: linear-gradient(135deg, #ec4899, #db2777); }
.book-brown { background: linear-gradient(135deg, #a16207, #92400e); }
.book-gray { background: linear-gradient(135deg, #4b5563, #374151); }
.book-cyan { background: linear-gradient(135deg, #0891b2, #0e7490); }
.book-navy { background: linear-gradient(135deg, #1e3a8a, #1e40af); }
.book-black { background: linear-gradient(135deg, #1f2937, #111827); }

/* 书架装饰 */
.shelf-decoration {
  font-size: 24px;
  margin: 0 10px;
  animation: gentle-sway 4s ease-in-out infinite;
}

@keyframes gentle-sway {
  0%, 100% { transform: rotate(-2deg); }
  50% { transform: rotate(2deg); }
}

/* 书柜底部 */
.bookshelf-base {
  width: 100%;
  max-width: 800px;
  margin-top: 20px;
  background: linear-gradient(90deg, #1e3a8a, #1e40af);
  border: 4px solid #0f172a;
  padding: 15px;
  border-radius: 8px;
}

.base-decorations {
  display: flex;
  justify-content: space-around;
  align-items: center;
}

.decoration-item {
  font-size: 20px;
  padding: 8px;
  background: rgba(224, 242, 254, 0.2);
  border: 2px solid #1e3a8a;
  border-radius: 4px;
}

/* 书籍详情面板 */
.book-detail-panel {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: #e0f2fe;
  border: 4px solid #0ea5e9;
  padding: 20px;
  max-width: 400px;
  width: 90%;
  z-index: 100;
  border-radius: 8px;
  box-shadow: 0 8px 32px rgba(0,0,0,0.5);
}
.detail-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
  padding-bottom: 10px;
  border-bottom: 2px solid #0ea5e9;
}

.detail-title {
  color: #1e3a8a;
  font-size: 14px;
}

.close-btn {
  width: 30px;
  height: 30px;
  padding: 0;
  font-size: 16px;
}

.detail-content {
  color: #1e40af;
}

.detail-info {
  margin-bottom: 15px;
}

.detail-info p {
  margin-bottom: 5px;
  font-size: 10px;
}

.detail-description {
  margin-bottom: 15px;
}

.detail-description h4 {
  margin-bottom: 8px;
  font-size: 11px;
  color: #1e3a8a;
}

.detail-description p {
  font-size: 9px;
  line-height: 1.4;
}

.detail-actions {
  text-align: center;
}

.pixel-btn {
  font-family: 'WenQuanYi-Pixel', 'Courier New', monospace;
  padding: 8px 12px;
  border: 3px solid #0ea5e9;
  background: #e0f2fe;
  color: #1e3a8a;
  cursor: pointer;
  transition: all 0.1s;
  font-size: 10px;
}

.pixel-btn:hover {
  background: #0ea5e9;
  color: #e0f2fe;
  transform: translate(-1px, -1px);
  box-shadow: 1px 1px 0px #0284c7;
}

.favorite-btn {
  background: #fecaca;
  border-color: #ef4444;
  color: #dc2626;
}

/* 书籍浮窗 */
.book-tooltip {
  position: fixed;
  z-index: 1000;
  pointer-events: none;
  max-width: 200px;
}

.tooltip-content {
  background: rgba(30, 58, 138, 0.95);
  color: #e0f2fe;
  padding: 10px;
  border: 2px solid #1e40af;
  font-size: 9px;
  border-radius: 4px;
}

.tooltip-content h4 {
  margin-bottom: 4px;
  font-size: 10px;
  color: #7dd3fc;
}

.tooltip-content p {
  margin-bottom: 4px;
  color: #e0f2fe;
}

.tooltip-category {
  background: #1e40af;
  padding: 2px 4px;
  font-size: 7px;
  border-radius: 2px;
}

/* 响应式设计 */
@media (max-width: 768px) {
  .bookshelf-frame {
    max-width: 100%;
    padding: 5px;
  }
  
  .books-row {
    gap: 4px;
    padding: 0 5px;
    min-height: 180px;
  }
  
  .book {
    width: 35px;
  }
  
  .book-title {
    font-size: 7px;
  }
  
  .book-author {
    font-size: 6px;
  }
  
  .shelf-decoration {
    font-size: 18px;
    margin: 0 5px;
  }
  
  .book-detail-panel {
    width: 95%;
    padding: 15px;
  }
}
</style>