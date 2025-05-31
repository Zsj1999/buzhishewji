<template>
  <div class="contact">
    <div class="contact-header">
      <h2>联系我们</h2>
      <p>期待与您沟通，为您提供专业的设计服务</p>
    </div>
    
    <div class="contact-content">
      <el-row :gutter="40">
        <el-col :xs="24" :sm="24" :md="12">
          <div class="contact-form">
            <h3>在线咨询</h3>
            <el-form :model="form" :rules="rules" ref="form" label-width="80px">
              <el-form-item label="姓名" prop="name">
                <el-input v-model="form.name" placeholder="请输入您的姓名"></el-input>
              </el-form-item>
              <el-form-item label="电话" prop="phone">
                <el-input v-model="form.phone" placeholder="请输入您的联系电话"></el-input>
              </el-form-item>
              <el-form-item label="邮箱" prop="email">
                <el-input v-model="form.email" placeholder="请输入您的邮箱"></el-input>
              </el-form-item>
              <el-form-item label="咨询内容" prop="message">
                <el-input type="textarea" v-model="form.message" rows="4" placeholder="请描述您的需求"></el-input>
              </el-form-item>
              <el-form-item>
                <el-button type="primary" @click="submitForm('form')">提交</el-button>
                <el-button @click="resetForm('form')">重置</el-button>
              </el-form-item>
            </el-form>
          </div>
        </el-col>
        
        <el-col :xs="24" :sm="24" :md="12">
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
                <p>010-12345678</p>
              </div>
            </div>
            <div class="info-item">
              <i class="el-icon-message"></i>
              <div>
                <h4>邮箱</h4>
                <p>contact@buzhisheji.com</p>
              </div>
            </div>
            <div class="info-item">
              <i class="el-icon-time"></i>
              <div>
                <h4>工作时间</h4>
                <p>周一至周五 9:00-18:00</p>
              </div>
            </div>
          </div>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ContactPage',
  data() {
    return {
      form: {
        name: '',
        phone: '',
        email: '',
        message: ''
      },
      rules: {
        name: [
          { required: true, message: '请输入姓名', trigger: 'blur' },
          { min: 2, max: 20, message: '长度在 2 到 20 个字符', trigger: 'blur' }
        ],
        phone: [
          { required: true, message: '请输入联系电话', trigger: 'blur' },
          { pattern: /^1[3-9]\d{9}$/, message: '请输入正确的手机号码', trigger: 'blur' }
        ],
        email: [
          { required: true, message: '请输入邮箱地址', trigger: 'blur' },
          { type: 'email', message: '请输入正确的邮箱地址', trigger: 'blur' }
        ],
        message: [
          { required: true, message: '请输入咨询内容', trigger: 'blur' },
          { min: 10, max: 500, message: '长度在 10 到 500 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.$message({
            message: '提交成功，我们会尽快与您联系！',
            type: 'success'
          });
          this.resetForm(formName);
        } else {
          return false;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    }
  }
}
</script>

<style scoped>
.contact {
  padding: 40px 20px;
}

.contact-header {
  text-align: center;
  margin-bottom: 40px;
  max-width: 1200px;
  margin-left: auto;
  margin-right: auto;
}

.contact-header h2 {
  font-size: 36px;
  color: #545c64;
  margin-bottom: 10px;
}

.contact-header p {
  font-size: 18px;
  color: #666;
  max-width: 800px;
  margin: 0 auto;
}

.contact-content {
  max-width: 1200px;
  margin: 0 auto;
}

.contact-form,
.contact-info {
  background: #f5f7fa;
  padding: 30px;
  border-radius: 4px;
  height: 100%;
}

.contact-form h3,
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

@media screen and (max-width: 768px) {
  .contact {
    padding: 20px 15px;
  }

  .contact-header {
    margin-bottom: 20px;
  }
  
  .contact-header h2 {
    font-size: 24px;
  }
  
  .contact-header p {
    font-size: 16px;
    padding: 0 15px;
  }
  
  .contact-form,
  .contact-info {
    margin-bottom: 20px;
    padding: 20px;
  }
  
  .contact-form h3,
  .contact-info h3 {
    font-size: 20px;
    margin-bottom: 20px;
  }

  .info-item {
    margin-bottom: 20px;
  }

  .info-item i {
    font-size: 20px;
  }

  .info-item h4 {
    font-size: 16px;
  }

  .el-form-item {
    margin-bottom: 15px;
  }

  .el-button {
    width: 100%;
    margin-bottom: 10px;
  }
}
</style> 