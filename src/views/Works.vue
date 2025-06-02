<template>
  <div class="works">
    <div class="works-header">
      <h2>作品展示</h2>
      <p>精选室内设计作品，展现我们的设计理念与实力</p>
    </div>
    <div class="works-grid">
      <el-row :gutter="20">
        <el-col :xs="24" :sm="12" :md="8" v-for="(work, index) in jsonData" :key="index">
          <el-card class="work-card" :body-style="{ padding: '0px' }" @click.native="handleCardClick(work)">
            <div class="video-container" @mouseenter="showControls[index] = true"
              @mouseleave="showControls[index] = false">
              <video :src="getVideoPath(work.videoSrc)" class="work-image" ref="videoPlayer" @play="onPlay(index)"
                @pause="onPause(index)" @timeupdate="updateProgress(index)"
                @loadedmetadata="setDuration(index)"></video>
              <div class="custom-controls" @click.stop>
                <button @click="togglePlay(index)">
                  <i :class="isPlaying[index] ? 'el-icon-video-pause' : 'el-icon-video-play'"></i>
                </button>

                <div class="progress-bar" @click="seek($event, index)">
                  <div class="progress" :style="{ width: progress[index] + '%' }"></div>
                </div>
                <span class="time">{{ formatTime(currentTime[index]) }} / {{ formatTime(duration[index]) }}</span>
                <button @click="toggleMute(index)">
                  <i :class="isMuted[index] ? 'el-icon-close-notification' : 'el-icon-bell'"></i>
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
      isPlaying: [],
      isMuted: [],
      progress: [],
      currentTime: [],
      duration: [],
      showControls: [],
      activeVideoIndex: -1,
    }
  },
  created() {
    // 初始化状态数组
    this.jsonData.forEach(() => {
      this.isPlaying.push(false)
      this.isMuted.push(false)
      this.progress.push(0)
      this.currentTime.push(0)
      this.duration.push(0)
      this.showControls.push(false)
    })
  },
  methods: {
    handleCardClick(work) {
      console.log('点击作品:', work.title)
    },
    getVideoPath(path) {
      if (!path) return ''
      try {
        return require(`@/assets/${path.replace('../assets/', '')}`)
      } catch (e) {
        console.error('加载视频失败:', e)
        return ''
      }
    },

    mounted() {
      // 初始化数组（根据视频数量）
      this.initVideoStates(this.videos.length); // 假设videos是你的视频列表
    },
    methods: {
      initVideoStates(count) {
        for (let i = 0; i < count; i++) {
          this.$set(this.isPlaying, i, false);
          this.$set(this.showControls, i, true);
        }
      },
      togglePlay(index) {
        const video = this.$refs.videoPlayer[index];
        if (!video) return;
        try {
          // 暂停其他视频
          if (this.activeVideoIndex !== -1 && this.activeVideoIndex !== index) {
            const prevVideo = this.$refs.videoPlayer[this.activeVideoIndex];
            if (prevVideo && !prevVideo.paused) {
              prevVideo.pause();
              this.$set(this.isPlaying, this.activeVideoIndex, false);
              this.$set(this.showControls, this.activeVideoIndex, true);
            }
          }

          if (video.paused) {
            const onPlay = () => {
              this.$set(this.isPlaying, index, true);
              this.$set(this.showControls, index, false);
              this.activeVideoIndex = index;
              video.removeEventListener('play', onPlay);
            };

            video.addEventListener('play', onPlay);
            video.play().catch(err => {
              console.error('播放失败:', err);
              this.handlePlayError(index);
            });
          } else {
            const onPause = () => {
              this.$set(this.isPlaying, index, false);
              this.$set(this.showControls, index, true);
              this.activeVideoIndex = -1;
              video.removeEventListener('pause', onPause);
            };

            video.addEventListener('pause', onPause);
            video.pause();
          }
        } catch (error) {
          console.error('播放控制失败:', error);
          this.handlePlayError(index);
        }
      },
      handlePlayError(index) {
        this.$set(this.isPlaying, index, false);
        this.$set(this.showControls, index, true);
        this.activeVideoIndex = -1;
      }
    },
    togglePlay(index) {
      const video = this.$refs.videoPlayer[index]
      if (!video) return
      try {
        // 如果有其他视频正在播放，先暂停它
        if (this.activeVideoIndex !== -1 && this.activeVideoIndex !== index) {
          const prevVideo = this.$refs.videoPlayer[this.activeVideoIndex]
          if (prevVideo && !prevVideo.paused) {
            prevVideo.pause()
            this.$set(this.isPlaying, this.activeVideoIndex, false)
            this.$set(this.showControls, this.activeVideoIndex, true)
          }
        }

        // 处理当前视频播放/暂停
        if (video.paused) {
          const onPlay = () => {
            this.$set(this.isPlaying, index, true)
            this.$set(this.showControls, index, false)
            this.activeVideoIndex = index // 更新活跃视频索引
            video.removeEventListener('play', onPlay)
          }

          video.addEventListener('play', onPlay)
          video.play().catch(err => {
            console.error('播放失败:', err)
            this.$set(this.isPlaying, index, false)
            this.$set(this.showControls, index, true)
            this.activeVideoIndex = -1 // 播放失败时重置
          })
        } else {
          const onPause = () => {
            this.$set(this.isPlaying, index, false)
            this.$set(this.showControls, index, true)
            this.activeVideoIndex = -1 // 视频暂停时重置
            video.removeEventListener('pause', onPause)
          }

          video.addEventListener('pause', onPause)
          video.pause()
        }
      } catch (error) {
        console.error('播放控制失败:', error)
        this.$set(this.isPlaying, index, false)
        this.$set(this.showControls, index, true)
        this.activeVideoIndex = -1
      }
    },
    toggleMute(index) {
      const video = this.$refs.videoPlayer[index]
      if (!video) return

      try {
        // 先更新视频状态
        video.muted = !video.muted

        // 使用 $set 确保响应式更新
        this.$set(this.isMuted, index, video.muted)

        // 可选：添加音频状态变化的视觉反馈
        this.$nextTick(() => {
          console.log(`音频已${video.muted ? '静音' : '取消静音'}`)
        })
      } catch (error) {
        console.error('切换静音失败:', error)
        // 出错时重置状态
        this.$set(this.isMuted, index, video.muted)
      }
    },
    toggleFullscreen(index) {
      const videoContainer = this.$refs.videoPlayer[index].parentElement
      if (videoContainer.requestFullscreen) {
        videoContainer.requestFullscreen()
      }
    },
    updateProgress(index) {
      const video = this.$refs.videoPlayer[index]
      if (video.duration) {
        this.progress[index] = (video.currentTime / video.duration) * 100
        this.currentTime[index] = video.currentTime
      }
    },
    setDuration(index) {
      const video = this.$refs.videoPlayer[index]
      this.duration[index] = video.duration
    },
    seek(event, index) {
      const video = this.$refs.videoPlayer[index]
      const progressBar = event.currentTarget
      const pos = (event.pageX - progressBar.getBoundingClientRect().left) / progressBar.offsetWidth
      video.currentTime = pos * video.duration
    },
    onPlay(index) {
      this.isPlaying[index] = true
      this.showControls[index] = false
    },
    onPause(index) {
      this.isPlaying[index] = false
      this.showControls[index] = true
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
/* 确保这些关键样式存在 */
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
  opacity: 1;
  /* 确保不透明 */
  transition: opacity 0.3s;
}

/* 其他保持原有样式不变 */
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