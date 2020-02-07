<template>
  <div>
    <el-row >
      <!--<search-bar></search-bar>-->
      <el-tooltip effect="dark" placement="right"
                  v-for="item in callocations.slice((currentPage-1)*pageSize,currentPage*pageSize)"
                  :key="item.id">
        <p slot="content" style="width: 200px" class="abstract">{{item.collocationType}}</p>
        <el-card style="width: 250px;margin-bottom: 20px;height: 250px;float: left;margin-right: 15px" class="book"
                 bodyStyle="padding:10px" shadow="hover">
            <img :src="item.imgUrl" alt="封面" @click="editBook(item)">
        </el-card>
      </el-tooltip>
      <edit-form @onSubmit="loadBooks()" ref="edit"></edit-form>
    </el-row>
    <el-row class="pagination">
      <div align="middle" padding="100px">
        <el-pagination
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :current-page="currentPage"
            :page-sizes="pageSizes"
            :page-size="pageSize"
            layout="total, sizes, prev, pager, next, jumper"
            :total="total"/>
      </div>
    </el-row>
  </div>
</template>

<script>
import EditForm from './EditForm'
  export default {
    name: 'Collocations',
    components: {EditForm},
    data () {
      return {
        callocations: [],
        currentPage: 1,
        pageSizes: [9, 18, 45, 90],
        pageSize: 9,
        pageNo: 1,
        total:0,
      }
    },
    mounted: function () {
      this.loadCallocations()
    },
    methods:{
      loadCallocations () {
        var _this = this
        this.$axios.get('/collocations', {
            params: {
              pageNo: this.pageNo,
              pageSize: this.pageSize
              }
        }).then(resp => {
          if (resp && resp.status === 200) {
            _this.callocations = resp.data.resultObj.list
            _this.total = resp.data.resultObj.total
          }
        })
      },
      handleCurrentChange: function (currentPage) {
        this.pageNo = currentPage
        this.loadCallocations()
      },
      handleSizeChange: function(pageSize) {
        this.pageSize = pageSize
        this.loadCallocations()
      },
      editBook: function(item) {
        this.$refs.edit.dialogFormVisible = true
        this.$refs.edit.form = {
          id: item.id,
          collocationUrl: item.collocationUrl,
          imgUrl: item.imgUrl,
          collocationType: item.collocationType,
          collocationDesc: item.collocationDesc,
          isDelete: item.isDelete,
          isShow: item.isShow,
          // DressCollocationItem: {
          //   id: item.category.id.toString(),
          //   name: item.category.name
          // }
        }
        // this.$refs.edit.category = {
        //   id: item.category.id.toString()
        // }
      }
    }
  }
</script>

<style scoped>
  img {
    width: 230px;
    height: 230px;
    margin: 0 auto;
  }

  .abstract {
    display: block;
    line-height: 5px;
  }

  .el-pagination {
    padding: 100px;
    font-weight: 500;
  }
</style>

