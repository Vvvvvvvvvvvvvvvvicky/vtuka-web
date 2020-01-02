<template>
  <div>
    <el-row >
      <!--<search-bar></search-bar>-->
      <el-tooltip effect="dark" placement="right"
                  v-for="item in callocations"
                  :key="item.id">
       
        <p slot="content" style="width: 300px" class="abstract">{{item.collocationType}}</p>
        <el-card style="width: 450px;margin-bottom: 20px;height: 450px;float: left;margin-right: 15px" class="book"
                 bodyStyle="padding:10px" shadow="hover">
          <div class="cover">
            <img :src="item.imgUrl" alt="封面">
          </div>
          <div class="info">
            <div class="title">
              <a href="">{{item.collocationType}}</a>
            </div>
          </div>
        </el-card>
      </el-tooltip>
    </el-row>
    <el-row class="pagination">
      <div align="middle" padding="100px">
       <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="currentPage"
      :page-sizes="pageSizes"
      :page-size="pagesize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total">
    </el-pagination>
      </div>
    </el-row>
  </div>
</template>

<script>
  export default {
    name: 'Collocations',
    data () {
      return {
        callocations: [],
        currentPage: 1,
        pageSizes: [10, 20, 30, 40, 50],
        pagesize: 10,
        total:0
      }
    },
    mounted: function () {
      this.loadCallocations()
    },
    methods:{
      loadCallocations () {
        var _this = this
        this.$axios.get('/collocations').then(resp => {
          if (resp && resp.status === 200) {
            _this.callocations = resp.data.resultObj.list
            _this.total = resp.data.resultObj.total
          }
        })
      },
      handleCurrentChange: function (currentPage) {
        this.currentPage = currentPage
      },
      handleSizeChange: function(pagesize) {
        this.pagesize = pagesize
      }
    }
  }
</script>

<style scoped>
  .pagination {
    height: 240px;
  }

  .pagination_div {
    
  }

  .cover {
    /*width: 455px;
    height: 172px;*/
    margin-bottom: 7px;
    overflow: hidden;
    cursor: pointer;
  }

  img {
    width: 400px;
    height: 400px;
    margin: 0 auto;
  }

  .title {
    font-size: 14px;
    text-align: left;
  }

  .author {
    color: #333;
    width: 102px;
    font-size: 13px;
    margin-bottom: 6px;
    text-align: left;
  }

  .abstract {
    display: block;
    line-height: 17px;
  }

  a {
    text-decoration: none;
  }

  a:link, a:visited, a:focus {
    color: #3377aa;
  }

  .el-pagination {
    padding: 100px;
    font-weight: 500;
  }
</style>

