<template>
  <div style="width: 90%">
    <h3 style="float: left">我的关注</h3>
    <div style="clear: both;"></div>
    <el-row :gutter="30">
      <div class="container" v-if="show" style="float: left;width: 100%">
        <el-table style="width: 100%;"
                  :data="blogList.slice((currentPage-1)*pagesize,currentPage*pagesize)">
          <el-table-column type="index" width="50">
          </el-table-column>
          <el-table-column label="author" prop="name" width="120">
            <template slot-scope="scope">
              <el-button @click="handleClick(scope.row)" type="text">{{scope.row.name}}</el-button>
            </template>
          </el-table-column>
          <el-table-column label="organ" prop="organ" width="240">
          </el-table-column>
          <el-table-column label="tag" prop="tag" :formatter = "tagFormat" width="240">
          </el-table-column>
          <el-table-column label="操作" width="100">
            <template slot-scope="scope">
              <el-button @click="handleClick(scope.row)" type="text" size="small">查看</el-button>
            </template>
          </el-table-column>
        </el-table>
        <div style="clear: both"></div>
        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="currentPage"
          :page-sizes="[5, 10, 20, 40]"
          :page-size="pagesize"
          layout="total, sizes, prev, pager, next, jumper"
          :total="blogList.length"
          style="float: left">
        </el-pagination>
      </div>
    </el-row>
    <div v-if="!show">
      <br>
      <br>
      <br>
      <img src="../../../assets/blank.png">
      <br>
      <br>
      <p style="color: gray">空空如也</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'UserSearch',
  data () {
    return {
      currentPage: 1,
      pagesize: 10,
      blogList: [],
      username: this.$store.state.user.username,
      show: true
    }
  },
  created: function () {
    this.handleBlogList()
  },
  methods: {
    handleSizeChange: function (size) {
      this.pagesize = size
      console.log(this.pagesize)
    },
    handleCurrentChange: function (currentPage) {
      this.currentPage = currentPage
      console.log(this.currentPage)
    },
    handleBlogList () {
      var self = this
      self.$axios.post('/api/returnMyfollow', {
        username: this.username
      })
        .then(function (response) {
          if (response.data.code === 200) {
            self.show = true
            self.blogList = response.data.data
          } else {
            self.show = false
            self.$message({
              type: 'warning',
              message: 'no following'})
          }
        })
        .catch(function (error) {
          alert(error)
        })
    },
    tagFormat (row, column) {
      return row.tag.join(', ')
    },
    handleClick (row) {
      this.$router.push('/author/' + row._id)
      // this.reload()
    }
  }
}
</script>

<style scoped>

</style>
