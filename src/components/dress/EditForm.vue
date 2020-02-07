<template>
  <div>
    <i class="el-icon-circle-plus-outline"  @click="dialogFormVisible = true"></i>
    <el-dialog
      title="修改搭配"
      width="600px"
      :visible.sync="dialogFormVisible"
      @close="clear">
      <el-form v-model="form" style="text-align: left" >
        <el-form-item label="搭配源信息：" :label-width="formLabelWidth" prop="collocationUrl">
          <el-input v-model="form.collocationUrl" autocomplete="on" placeholder="搭配源地址，例如：http://www.xingpin.com/px/58999"></el-input>
        </el-form-item>
        <el-form-item label="搭配封面主图：" :label-width="formLabelWidth" prop="imgUrl">
          <el-input v-model="form.imgUrl" autocomplete="off"></el-input>
          <img :src="form.imgUrl" style="margin:15px;">
        </el-form-item>
        <el-form-item label="搭配名称：" :label-width="formLabelWidth" prop="collocationType">
          <el-input v-model="form.collocationType" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="搭配描述：" :label-width="formLabelWidth" prop="collocationDesc">
          <el-input  type="textarea" v-model="form.collocationDesc" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="存储状态：" :label-width="formLabelWidth" prop="isDelete">
          <el-switch v-model="form.isDelete" style="text-align:left;"></el-switch>
        </el-form-item>
        <el-form-item label="展示状态：" :label-width="formLabelWidth" prop="isShow">
          <el-switch v-model="form.isShow"></el-switch>
        </el-form-item>
        <!-- <el-form-item label="分类" :label-width="formLabelWidth" prop="cid">
        <el-select placeholder="请选择分类">
          <el-option label="文学" value="1"></el-option>
          <el-option label="流行" value="2"></el-option>
          <el-option label="文化" value="3"></el-option>
          <el-option label="生活" value="4"></el-option>
          <el-option label="经管" value="5"></el-option>
          <el-option label="科技" value="6"></el-option>
        </el-select>
        </el-form-item> -->
        <el-input type="hidden" v-model="form.id" autocomplete="off"></el-input>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false;innerVisible = true" >查看/编辑单品清单</el-button>
          <el-dialog
            width="30%"
            title="单品信息"
            :visible.sync="innerVisible"
            append-to-body>
            <div slot="footer" class="dialog-footer">
              <el-button @click="dialogFormVisible = false">取 消</el-button>
              <el-button type="primary" @click="onSubmit">提 交</el-button>
            </div>
          </el-dialog>
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="onSubmit">提 交</el-button>
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
        innerVisible: false,
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
        sub_form: {
          id: '',
          itemUrl: '',
          itemName: '',
          itemBrand: '',
          imgUrl: '',
          price:0,
          isDelete: '',
        },
        formLabelWidth: '120px'
      }
    },
    methods: {
      clear () {
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

<style>
  .el-icon-circle-plus-outline {
    margin: 50px 0 0 20px;
    font-size: 100px;
    float: left;
    cursor: pointer;
  }
  .el-dialog {
    width: 600px;
  }

  .el-form-item__content {
    text-align: left;
  }
  .el-dialog__body {
    padding: 30px 20px 0px 20px;
  }
  .el-button {
    margin: 0px 10px;
  }
</style>
