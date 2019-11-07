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
        <Page :total="10" :current="1" show-total @on-change="pageChange"></Page>
      </div>
    </div>
    <Modal v-model="iModal" :title="title" :style="{top:'20px'}" :loading="loading" :closable="false"
           :mask-closable="false" :footer-hide="true">
      <RoleDetails ref="roleInfo" :info="roleInfo" @closeModal="closeModal"></RoleDetails>
    </Modal>
  </Card>
</template>

<script>
  import './index.less'
  import RoleDetails from "./components/role-details";
  import Authorize from "./components/authorize";

  const obj = {
    id: '',
    role_name: '',
    desc: '',
    auth_node: '',
    status: '',
    create_time: ''
  }
  export default {
    name: 'Role',
    components: {
      Authorize,
      RoleDetails,
    },
    data() {
      return {
        tableData: [
          {
            id: '1',
            role_name: '系统角色',
            desc: '系统角色',
            auth_node: '',
            status: '1',
            create_time: '2019-10-13'
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
            title: '角色名称',
            key: 'title',
            align: 'center',
          },
          {
            title: '角色说明',
            key: 'type',
            align: 'center',
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
            width: 200,
            render: (h, params) => {
              return h('div', [
                h('Button', {
                  props: {
                    type: 'warning',
                    size: 'small'
                  },
                  style: {
                    marginRight: '5px'
                  },
                  on: {
                    click: () => {
                      this.authorize(params.index)
                    }
                  }
                }, '授权'),
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
        title: '新增系统角色',
        loading: true,
        tabLoading: false,
        roleInfo: obj,
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
          this.title = '修改系统角色'
          this.roleInfo = this.tableData[index]
          console.log(this.tableData[index])
        } else {
          this.roleInfo = obj

        }
        this.iModal = true
      },
      authorize(index) {
        this.$router.push({path: '/system_setting/authorize'})
      },
      remove(index) {
        this.$Modal.confirm({
          title: "删除角色",
          content: "是否删除该角色？",
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
