<template>
  <div>
    <el-form
      class="detail-form-content"
      ref="ruleForm"
      :model="ruleForm"
      :rules="rules"
      label-width="80px"
    >
      <el-row>
                  <el-col :span="12">
        <el-form-item  v-if="type!='info'"  label="帖子标题" prop="title">
          <el-input v-model="ruleForm.title" 
              placeholder="帖子标题" clearable></el-input>
        </el-form-item>
        <div v-else>
          <el-form-item v-if="ruleForm.title" label="帖子标题" prop="title">
              <el-input v-model="ruleForm.title" 
                placeholder="帖子标题" readonly></el-input>
          </el-form-item>
        </div>
      </el-col>
                                          <el-col :span="12">
        <el-form-item  v-if="type!='info'"  label="父节点id" prop="parentid">
          <el-input v-model="ruleForm.parentid" 
              placeholder="父节点id" clearable></el-input>
        </el-form-item>
        <div v-else>
          <el-form-item v-if="ruleForm.parentid" label="父节点id" prop="parentid">
              <el-input v-model="ruleForm.parentid" 
                placeholder="父节点id" readonly></el-input>
          </el-form-item>
        </div>
      </el-col>
                                          <el-col :span="12">
        <el-form-item  v-if="type!='info'"  label="用户名" prop="username">
          <el-input v-model="ruleForm.username" 
              placeholder="用户名" clearable></el-input>
        </el-form-item>
        <div v-else>
          <el-form-item v-if="ruleForm.username" label="用户名" prop="username">
              <el-input v-model="ruleForm.username" 
                placeholder="用户名" readonly></el-input>
          </el-form-item>
        </div>
      </el-col>
                        <el-col :span="12">
        <el-form-item  v-if="type!='info'"  label="是否关闭[关闭/开放]" prop="isdone">
          <el-input v-model="ruleForm.isdone" 
              placeholder="是否关闭[关闭/开放]" clearable></el-input>
        </el-form-item>
        <div v-else>
          <el-form-item v-if="ruleForm.isdone" label="是否关闭[关闭/开放]" prop="isdone">
              <el-input v-model="ruleForm.isdone" 
                placeholder="是否关闭[关闭/开放]" readonly></el-input>
          </el-form-item>
        </div>
      </el-col>
                        </el-row>
                                      <el-row>
            <el-col :span="24">
              <el-form-item v-if="type!='info'" label="帖子内容" prop="content">
                <el-input
                  style="min-width: 200px; max-width: 600px;"
                  type="textarea"
                  :rows="8"
                  placeholder="帖子内容"
                  v-model="ruleForm.content">
                </el-input>
              </el-form-item>
              <div v-else>
                <el-form-item v-if="ruleForm.content" label="帖子内容" prop="content">
                    <span>{{ruleForm.content}}</span>
                </el-form-item>
              </div>
            </el-col>
          </el-row>
                                                                                                                                                                      <el-form-item>
                <el-button v-if="type!='info'" type="primary" @click="onSubmit">提交</el-button>
        <el-button v-if="type!='info'" @click="back()">取消</el-button>
        <el-button v-if="type=='info'" @click="back()">返回</el-button>
      </el-form-item>
    </el-form>
    
    
  </div>
</template>
<script>
// 数字，邮件，手机，url，身份证校验
import { isNumber,isIntNumer,isEmail,isMobile,isURL,checkIdCard } from "@/utils/validate";
export default {
  data() {
    let self = this
    var validateIdCard = (rule, value, callback) => {
      if(!value){
        callback();
      } else if (!checkIdCard(value)) {
        callback(new Error("请输入正确的身份证号码"));
      } else {
        callback();
      }
    };
    var validateUrl = (rule, value, callback) => {
      if(!value){
        callback();
      } else if (!isURL(value)) {
        callback(new Error("请输入正确的URL地址"));
      } else {
        callback();
      }
    };
    var validatePhone = (rule, value, callback) => {
      if(!value){
        callback();
      } else if (!isMobile(value)) {
        callback(new Error("请输入正确的电话号码"));
      } else {
        callback();
      }
    };
    var validateEmail = (rule, value, callback) => {
      if(!value){
        callback();
      } else if (!isEmail(value)) {
        callback(new Error("请输入正确的邮箱地址"));
      } else {
        callback();
      }
    };
    var validateNumber = (rule, value, callback) => {
      if(!value){
        callback();
      } else if (!isNumber(value)) {
        callback(new Error("请输入数字"));
      } else {
        callback();
      }
    };
    var validateIntNumber = (rule, value, callback) => {
      if(!value){
        callback();
      } else if (!isIntNumer(value)) {
        callback(new Error("请输入整数"));
      } else {
        callback();
      }
    };
    return {
      id: '',
      type: '',
            ruleForm: {
                        title: '',
                                content: '',
                                parentid: '',
                                userid: '',
                                username: '',
                                isdone: '',
                      },
                                                                                                rules: {
                  title: [
                                                                                              ],
                  content: [
                            { required: true, message: '帖子内容不能为空', trigger: 'blur' },
                                                                                              ],
                  parentid: [
                                                                                              ],
                  userid: [
                            { required: true, message: '用户id不能为空', trigger: 'blur' },
                                                                                              ],
                  username: [
                                                                                              ],
                  isdone: [
                                                                                              ],
              }
    };
  },
  props: ["parent"],
  computed: {
                                                                              },
  methods: {
        // 下载
    download(file){
      window.open(`${file}`)
    },
    // 初始化
    init(id,type) {
      if (id) {
        this.id = id;
        this.type = type;
      }
      if(this.type=='info'||this.type=='else'){
        this.info(id);
      }else if(this.type=='cross'){
        var obj = this.$storage.getObj('crossObj');
        for (var o in obj){
                    if(o=='title'){
            this.ruleForm.title = obj[o];
            continue;
          }
                    if(o=='content'){
            this.ruleForm.content = obj[o];
            continue;
          }
                    if(o=='parentid'){
            this.ruleForm.parentid = obj[o];
            continue;
          }
                    if(o=='userid'){
            this.ruleForm.userid = obj[o];
            continue;
          }
                    if(o=='username'){
            this.ruleForm.username = obj[o];
            continue;
          }
                    if(o=='isdone'){
            this.ruleForm.isdone = obj[o];
            continue;
          }
                  }
                                                                                                              }
                                                                                                                                          },
                                                        // 多级联动参数
                                                                        info(id) {
      this.$http({
        url: `forum/info/${id}`,
        method: "get"
      }).then(({ data }) => {
        if (data && data.code === 0) {
          this.ruleForm = data.data;
        } else {
          this.$message.error(data.msg);
        }
      });
    },
        // 提交
    onSubmit() {
                  // ${column.compare}
                              // ${column.compare}
                              // ${column.compare}
                              // ${column.compare}
                              // ${column.compare}
                              // ${column.compare}
                                                                                                                                                this.$refs["ruleForm"].validate(valid => {
        if (valid) {
          this.$http({
            url: `forum/${!this.ruleForm.id ? "save" : "update"}`,
            method: "post",
            data: this.ruleForm
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.$message({
                message: "操作成功",
                type: "success",
                duration: 1500,
                onClose: () => {
                  this.parent.showFlag = true;
                  this.parent.addOrUpdateFlag = false;
                  this.parent.forumCrossAddOrUpdateFlag = false;
                  this.parent.search();
                }
              });
            } else {
              this.$message.error(data.msg);
            }
          });
        }
      });
    },
    // 获取uuid
    getUUID () {
      return new Date().getTime();
    },
    // 返回
    back() {
      this.parent.showFlag = true;
      this.parent.addOrUpdateFlag = false;
      this.parent.forumCrossAddOrUpdateFlag = false;
    },
                                                                              }
};
</script>
<style lang="scss">
.editor{
  height: 500px;
}
.amap-wrapper {
  width: 100%;
  height: 500px;
}
.search-box {
  position: absolute;
}
</style>
