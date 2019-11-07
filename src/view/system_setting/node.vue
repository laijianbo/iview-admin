<template>
  <Card>
    <div class="search-con search-con-top">
      <Input clearable placeholder="输入关键字搜索" class="search-input" v-model="searchKey"/>
      <Button @click="handleSearch" class="search-btn" type="primary">
        <Icon type="search"/>&nbsp;&nbsp;搜索
      </Button>
      <Button @click="info()" type="success" :style="{float:'right'}">
        <Icon type="md-add"/>&nbsp;新增
      </Button>
    </div>
    <br>
    <Table stripe :data="tableData" :columns="columnData" max-height="500" :loading="tabLoading"></Table>
    <div style="margin: 10px;overflow: hidden">
      <div style="float: right;">
        <Page :total="100" :current="1" show-total show-sizer show-elevator @on-change="pageChange"></Page>
      </div>
    </div>
    <Modal v-model="iModal" :title="title" :style="{top:'20px'}" :loading="loading" :closable="false"
           :mask-closable="false" :footer-hide="true">
      <NodeDetails ref="nodeInfo" :info="nodeInfo" @closeModal="closeModal"></NodeDetails>
    </Modal>

  </Card>
</template>

<script>
  import './index.less'
  import NodeDetails from "./components/node-details";

  const obj = {
    id: '',
    pid: '',
    title: '',
    type: '',
    icon: '',
    url: '',
    display: '',
    status: '',
    create_time: ''
  }
  export default {
    name: 'Node',
    components: {
      NodeDetails,
    },
    data() {
      return {
        tableData: [
          {
            id: '1',
            pid: '0',
            title: '系统管理',
            type: '1',
            icon: '',
            url: '',
            display: '1',
            status: '1',
            create_time: '2016-10-03'
          },
          {
            id: '2',
            pid: '1',
            title: '用户管理',
            type: '1',
            icon: '',
            url: '',
            display: '1',
            status: '1',
            create_time: '2016-10-03'
          },
          {
            id: '3',
            pid: '1',
            title: '获取用户列表',
            type: '2',
            icon: '',
            url: '',
            display: '1',
            status: '1',
            create_time: '2016-10-03'
          },
        ],
        columnData: [
          {
            title: '编号',
            key: 'id',
            align: 'center',
            width: 100,
          },
          {
            title: '节点名称',
            key: 'title',
            align: 'center',
          },
          {
            title: '类型',
            key: 'type',
            align: 'center',
            width: 100,
          },
          {
            title: '图标',
            align: 'center',
            key: 'icon',
            width: 100
          },
          {
            title: '前端地址',
            align: 'center',
            key: 'url',
            width: 200
          },
          {
            title: '是否展示',
            align: 'center',
            key: 'display',
            width: 100
          },
          {
            title: '状态',
            align: 'center',
            key: 'status',
            width: 100
          },
          {
            title: '创建时间',
            align: 'center',
            key: 'create_time',
            width: 150
          },
          {
            title: '操作',
            align: 'center',
            width: 130,
            render: (h, params) => {
              return h('div', [
                h('Button', {
                  props: {
                    type: 'primary',
                    size: 'small'
                  },
                  style: {
                    marginRight: '5px'
                  },
                  on: {
                    click: () => {
                      this.info(params.index)
                    }
                  }
                }, '编辑'),
                h('Button', {
                  props: {
                    type: 'error',
                    size: 'small'
                  },
                  on: {
                    click: () => {
                      this.remove(params.index)
                    }
                  }
                }, '删除')
              ])
            }
          },
        ],
        searchKey: '',
        iModal: false,
        title: '新增系统节点',
        loading: true,
        tabLoading: false,
        nodeInfo: obj,
        currentPage: 1,
        show: false
      }
    },
    methods: {
      getList() {
        this.$Loading.start()
        this.tabLoading = true
        setTimeout(() => {
          this.$Loading.finish()
          this.tabLoading = false
        }, 2000)
      },
      info(index = false) {
        if (index !== false) {
          this.title = '修改系统节点'
          this.nodeInfo = this.tableData[index]
          console.log(this.tableData[index])
        } else {
          this.nodeInfo = obj

        }
        this.iModal = true
      },
      remove(index) {
        this.$Modal.confirm({
          title: "删除节点",
          content: "是否删除该节点？",
          onOk: (key) => {
            this.tableData.splice(index, 1)
            this.$Modal.success({title: "操作提示", content: "删除成功"})
          },
        })
      },
      handleSearch() {
        this.getList()
      },
      pageChange(page) {
        this.currentPage = page
        this.getList()
      },
      closeModal() {
        this.iModal = false
      }
    },
    mounted() {
      this.getList()
    }
  }
</script>
