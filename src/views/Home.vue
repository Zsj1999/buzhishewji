<template>
  <div class="home">
    <div class="banner">
      <swiper :options="swiperOption" class="swiper">
        <swiper-slide v-for="(slide, index) in slides" :key="index">
          <img :src="slide.image" :alt="slide.title">
        </swiper-slide>
        <div class="swiper-pagination" slot="pagination"></div>
      </swiper>
    </div>
    
    <div class="intro-section">
      <h2>不止设计 - 打造理想空间</h2>
      <p>我们专注于室内空间设计，致力于为客户创造舒适、美观、实用的居住环境。</p>
      <el-button type="primary" @click="$router.push('/works')">查看作品</el-button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HomePage',
  data() {
    return {
      swiperOption: {
        pagination: {
          el: '.swiper-pagination',
          clickable: true
        },
        autoplay: {
          delay: 3000
        },
        loop: true
      },
      slides: [
        {
          image: require('../assets/images/banner1.jpg'),
          title: '设计作品展示1'
        },
        {
          image: require('../assets/images/banner2.jpg'),
          title: '设计作品展示2'
        },
        {
          image: require('../assets/images/banner3.jpg'),
          title: '设计作品展示3'
        },
        {
          image: require('../assets/images/banner4.jpg'),
          title: '设计作品展示4'
        },
        
      ]
    }
  }
}
</script>

<style scoped>
.home {
  width: 100%;
}

.banner {
  width: 100%;
  height: 600px;
  position: relative;
}

.swiper {
  width: 100%;
  height: 100%;
}

.swiper img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.intro-section {
  max-width: 1200px;
  margin: 60px auto;
  text-align: center;
  padding: 0 20px;
}

.intro-section h2 {
  font-size: 36px;
  color: #545c64;
  margin-bottom: 20px;
}

.intro-section p {
  font-size: 18px;
  color: #666;
  margin-bottom: 30px;
  line-height: 1.6;
  max-width: 800px;
  margin-left: auto;
  margin-right: auto;
}

@media screen and (max-width: 768px) {
  .banner {
    height: 300px;
  }
  
  .intro-section {
    margin: 30px auto;
  }
  
  .intro-section h2 {
    font-size: 24px;
  }
  
  .intro-section p {
    font-size: 16px;
    padding: 0 15px;
  }

  .el-button {
    width: 100%;
    max-width: 300px;
  }
}
</style> 