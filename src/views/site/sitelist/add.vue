<template>
  <el-dialog
          title="新增网点"
          width="600px"
          @close="cancel"
          :close-on-click-modal="false"
          :visible.sync="dialogTableVisible">
    <el-form :model="form" :rules="rules" ref="Form" label-width="120px" hide-required-asterisk>
      <el-form-item label="网点名称" prop="c__branchesName">
        <el-input v-model="form.c__branchesName"></el-input>
      </el-form-item>
      <el-form-item label="联系方式" prop="c_br_phone">
        <el-input v-model="form.c_br_phone"></el-input>
      </el-form-item>
      <el-form-item label="网点地址" prop="c_br_address">
        <el-input v-model="form.c_br_address"></el-input>
      </el-form-item>
      <el-form-item label="网点级别" prop="i">
        <el-select v-model="form.i" placeholder="请选择网点级别">
          <el-option v-if="role == 'level'" label="总部" value="0"></el-option>
          <el-option v-if="role == 'level0' || role == 'level'" label="一级网点" value="1"></el-option>
          <el-option v-if="role == 'level1' || role == 'level'" label="二级网点" value="2"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="网点账号" prop="username">
        <el-input v-model="form.username"></el-input>
      </el-form-item>
      <el-form-item label="网点密码" prop="password">
        <el-input v-model="form.password"></el-input>
      </el-form-item>
      <el-form-item label="经理名称">
        <el-input v-model="form.c_jili"></el-input>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button @click="dialogTableVisible = false">取 消</el-button>
      <el-button :loading="isSubmitLoading" type="primary" @click="submitForm('Form')">确 定</el-button>
    </div>
  </el-dialog>
</template>

<script>
  import {addSiteApi} from '@/api/site'

  export default {
    name: "AddSite",
    data() {
      return {
        dialogTableVisible: false,
        isSubmitLoading: false,
        form: {
          c__branchesName: '',
          c_br_phone: '',
          c_br_address: '',
          l_branchesName: '',
          l_br_phone: '',
          l_br_address: '',
          username: '',
          password: '',
          i: '',
          u_id: 0,
          c_jili: '',
          l_jili: ''
        },
        rules: {
          c__branchesName: {required: true, message: '请输入名称', trigger: 'blur'},
          c_br_phone: {required: true, message: '请输入联系方式', trigger: 'blur'},
          c_br_address: {required: true, message: '请输入地址', trigger: 'blur'},
          l_branchesName: {required: true, message: '请输入名称', trigger: 'blur'},
          l_br_phone: {required: true, message: '请输入联系方式', trigger: 'blur'},
          l_br_address: {required: true, message: '请输入地址', trigger: 'blur'},
          username: {required: true, message: '请输入账号', trigger: 'blur'},
          password: {required: true, message: '请输入密码', trigger: 'blur'},
          i: {required: true, message: '请选择网点级别', trigger: 'change'}
        }
      }
    },
    computed: {
      userId() {
        return this.$store.getters.userId
      },
      role() {
        return this.$store.getters.user.authorities[0].authority
      }
    },
    methods: {
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            let data = {...this.form};
            data.u_id = this.userId;
            this.isSubmitLoading = true;
            addSiteApi(data).then(() => {
              this.isSubmitLoading = false;
              this.$emit('update');
              this.cancel()
            }).catch(() => {
              this.isSubmitLoading = false;
            });
          } else {
            return false;
          }
        });
      },
      cancel() {
        this.dialogTableVisible = false;
        Object.assign(this.$data.form, this.$options.data().form);
        this.$refs['Form'].resetFields()
      }
    }
  }
</script>

<style scoped>

</style>
