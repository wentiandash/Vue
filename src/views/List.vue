<template>
    <Table border :columns="columns1" :data="news">
        <template slot-scope="{ row }" slot="name">
            <strong>{{ row.name }}</strong>
        </template>
        <template slot-scope="{ row }" slot="action">
            <Button type="error" size="small" @click="remove(row._id)">删除</Button>
        </template>
    </Table>
</template>
<script>
export default {
  data () {
    return {
      columns1: [
        {
          title: '新闻标题',
          key: 'username'
        },
        {
          title: '新闻内容',
          key: 'title'
        },
        {
          title: '作者',
          key: 'username'
        },
        {
          title: '发布时间',
          key: 'begintime'
        },
        {
          title: '操作',
          slot: 'action',
          width: 150,
          align: 'center'
        }
      ],
      news: []
    }
  },
  methods: {
    getlist () {
      this.axios.get('/news/getlist')
        .then(result => {
          if (result.data.state) {
            this.news = result.data.data
          } else {
            console.log('数据访问出错！')
          }
        })
    },
    remove (id) {
      this.$Modal.confirm({
        title: '请确认',
        content: '<p>确定要删除当前数据吗？</p>',
        onOk: () => {
          this.axios.get('/news/remove/' + id)
            .then(res => {
              if (res.status === 200) {
                this.$Message.success('删除成功')
                this.getlist()
              }
            })
        }
      })
    }
  },
  created () {
    this.getlist()
  }
}
</script>
