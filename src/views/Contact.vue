<template>
  <div class="contact">
    <div class="contact-header">
      <h2>联系我们</h2>
      <p>期待与您沟通，为您提供专业的设计服务</p>
    </div>

    <div class="contact-content">
      <div class="contact-info">
        <h3>联系方式</h3>
        <div class="info-item">
          <i class="el-icon-location"></i>
          <div>
            <h4>地址</h4>
            <p>安国市祁州大路376号不止设计工作室</p>
          </div>
        </div>
        <div class="info-item">
          <i class="el-icon-phone"></i>
          <div>
            <h4>电话</h4>
            <p><a :href="`tel:18832245000`" class="phone-link">18832245000</a></p>
            <p><a :href="`tel:18132123590`" class="phone-link">18132123590</a></p>
          </div>
        </div>
        <div class="info-item">
          <i class="el-icon-time"></i>
          <div>
            <h4>工作时间</h4>
            <p>周一至周日 9:00-19:00</p>
          </div>
        </div>
        <div class="map-container">
          <!-- 这里可以放置地图组件或iframe嵌入的地图 -->
          <div class="map-container">
  <iframe 
    src="https://map.baidu.com/?qt=s&wd=安国市祁州大路376号" 
    width="100%" 
    height="100%"
    frameborder="0">
  </iframe>
</div>
          <p>地图位置展示区域</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ContactPage'
}
</script>

<style scoped>
.contact {
  padding: 40px 20px;
  max-width: 800px;
  margin: 0 auto;
}

.contact-header {
  text-align: center;
  margin-bottom: 40px;
}

.contact-header h2 {
  font-size: 36px;
  color: #545c64;
  margin-bottom: 10px;
}

.contact-header p {
  font-size: 18px;
  color: #666;
}

.contact-info {
  background: #f5f7fa;
  padding: 30px;
  border-radius: 4px;
}

.contact-info h3 {
  font-size: 24px;
  color: #545c64;
  margin-bottom: 30px;
  text-align: center;
}

.info-item {
  display: flex;
  align-items: flex-start;
  margin-bottom: 30px;
}

.info-item i {
  font-size: 24px;
  color: #409EFF;
  margin-right: 15px;
  margin-top: 5px;
}

.info-item h4 {
  font-size: 18px;
  color: #545c64;
  margin-bottom: 5px;
}

.info-item p {
  font-size: 14px;
  color: #666;
  line-height: 1.6;
}

.phone-link {
  color: #409EFF;
  text-decoration: none;
  transition: color 0.3s;
}

.phone-link:hover {
  color: #337ecc;
  text-decoration: underline;
}

.map-container {
  margin-top: 40px;
  height: 300px;
  background: #eee;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 4px;
}

@media screen and (max-width: 768px) {
  .contact {
    padding: 20px 15px;
  }
  
  .contact-header h2 {
    font-size: 28px;
  }
  
  .contact-header p {
    font-size: 16px;
  }
  
  .contact-info {
    padding: 20px;
  }
  
  .info-item {
    margin-bottom: 20px;
  }
  
  .info-item i {
    font-size: 20px;
  }
  
  .map-container {
    height: 200px;
  }
}
</style>