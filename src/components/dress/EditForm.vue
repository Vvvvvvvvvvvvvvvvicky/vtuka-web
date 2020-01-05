<template>
  <div>
    <i class="el-icon-circle-plus-outline"  @click="dialogFormVisible = true"></i>
    <el-dialog
      title="添加/修改搭配"
      :visible.sync="dialogFormVisible"
      @close="clear">
      <el-form v-model="form" style="text-align: left" ref="dataForm">
        <el-form-item label="搭配源信息" :label-width="formLabelWidth" prop="collocationUrl">
          <el-input v-model="form.title" autocomplete="off" placeholder="不加《》"></el-input>
        </el-form-item>
        <el-form-item label="搭配主图" :label-width="formLabelWidth" prop="imgUrl">
          <el-input v-model="form.author" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="搭配名称" :label-width="formLabelWidth" prop="collocationType">
          <el-input v-model="form.date" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="搭配描述" :label-width="formLabelWidth" prop="collocationDesc">
          <el-input v-model="form.press" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="存储状态（是否删除）" :label-width="formLabelWidth" prop="isDelete">
          <el-input v-model="form.cover" autocomplete="off" placeholder="图片 URL"></el-input>
          <img-upload @onUpload="uploadImg" ref="imgUpload"></img-upload>
        </el-form-item>
        <el-form-item label="展示状态（是否显示）" :label-width="formLabelWidth" prop="isShow">
          <el-input type="textarea" v-model="form.abs" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="搭配具体单品信息" :label-width="formLabelWidth" prop="DressCollocationItem">
          <el-input type="textarea" v-model="form.abs" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="分类" :label-width="formLabelWidth" prop="cid">
        <el-select v-model="form.category.id" placeholder="请选择分类">
          <el-option label="文学" value="1"></el-option>
          <el-option label="流行" value="2"></el-option>
          <el-option label="文化" value="3"></el-option>
          <el-option label="生活" value="4"></el-option>
          <el-option label="经管" value="5"></el-option>
          <el-option label="科技" value="6"></el-option>
        </el-select>
        </el-form-item>
        <el-form-item prop="id" style="height: 0">
          <el-input type="hidden" v-model="form.id" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="onSubmit">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
  export default {
    name: 'EditForm',
    data () {
      return {
        dialogFormVisible: false,
        form: {
          id: '',
          collocationUrl: '',
          imgUrl: '',
          collocationType: '',
          collocationDesc: '',
          isDelete: '',
          isShow: '',
          DressCollocationItems: []
        },
        formLabelWidth: '120px'
      }
    },
    methods: {
      clear () {
        //this.$refs.imgUpload.clear()
        this.form = {
          id: '',
          collocationUrl: '',
          imgUrl: '',
          collocationType: '',
          collocationDesc: '',
          isDelete: '',
          isShow: '',
          DressCollocationItems: []
        }
      },
      onSubmit () {
        this.$axios
          .post('/books', {
              id: this.form.id,
              collocationUrl: this.form.collocationUrl,
              imgUrl: this.form.imgUrl,
              collocationType: this.form.collocationType,
              collocationDesc: this.form.collocationDesc,
              isDelete: this.form.isDelete,
              isShow: this.form.isShow,
              DressCollocationItems: this.form.DressCollocationItems
          }).then(resp => {
            if (resp && resp.status === 200) {
              this.dialogFormVisible = false
              this.$emit('onSubmit')
            }
        })
      },
    }
  }
</script>

<style scoped>
  .el-icon-circle-plus-outline {
    margin: 50px 0 0 20px;
    font-size: 100px;
    float: left;
    cursor: pointer;
  }
</style>
