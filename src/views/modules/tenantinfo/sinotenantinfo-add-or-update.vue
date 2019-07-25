<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="租户名称" prop="name">
      <el-input v-model="dataForm.name" placeholder="租户名称"></el-input>
    </el-form-item>
    <el-form-item label="租户简称" prop="shortName">
      <el-input v-model="dataForm.shortName" placeholder="租户简称"></el-input>
    </el-form-item>
    <el-form-item label="租户code" prop="code">
      <el-input v-model="dataForm.code" placeholder="租户code"></el-input>
    </el-form-item>
    <el-form-item label="域名" prop="domainName">
      <el-input v-model="dataForm.domainName" placeholder="域名"></el-input>
    </el-form-item>
    <el-form-item label="logo图标地址" prop="logo">
      <el-input v-model="dataForm.logo" placeholder="logo图标地址"></el-input>
    </el-form-item>
    <el-form-item label="微信图标地址" prop="wechatNumber">
      <el-input v-model="dataForm.wechatNumber" placeholder="微信图标地址"></el-input>
    </el-form-item>
    <el-form-item label="客服热线" prop="customerHotline">
      <el-input v-model="dataForm.customerHotline" placeholder="客服热线"></el-input>
    </el-form-item>
    <el-form-item label="邮箱地址" prop="email">
      <el-input v-model="dataForm.email" placeholder="邮箱地址"></el-input>
    </el-form-item>
    <el-form-item label="版本" prop="version">
      <el-input v-model="dataForm.version" placeholder="版本"></el-input>
    </el-form-item>
    <el-form-item label="备案号" prop="recordNo">
      <el-input v-model="dataForm.recordNo" placeholder="备案号"></el-input>
    </el-form-item>
    <el-form-item label="租户状态" prop="status">
      <el-input v-model="dataForm.status" placeholder="租户状态"></el-input>
    </el-form-item>
    <el-form-item label="创建时间" prop="ctime">
      <el-input v-model="dataForm.ctime" placeholder="创建时间"></el-input>
    </el-form-item>
    <el-form-item label="修改时间" prop="mtime">
      <el-input v-model="dataForm.mtime" placeholder="修改时间"></el-input>
    </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
  export default {
    data () {
      return {
        visible: false,
        dataForm: {
          id: 0,
          name: '',
          shortName: '',
          code: '',
          domainName: '',
          logo: '',
          wechatNumber: '',
          customerHotline: '',
          email: '',
          version: '',
          recordNo: '',
          status: '',
          ctime: '',
          mtime: ''
        },
        dataRule: {
          name: [
            { required: true, message: '租户名称不能为空', trigger: 'blur' }
          ],
          shortName: [
            { required: true, message: '租户简称不能为空', trigger: 'blur' }
          ],
          code: [
            { required: true, message: '租户code不能为空', trigger: 'blur' }
          ],
          domainName: [
            { required: true, message: '域名不能为空', trigger: 'blur' }
          ],
          logo: [
            { required: true, message: 'logo图标地址不能为空', trigger: 'blur' }
          ],
          wechatNumber: [
            { required: true, message: '微信图标地址不能为空', trigger: 'blur' }
          ],
          customerHotline: [
            { required: true, message: '客服热线不能为空', trigger: 'blur' }
          ],
          email: [
            { required: true, message: '邮箱地址不能为空', trigger: 'blur' }
          ],
          version: [
            { required: true, message: '版本不能为空', trigger: 'blur' }
          ],
          recordNo: [
            { required: true, message: '备案号不能为空', trigger: 'blur' }
          ],
          status: [
            { required: true, message: '租户状态不能为空', trigger: 'blur' }
          ],
          ctime: [
            { required: true, message: '创建时间不能为空', trigger: 'blur' }
          ],
          mtime: [
            { required: true, message: '修改时间不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.id = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.id) {
            this.$http({
              url: this.$http.adornUrl(`/tenantinfo/sinotenantinfo/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.name = data.sinotenantinfo.name
                this.dataForm.shortName = data.sinotenantinfo.shortName
                this.dataForm.code = data.sinotenantinfo.code
                this.dataForm.domainName = data.sinotenantinfo.domainName
                this.dataForm.logo = data.sinotenantinfo.logo
                this.dataForm.wechatNumber = data.sinotenantinfo.wechatNumber
                this.dataForm.customerHotline = data.sinotenantinfo.customerHotline
                this.dataForm.email = data.sinotenantinfo.email
                this.dataForm.version = data.sinotenantinfo.version
                this.dataForm.recordNo = data.sinotenantinfo.recordNo
                this.dataForm.status = data.sinotenantinfo.status
                this.dataForm.ctime = data.sinotenantinfo.ctime
                this.dataForm.mtime = data.sinotenantinfo.mtime
              }
            })
          }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/tenantinfo/sinotenantinfo/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'name': this.dataForm.name,
                'shortName': this.dataForm.shortName,
                'code': this.dataForm.code,
                'domainName': this.dataForm.domainName,
                'logo': this.dataForm.logo,
                'wechatNumber': this.dataForm.wechatNumber,
                'customerHotline': this.dataForm.customerHotline,
                'email': this.dataForm.email,
                'version': this.dataForm.version,
                'recordNo': this.dataForm.recordNo,
                'status': this.dataForm.status,
                'ctime': this.dataForm.ctime,
                'mtime': this.dataForm.mtime
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message({
                  message: '操作成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    this.visible = false
                    this.$emit('refreshDataList')
                  }
                })
              } else {
                this.$message.error(data.msg)
              }
            })
          }
        })
      }
    }
  }
</script>
