<template>
  <div class="works">
    <div class="works-header">
      <h2>作品展示</h2>
      <p>精选室内设计作品，展现我们的设计理念与实力</p>
    </div>
    <div class="works-grid">
      <el-row :gutter="20">
        <el-col 
          :xs="24" 
          :sm="12" 
          :md="8" 
          v-for="(work, index) in jsonData" 
          :key="index"
        >
          <el-card 
            class="work-card" 
            :body-style="{ padding: '0px' }"
          >
            <div 
              class="video-container"
              @mouseenter="videoStates[index].showControls = true"
              @mouseleave="videoStates[index].showControls = isPlaying[index] ? false : true"
            >
              <video 
                :src="getVideoPath(work.videoSrc)" 
                class="work-image" 
                ref="videoPlayer"
                @play="onPlay(index)"
                @pause="onPause(index)"
                @timeupdate="updateProgress(index)"
                @loadedmetadata="setDuration(index)"
              ></video>
              
              <div 
                class="custom-controls" 
                
                @click.stop
              >
                <button @click="togglePlay(index)">
                  <i :class="videoStates[index].isPlaying ? 'el-icon-video-pause' : 'el-icon-video-play'"></i>
                </button>
                
                <div class="progress-bar" @click="seek($event, index)">
                  <div class="progress" :style="{ width: videoStates[index].progress + '%' }"></div>
                </div>
                
                <span class="time">
                  {{ formatTime(videoStates[index].currentTime) }} / {{ formatTime(videoStates[index].duration) }}
                </span>
                
                <button @click="toggleMute(index)">
                  <i :class="videoStates[index].isMuted ? 'el-icon-close-notification' : 'el-icon-bell'"></i>
                </button>
                
                <button @click="toggleFullscreen(index)">
                  <i class="el-icon-full-screen"></i>
                </button>
              </div>
            </div>

            <div class="work-info">
              <h3>{{ work.title }}</h3>
              <p>{{ work.description }}</p>
            </div>
          </el-card>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
import jsonData from '@/util/shinei.json'

export default {
  name: 'WorksPage',
  data() {
    return {
      jsonData: jsonData.works,
      videoStates: [],
      activeVideoIndex: -1
    }
  },
  created() {
    // 初始化视频状态
    this.videoStates = this.jsonData.map(() => ({
      isPlaying: false,
      isMuted: false,
      showControls: true,
      progress: 0,
      currentTime: 0,
      duration: 0
    }))
  },
  methods: {
    handleCardClick(work) {
      console.log('点击作品:', work.title)
    },
    
    getVideoPath(path) {
      if (!path) return ''
      try {
        const relativePath = path.replace('../assets/', '')
        return require(`@/assets/${relativePath}`)
      } catch (e) {
        console.error('加载视频失败:', e)
        return ''
      }
    },
    
    async togglePlay(index) {
      try {
        const video = this.$refs.videoPlayer[index]
        if (!video) return
        
        // 暂停其他正在播放的视频
        if (this.activeVideoIndex !== -1 && this.activeVideoIndex !== index) {
          const prevVideo = this.$refs.videoPlayer[this.activeVideoIndex]
          if (prevVideo && !prevVideo.paused) {
            prevVideo.pause()
            this.$set(this.videoStates[this.activeVideoIndex], 'isPlaying', false)
            this.$set(this.videoStates[this.activeVideoIndex], 'showControls', true)
          }
        }

        if (video.paused) {
          await video.play()
          this.$set(this.videoStates[index], 'isPlaying', true)
          this.$set(this.videoStates[index], 'showControls', false)
          this.activeVideoIndex = index
        } else {
          video.pause()
          this.$set(this.videoStates[index], 'isPlaying', false)
          this.$set(this.videoStates[index], 'showControls', true)
          this.activeVideoIndex = -1
        }
      } catch (error) {
        console.error('播放失败:', error)
        // 处理浏览器自动播放限制
        if (error.name === 'NotAllowedError') {
          const video = this.$refs.videoPlayer[index]
          video.muted = true
          await video.play()
          this.$set(this.videoStates[index], 'isPlaying', true)
          this.$set(this.videoStates[index], 'isMuted', true)
          this.$set(this.videoStates[index], 'showControls', false)
          this.activeVideoIndex = index
        }
      }
    },
    
    toggleMute(index) {
      const video = this.$refs.videoPlayer[index]
      if (!video) return
      
      video.muted = !video.muted
      this.$set(this.videoStates[index], 'isMuted', video.muted)
    },
    
    toggleFullscreen(index) {
      const videoContainer = this.$refs.videoPlayer[index].parentElement
      if (videoContainer.requestFullscreen) {
        videoContainer.requestFullscreen()
      }
    },
    
    updateProgress(index) {
      const video = this.$refs.videoPlayer[index]
      if (video && video.duration) {
        this.$set(this.videoStates[index], 'progress', (video.currentTime / video.duration) * 100)
        this.$set(this.videoStates[index], 'currentTime', video.currentTime)
      }
    },
    
    setDuration(index) {
      const video = this.$refs.videoPlayer[index]
      if (video) {
        this.$set(this.videoStates[index], 'duration', video.duration)
      }
    },
    
    seek(event, index) {
      const video = this.$refs.videoPlayer[index]
      if (!video) return
      
      const progressBar = event.currentTarget
      const pos = (event.pageX - progressBar.getBoundingClientRect().left) / progressBar.offsetWidth
      video.currentTime = pos * video.duration
    },
    
    onPlay(index) {
      this.$set(this.videoStates[index], 'isPlaying', true)
      this.$set(this.videoStates[index], 'showControls', false)
      this.activeVideoIndex = index
    },
    
    onPause(index) {
      this.$set(this.videoStates[index], 'isPlaying', false)
      this.$set(this.videoStates[index], 'showControls', true)
      this.activeVideoIndex = -1
    },
    
    formatTime(seconds) {
      if (isNaN(seconds)) return "0:00"
      const minutes = Math.floor(seconds / 60)
      seconds = Math.floor(seconds % 60)
      return `${minutes}:${seconds < 10 ? '0' : ''}${seconds}`
    }
  }
}
</script>

<style scoped>
/* 保持原有的样式不变 */
.works {
  padding: 40px 20px;
}

.works-header {
  text-align: center;
  margin-bottom: 40px;
  max-width: 1200px;
  margin-left: auto;
  margin-right: auto;
}

.works-header h2 {
  font-size: 36px;
  color: #545c64;
  margin-bottom: 10px;
}

.works-header p {
  font-size: 18px;
  color: #666;
  max-width: 800px;
  margin: 0 auto;
}

.works-grid {
  max-width: 1200px;
  margin: 0 auto;
}

.work-card {
  margin-bottom: 20px;
  transition: transform 0.3s;
  height: 100%;
  cursor: pointer;
}

.work-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 18px rgba(0, 0, 0, 0.1);
}

.work-info {
  padding: 20px;
}

.work-info h3 {
  font-size: 20px;
  color: #545c64;
  margin-bottom: 10px;
}

.work-info p {
  font-size: 14px;
  color: #666;
  line-height: 1.6;
}

.video-container {
  position: relative;
  width: 100%;
  background-color: #000;
}

.work-image {
  width: 100%;
  height: 300px;
  object-fit: cover;
  display: block;
  cursor: pointer;
}

.custom-controls {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  background: linear-gradient(transparent, rgba(0, 0, 0, 0.7));
  padding: 10px;
  display: flex;
  align-items: center;
  z-index: 10;
  transition: opacity 0.3s;
}

.custom-controls button {
  background: none;
  border: none;
  color: white;
  font-size: 16px;
  margin-right: 10px;
  cursor: pointer;
  outline: none;
  opacity: 0.8;
  transition: opacity 0.2s;
}

.custom-controls button:hover {
  opacity: 1;
  color: #409EFF;
}

.progress-bar {
  flex-grow: 1;
  height: 4px;
  background: rgba(255, 255, 255, 0.2);
  margin: 0 10px;
  cursor: pointer;
  border-radius: 2px;
  position: relative;
}

.progress-bar:hover {
  height: 6px;
}

.progress {
  height: 100%;
  background: #409EFF;
  border-radius: 2px;
  width: 0;
  transition: width 0.1s;
}

.time {
  color: white;
  font-size: 12px;
  margin: 0 10px;
  font-family: Arial, sans-serif;
  min-width: 90px;
  text-align: center;
}

@media screen and (max-width: 768px) {
  .works {
    padding: 20px 15px;
  }

  .works-header {
    margin-bottom: 20px;
  }
  
  .works-header h2 {
    font-size: 24px;
  }
  
  .works-header p {
    font-size: 16px;
    padding: 0 15px;
  }
  
  .work-image {
    height: 200px;
  }

  .work-info {
    padding: 15px;
  }

  .work-info h3 {
    font-size: 18px;
  }

  .work-info p {
    font-size: 14px;
  }

  .custom-controls {
    padding: 8px;
  }

  .custom-controls button {
    font-size: 14px;
    margin-right: 5px;
  }

  .time {
    font-size: 10px;
    min-width: 70px;
  }
}
</style>