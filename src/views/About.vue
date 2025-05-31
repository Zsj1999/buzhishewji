<template>
  <div class="about">
    <div class="about-header">
      <h2>关于我们</h2>
      <p>不止设计 - 专注室内空间设计</p>
    </div>
    
    <div class="about-content">
      <div class="company-intro">
        <h3>公司简介</h3>
        <p>不止设计成立于2020年，是一家专注于室内空间设计的专业工作室。我们致力于为客户提供高品质的室内设计服务，打造舒适、美观、实用的居住环境。</p>
        <p>我们的设计理念是：将艺术与实用完美结合，让每个空间都能讲述独特的故事。</p>
      </div>
      
      <div class="our-advantages">
        <h3>我们的优势</h3>
        <el-row :gutter="20">
          <el-col :xs="24" :sm="12" :md="8" v-for="(advantage, index) in advantages" :key="index">
            <div class="advantage-item">
              <i :class="advantage.icon"></i>
              <h4>{{ advantage.title }}</h4>
              <p>{{ advantage.description }}</p>
            </div>
          </el-col>
        </el-row>
      </div>
      
      <div class="our-team">
        <h3>设计团队</h3>
        <el-row :gutter="20">
          <el-col :xs="24" :sm="12" :md="8" v-for="(member, index) in team" :key="index">
            <el-card class="team-card" :body-style="{ padding: '0px' }">
              <img :src="member.avatar" class="team-avatar">
              <div class="team-info">
                <h4>{{ member.name }}</h4>
                <p>{{ member.position }}</p>
                <p class="team-desc">{{ member.description }}</p>
              </div>
            </el-card>
          </el-col>
        </el-row>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AboutPage',
  data() {
    return {
      advantages: [
        {
          icon: 'el-icon-star-on',
          title: '专业设计团队',
          description: '拥有多年设计经验的专业团队，为您提供最优质的设计方案'
        },
        {
          icon: 'el-icon-medal',
          title: '品质保证',
          description: '严格把控设计质量，确保每个项目都能达到最佳效果'
        },
        {
          icon: 'el-icon-service',
          title: '贴心服务',
          description: '全程一对一服务，及时响应客户需求，提供专业建议'
        }
      ],
      team: [
        {
          name: '张设计师',
          position: '首席设计师',
          description: '10年设计经验，擅长现代简约风格',
          avatar: 'https://via.placeholder.com/200x200/545c64/ffffff?text=张设计师'
        },
        {
          name: '李设计师',
          position: '资深设计师',
          description: '8年设计经验，擅长北欧风格',
          avatar: 'https://via.placeholder.com/200x200/545c64/ffffff?text=李设计师'
        },
        {
          name: '王设计师',
          position: '设计师',
          description: '5年设计经验，擅长新中式风格',
          avatar: 'https://via.placeholder.com/200x200/545c64/ffffff?text=王设计师'
        }
      ]
    }
  }
}
</script>

<style scoped>
.about {
  padding: 40px 20px;
}

.about-header {
  text-align: center;
  margin-bottom: 40px;
  max-width: 1200px;
  margin-left: auto;
  margin-right: auto;
}

.about-header h2 {
  font-size: 36px;
  color: #545c64;
  margin-bottom: 10px;
}

.about-header p {
  font-size: 18px;
  color: #666;
  max-width: 800px;
  margin: 0 auto;
}

.about-content {
  max-width: 1200px;
  margin: 0 auto;
}

.company-intro {
  margin-bottom: 60px;
  text-align: center;
  padding: 0 20px;
}

.company-intro h3 {
  font-size: 28px;
  color: #545c64;
  margin-bottom: 20px;
}

.company-intro p {
  font-size: 16px;
  color: #666;
  line-height: 1.8;
  margin-bottom: 15px;
  max-width: 800px;
  margin-left: auto;
  margin-right: auto;
}

.our-advantages {
  margin-bottom: 60px;
  padding: 0 20px;
}

.our-advantages h3 {
  font-size: 28px;
  color: #545c64;
  margin-bottom: 30px;
  text-align: center;
}

.advantage-item {
  text-align: center;
  padding: 20px;
  background: #f5f7fa;
  border-radius: 4px;
  height: 100%;
  margin-bottom: 20px;
}

.advantage-item i {
  font-size: 40px;
  color: #409EFF;
  margin-bottom: 15px;
}

.advantage-item h4 {
  font-size: 20px;
  color: #545c64;
  margin-bottom: 10px;
}

.advantage-item p {
  font-size: 14px;
  color: #666;
  line-height: 1.6;
}

.our-team {
  margin-bottom: 40px;
  padding: 0 20px;
}

.our-team h3 {
  font-size: 28px;
  color: #545c64;
  margin-bottom: 30px;
  text-align: center;
}

.team-card {
  margin-bottom: 20px;
  transition: transform 0.3s;
  height: 100%;
}

.team-card:hover {
  transform: translateY(-5px);
}

.team-avatar {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.team-info {
  padding: 20px;
  text-align: center;
}

.team-info h4 {
  font-size: 20px;
  color: #545c64;
  margin-bottom: 5px;
}

.team-info p {
  font-size: 14px;
  color: #666;
  margin-bottom: 5px;
}

.team-desc {
  font-size: 14px;
  color: #666;
  line-height: 1.6;
}

@media screen and (max-width: 768px) {
  .about {
    padding: 20px 15px;
  }

  .about-header {
    margin-bottom: 20px;
  }
  
  .about-header h2 {
    font-size: 24px;
  }
  
  .about-header p {
    font-size: 16px;
    padding: 0 15px;
  }
  
  .company-intro,
  .our-advantages,
  .our-team {
    padding: 0 15px;
  }

  .company-intro h3,
  .our-advantages h3,
  .our-team h3 {
    font-size: 24px;
    margin-bottom: 20px;
  }
  
  .advantage-item {
    margin-bottom: 15px;
  }

  .advantage-item i {
    font-size: 32px;
  }

  .advantage-item h4 {
    font-size: 18px;
  }

  .team-avatar {
    height: 180px;
  }

  .team-info {
    padding: 15px;
  }

  .team-info h4 {
    font-size: 18px;
  }
}
</style> 