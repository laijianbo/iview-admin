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
    <Table stripe :data="tableData" :columns="columnData" max-height="500"></Table>
    <div style="margin: 10px;overflow: hidden">
      <div style="float: right;">
        <Page :total="100" :current="1" show-total show-sizer show-elevator @on-change="pageChange"></Page>
      </div>
    </div>
    <Modal v-model="iModal" :title="title" :style="{top:'20px'}" :loading="loading" :closable="false"
           :mask-closable="false" :footer-hide="true">
      <UserInfo ref="user_info" :info="userInfo" @closeModal="closeModal"></UserInfo>
    </Modal>

  </Card>
</template>

<script>
  import './index.less'
  import UserInfo from "./components/user-info";

  const obj = {
    id: '',
    account: '',
    role_name: '',
    mobile: '',
    email: '',
    remark: '',
    last_login_ip: '',
    last_login_time: '',
    create_time: ''
  }
  export default {
    name: 'user',
    components: {
      UserInfo,
    },
    data() {
      return {
        tableData: [
          {
            id: 1,
            account: 'admin',
            role_name: '系统角色',
            mobile: '1888888888',
            email: 'lai390701@163.com',
            remark: '超级管理员',
            last_login_ip: '127.0.0.1',
            last_login_time: '2016-10-03',
            create_time: '2016-10-03'
          },
          {
            id: 1,
            account: 'admin1',
            role_name: '系统角色',
            mobile: '1888888888',
            email: 'lai390701@163.com',
            remark: '超级管理员',
            last_login_ip: '127.0.0.1',
            last_login_time: '2016-10-03',
            create_time: '2016-10-03'
          },
          {
            id: 1,
            account: 'admin2',
            role_name: '系统角色',
            mobile: '1888888888',
            email: 'lai390701@163.com',
            remark: '超级管理员',
            last_login_ip: '127.0.0.1',
            last_login_time: '2016-10-03',
            create_time: '2016-10-03'
          },
          {
            id: 2,
            account: 'admin3',
            role_name: '系统角色',
            mobile: '1888888888',
            email: 'lai390701@163.com',
            remark: '超级管理员',
            last_login_ip: '127.0.0.1',
            last_login_time: '2016-10-03',
            create_time: '2016-10-03'
          },
          {
            id: 3,
            account: 'admin4',
            role_name: '系统角色',
            mobile: '1888888888',
            email: 'lai390701@163.com',
            remark: '超级管理员',
            last_login_ip: '127.0.0.1',
            last_login_time: '2016-10-03',
            create_time: '2016-10-03'
          },
          {
            id: 4,
            account: 'admin',
            role_name: '系统角色',
            mobile: '1888888888',
            email: 'lai390701@163.com',
            remark: '超级管理员',
            last_login_ip: '127.0.0.1',
            last_login_time: '2016-10-03',
            create_time: '2016-10-03'
          },
          {
            id: 5,
            account: 'admin',
            role_name: '系统角色',
            mobile: '1888888888',
            email: 'lai390701@163.com',
            remark: '超级管理员',
            last_login_ip: '127.0.0.1',
            last_login_time: '2016-10-03',
            create_time: '2016-10-03'
          },
          {
            id: 6,
            account: 'admin',
            role_name: '系统角色',
            mobile: '1888888888',
            email: 'lai390701@163.com',
            remark: '超级管理员',
            last_login_ip: '127.0.0.1',
            last_login_time: '2016-10-03',
            create_time: '2016-10-03'
          },
        ],
        columnData: [
          {
            title: '编号',
            key: 'id',
            align: 'center',
            width: 80,
            fixed: 'left'
          },
          {
            title: '账号',
            key: 'account',
            align: 'center',
            width: 150,
            fixed: 'left'
          },
          {
            title: '所属角色',
            align: 'center',
            key: 'role_name',
            width: 150
          },
          {
            title: '联系电话',
            align: 'center',
            key: 'mobile',
            width: 150
          },
          {
            title: '邮箱地址',
            align: 'center',
            key: 'email',
            width: 180
          },
          {
            title: '备注',
            key: 'remark',
            width: 200
          },
          {
            title: '上次登录ip',
            align: 'center',
            key: 'last_login_ip',
            width: 150
          },
          {
            title: '上次登录时间',
            align: 'center',
            key: 'last_login_time',
            width: 150
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
            fixed: 'right',
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
                }, '详情'),
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
        modalTitle: '新增系统用户',
        iModal: false,
        title: '新增系统用户',
        loading: true,
        userInfo: {...obj},
        currentPage: 1,
        show: false
      }
    },
    methods: {
      getList() {
        this.$Loading.start()
        setTimeout(() => {
          this.$Loading.finish()
        }, 2000)
      },
      info(index = false) {
        if (index !== false) {
          this.userInfo = this.tableData[index]
          console.log(this.tableData[index])
        } else {
          this.userInfo = obj

        }
        this.iModal = true
      },
      remove(index) {
        this.$Modal.confirm({
          title: "删除用户",
          content: "是否删除该用户？",
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
