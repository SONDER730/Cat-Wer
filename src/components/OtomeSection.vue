<template>
  <div class="otome-section">
    <!-- 左侧人物档案切换（可收起） -->
    <div :class="['character-list', { 'collapsed': isCollapsed }]">
      <!-- 展开/收起按钮 -->
      <button class="toggle-btn pixel-btn" @click="toggleCollapse">
        <span v-if="isCollapsed">👥</span>
        <span v-else>←</span>
      </button>
      
      <!-- 人物档案内容 -->
      <div v-if="!isCollapsed" class="character-content">
        <h3 class="list-title">人物档案</h3>
        <div class="character-tabs">
          <button
            v-for="(character, index) in characters"
            :key="character.id"
            :class="['character-tab', { 'active': selectedCharacter === index }]"
            @click="selectCharacter(index)"
          >
            <span class="tab-icon">{{ character.icon }}</span>
            <span class="tab-name">{{ character.name }}</span>
          </button>
        </div>
      </div>
    </div>

    <!-- 右侧人物详情 -->
    <div class="character-detail">
      <div class="character-card">
        <!-- 人物图片 -->
        <div class="character-image">
          <img 
            :src="currentCharacterImage" 
            :alt="currentCharacterData.name"
            @error="handleImageError"
          />
        </div>

        <!-- 人物信息 -->
        <div class="character-info">
          <h2 class="character-name">{{ currentCharacterData.name }}</h2>
          
          <div class="character-details">
            <div class="detail-item">
              <span class="detail-label">生日:</span>
              <span class="detail-value">{{ currentCharacterData.birthday }}</span>
            </div>
            
            <div class="character-description">
              <h4 class="desc-title">人物介绍</h4>
              <p class="desc-text">{{ currentCharacterData.description }}</p>
            </div>

            <div class="character-quote">
              <h4 class="quote-title">人物语录</h4>
              <div class="quote-content">
                <p class="quote-text">"{{ currentCharacterData.quote }}"</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'OtomeSection',
  data() {
    return {
      selectedCharacter: 0,
      isCollapsed: true, // 默认收起状态
      characters: [
        {
          id: 'xiaoyi',
          name: '萧逸',
          icon: '⚔️',
          birthday: '11月23日',
          description: '“他在阳光下眯起的笑眼，是暗夜中果决的火焰。浓烈而危险的漆黑，也潜藏着不为人知的无邪。他将酸甜盛夏烙印进无限的以后，或许只为等待你的出现。”',
          quote: `两个世界又有什么关系。
                    未来还那么长，我们一定会再见的。
                    我虽然一直往前走，心却留在了身后。
                    有你在的地方，才是我的故乡，我永远的终点。
                    赛车的终点是赛道的尽头，而我的终点是你的身边。
                    就算世间纷纷扰扰，也挡不住我要在下一秒赶到你的面前。`
        },
        {
          id: 'liubian',
          name: '刘辩',
          icon: '👑',
          birthday: '2月28日',
          description: '天子刘辩，幼年在隐鸢阁与殿下相识，两小无猜一起长大，十四岁时，他被接回宫中。回到宫中的刘辩，从数次毒杀与嫁祸中幸存，终日如履薄冰。儿时的那段岁月，成为他人生中最明亮无瑕的一段时光。',
          quote: `我想成为你的之死靡它。
                    整个天下，我都送给我钟情的人。
                    下次相见，希望是个无风无雨的日子。
                    只为了那一个名字，我就可以不顾一切。
                    你要替我把这个人找来，送到我的臂弯里来。
                    如果神志不清能这样抱着你，那我宁愿疯一辈子。`
        },
        {
          id: 'qiyu',
          name: '祁煜',
          icon: '🔮',
          birthday: '3月6日',
          description: '一位神祗般的祁煜也会为了爱人甘愿牺牲自己去拯救万千世界，诚然他绝对是一个出色的神明，从来没有对不起过他的子民也没有对不起过自己所爱的人，爱他的人在多面小鱼里总爱挑选自己最爱的一个，有的人会选温柔沉稳的金沙鱼，有的会选天真洒脱的小海神，而有的人则钟爱经历了时光荏苒却仍然深爱不疑的临空鱼，不管是哪一个祁煜，他都好似完美恋人让人不忍拒绝。',
          quote: `祝你想笑的时候就笑，想哭的时候就哭，永远自由惬意，无拘无束。还祝你每天睡到自然醒，每一餐都吃的心满意足，刮刮乐时永远不会只有“谢谢参与”。
                  
                  如果只是为了让它看起来完美无缺而忽视它内在的生命力，那它和标本也没什么区别。`
        },
        {
          id: 'xiaxiaoyin',
          name: '夏萧因',
          icon: '🌸',
          birthday: '9月10日',
          description: '权利滔天的摄政王，威震四海的海皇，强大凌厉的星际警署高位，掌管怠惰的恶魔，人人畏惧的西湖蛇妖…这样强大的人怎会是柔弱的？强势是他性格的底色，温柔只是锦上添花的佐料。',
          quote: `我才不要做早走的那个，自己的妻子，当然要自己守一辈子。
          如果有一天，你打开了所有的门，或许你就会知道，我爱你。
          每一个亲吻过海洋的人，最终都会回到海洋的怀抱。
          我认定是你，那就是你了。`
        },
        {
          id: 'xiayizhou',
          name: '夏以昼',
          icon: '☀️',
          birthday: '6月13日',
          description: `远空舰队新任执舰官，背景和行踪颇为机密,舰队内外的各方势力都在等待他露出破绽。
                        右臂被强化改造后可以转换为高密度金属，唯有超过阈值的痛觉才能被他感知。
                        据传原为深空航天署的战斗飞行员，目前DAA仍保留有他的痕迹。`,
          quote: `我总会想到哄你的办法，只要你还在我身边。
                  那你也拥有一点罪，好不好，别让我太孤独了。
                  你第一次拉住我的手的那一天，我就已经跑不掉了。
                  无论发生什么，我都会陪你一起。
                  也许因为，我恰好也比你想象中的多爱你一点。
                  如果我像这样把你留在身边，你会觉得我太自私了吗。
                  你想要回临空，我们就回临空。你想回到从前，我们就把老宅翻修，一起住回去。一座房子不够，那就给你建一座迷宫。我会在里面给你准备最好的一切，把它建成世界上最漂亮的花园。有我陪着，以后他们就再也找不到你了。
                  牵着我的手，就不怕走丢了。`
        }
      ]
    }
  },
  computed: {
    currentCharacterData() {
      return this.characters[this.selectedCharacter]
    },
    currentCharacterImage() {
      return `/Cat-Wer/images/Figure${this.selectedCharacter}.png`
    }
  },
  methods: {
    selectCharacter(index) {
      this.selectedCharacter = index
    },
    toggleCollapse() {
      this.isCollapsed = !this.isCollapsed
    },
    handleImageError() {
      // 如果图片加载失败，可以显示默认图片或占位符
      console.log('图片加载失败:', this.currentCharacterImage)
    }
  }
}
</script>

<style scoped>
.otome-section {
  display: flex;
  height: 100%;
  font-family: 'WenQuanYi-Pixel', 'Courier New', monospace;
  gap: 20px;
  padding: 20px;
}

/* 左侧人物列表（可收起） */
.character-list {
  width: 220px;
  background: #f8f9fa;
  border: 3px solid #333;
  padding: 15px;
  position: relative;
  transition: width 0.3s ease, padding 0.3s ease;
  overflow: hidden;
}

.character-list.collapsed {
  width: 60px;
  padding: 10px;
}

.toggle-btn {
  position: absolute;
  top: 15px;
  right: 15px;
  z-index: 10;
  width: 30px;
  height: 30px;
  padding: 0;
  font-size: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.character-list.collapsed .toggle-btn {
  top: 15px;
  right: 10px;
  left: 10px;
  width: 40px;
}

.character-content {
  transition: opacity 0.3s ease;
}

.character-list.collapsed .character-content {
  opacity: 0;
  pointer-events: none;
}

.pixel-btn {
  font-family: 'WenQuanYi-Pixel', 'Courier New', monospace;
  border: 2px solid #333;
  background: white;
  color: #333;
  cursor: pointer;
  transition: all 0.1s;
  font-size: 10px;
}

.pixel-btn:hover {
  background: #333;
  color: white;
  transform: translate(-1px, -1px);
  box-shadow: 1px 1px 0px #666;
}

.pixel-btn:active {
  transform: translate(0, 0);
  box-shadow: none;
}

.list-title {
  font-size: 14px;
  color: #333;
  margin-bottom: 15px;
  text-align: center;
  border-bottom: 2px solid #666;
  padding-bottom: 8px;
}

.character-tabs {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.character-tab {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 10px;
  border: 2px solid #ddd;
  background: white;
  cursor: pointer;
  transition: all 0.2s;
  font-family: inherit;
  text-align: left;
}

.character-tab:hover {
  border-color: #333;
  background: #f0f8ff;
  transform: translateX(3px);
}

.character-tab.active {
  border-color: #007bff;
  background: #e7f3ff;
  transform: translateX(5px);
}

.tab-icon {
  font-size: 16px;
}

.tab-name {
  font-size: 11px;
  color: #333;
}

/* 右侧人物详情 */
.character-detail {
  flex: 1;
  overflow-y: auto;
}

.character-card {
  background: white;
  border: 3px solid #333;
  display: grid;
  grid-template-columns: 300px 1fr;
  min-height: 100%;
}

.character-image {
  border-right: 3px solid #333;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #f5f5f5;
  padding: 4px;
}

.character-image img {
  max-width: 100%;
  max-height: 400px;
  object-fit: cover;
  border: 2px solid #333;
}

.character-info {
  padding: 5px;
  display: flex;
  flex-direction: column;
  gap: 5px;
}

.character-name {
  font-size: 20px;
  color: #333;
  text-align: center;
  border-bottom: 3px solid #333;
  padding-bottom: 10px;
  margin-bottom: 25px;
}

.character-details {
  display: flex;
  flex-direction: column;
  gap: 25px;
}

.detail-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px;
  background: #f9f9f9;
  border: 2px solid #ddd;
}

.detail-label {
  font-size: 12px;
  color: #666;
  font-weight: bold;
  min-width: 50px;
}

.detail-value {
  font-size: 14px;
  color: #333;
}

.character-description {
  background: #f8f9fa;
  border: 2px solid #ddd;
  padding: 15px;
}

.desc-title {
  font-size: 12px;
  color: #333;
  margin-bottom: 8px;
}

.desc-text {
  font-size: 11px;
  color: #555;
  line-height: 1.4;
}

.character-quote {
  background: #fffbf0;
  border: 2px solid #ffa500;
  padding: 20px;
  text-align: center;
}

.quote-title {
  font-size: 12px;
  color: #333;
  margin-bottom: 15px;
}

.quote-content {
  position: relative;
}

.quote-text {
  font-size: 14px;
  color: #555;
  line-height: 1.6;
  font-style: italic;
  position: relative;
  white-space: pre-line;
}

.quote-content::before {
  content: """;
  font-size: 40px;
  color: #ffa500;
  position: absolute;
  left: -10px;
  top: -10px;
}

.quote-content::after {
  content: """;
  font-size: 40px;
  color: #ffa500;
  position: absolute;
  right: -10px;
  bottom: -25px;
}

/* 响应式设计 */
@media (max-width: 768px) {
  .otome-section {
    flex-direction: column;
  }
  
  .character-list {
    width: 100%;
    padding: 10px;
  }
  
  .character-list.collapsed {
    width: 100%;
    height: 50px;
  }
  
  .toggle-btn {
    width: 40px;
    height: 30px;
  }
  
  .character-list.collapsed .toggle-btn {
    width: 80px;
    margin: 0 auto;
  }
  
  .character-tabs {
    flex-direction: row;
    overflow-x: auto;
    gap: 5px;
  }
  
  .character-tab {
    min-width: 100px;
    flex-shrink: 0;
  }
  
  .character-card {
    grid-template-columns: 1fr;
    grid-template-rows: auto 1fr;
  }
  
  .character-image {
    border-right: none;
    border-bottom: 3px solid #333;
    padding: 15px;
  }
  
  .character-image img {
    max-height: 250px;
  }
  
  .character-details {
    gap: 15px;
  }
}
</style>