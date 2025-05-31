<template>
  <div class="works">
    <div class="works-header">
      <h2>作品展示</h2>
      <p>精选室内设计作品，展现我们的设计理念与实力</p>
    </div>
    
    <div class="works-grid">
      <el-row :gutter="20">
        <el-col :xs="24" :sm="12" :md="8" v-for="(work, index) in works" :key="index">
          <el-card class="work-card" :body-style="{ padding: '0px' }">
            <img :src="work.image" class="work-image">
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
export default {
  name: 'WorksPage',
  data() {
    return {
      works: [
        {
          title: '现代简约风格客厅',
          description: '以简约线条和自然色调打造舒适空间',
          image: 'https://via.placeholder.com/400x300/545c64/ffffff?text=现代简约'
        },
        {
          title: '北欧风格卧室',
          description: '清新淡雅的北欧风格，营造温馨氛围',
          image: 'https://via.placeholder.com/400x300/545c64/ffffff?text=北欧风格'
        },
        {
          title: '工业风餐厅',
          description: '粗犷与精致的完美结合',
          image: 'https://via.placeholder.com/400x300/545c64/ffffff?text=工业风格'
        },
        {
          title: '新中式书房',
          description: '传统与现代的优雅融合',
          image: 'https://via.placeholder.com/400x300/545c64/ffffff?text=新中式'
        },
        {
          title: '轻奢风格客厅',
          description: '精致与奢华的完美诠释',
          image: 'https://via.placeholder.com/400x300/545c64/ffffff?text=轻奢风格'
        },
        {
          title: '日式风格卧室',
          description: '简约禅意的东方美学',
          image: 'https://via.placeholder.com/400x300/545c64/ffffff?text=日式风格'
        }
      ]
    }
  }
}
</script>

<style scoped>
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
}

.work-card:hover {
  transform: translateY(-5px);
}

.work-image {
  width: 100%;
  height: 300px;
  object-fit: cover;
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
}
</style> 