<template>
  <div style="background:#eee;">
    <Card dis-hover>
      <Form ref="formCustom" :label-width="90" :label-colon="true">
        <FormItem label="角色名称">
          <Input type="text" v-model="name" :style="{width:'200px'}" readonly></Input>
        </FormItem>
        <FormItem label="权限分配">
          <table class="table">
            <tbody>
            <tr class="ng-scope" v-for="(item, index) in list" :key="item.id">
              <td class="col-td-1">
                <Icon :type="item.icon"></Icon>
                &nbsp;{{item.title}}
              </td>
              <td class="col-td-2">
                <label class="checkbox-inline">
                  <Checkbox
                    :indeterminate="item.status"
                    :value="item.select"
                    @click.prevent.native="checkAll(index)">全选
                  </Checkbox>
                </label>
              </td>
              <td>
                <Checkbox v-for="(child, key) in item.children" :value="item.children[key].select" :key="child.id"
                          @on-change="changeData(index, key)">
                  {{child.title}}
                </Checkbox>
              </td>
            </tr>
            </tbody>
          </table>
        </FormItem>
        <FormItem>
          <Row>
            <i-col span="4" offset="20">
              <Button type="primary" @click="save()">提交</Button>
              <Button @click="()=>{this.$router.go(-1)}" style="margin-left: 8px">返回</Button>
            </i-col>
          </Row>
        </FormItem>
      </Form>
    </Card>
  </div>
</template>

<script>
  import './authorize.less'

  export default {
    name: "Authorize",
    data() {
      return {
        //节点名称
        name: '',
        //节点列表数据
        list: [
          {
            id: "154",
            name: "CommentDefault",
            pid: "0",
            icon: "md-add",
            select: true,
            status: false,
            title: "评价管理",
            children: [
              {
                id: "156",
                name: "CommentDel",
                pid: "154",
                select: true,
                title: "删除评价"
              },
              {
                id: "155",
                name: "CommentIndex",
                pid: "154",
                select: true,
                title: "评价列表"
              }
            ]
          },
        ],
        //原后台取出已有权限数据
        rules: []
      }
    },
    computed: {},
    methods: {
      changeData(index, key) {
        let t = this.list[index];
        let tk = t.children[key];
        //反转
        tk.select = !tk.select;
        //获取子菜单长度
        let len = t.children.length;
        let count = 0;
        //判断
        let is = false; //全选状态
        for (let item of t.children) {
          if (item.select === false) {
            count++;
            is = true; //总有一个反选
          }
        }

        if (is && count !== len) {
          if (tk.select) {
            this.rules.push(tk.id);
            if (!this.inArray(t.id)) {
              this.rules.push(t.id);
            }
          } else {
            //删除指定值
            this.removeByValue(tk.id);
          }
          t.status = true;
          t.select = false;
        } else if (is && count === len) {
          if (tk.select) {
            this.rules.push(tk.id);
          } else {
            //移除当前父类ID
            this.removeByValue(t.id);
            this.removeByValue(tk.id);
          }
          //待有非常之功，必待非常之人
          t.status = false;
          t.select = false;
        } else {
          t.status = false;
          t.select = true;
          this.rules.push(tk.id);
        }
      },
      //全不选 全选
      checkAll(index) {
        let t = this.list[index];
        if (typeof t.status === 'undefined' && t.select === false) {
          t.status = false;
          t.select = true;
          this.rules.push(t.id);
          this.reversal(index, true);
        } else if (typeof t.status !== 'undefined' && t.status === false && t.select === true) {
          t.status = false;
          t.select = false;
          this.reversal(index, false);
          //删除父ID
          this.removeByValue(t.id);
        } else if (typeof t.status !== 'undefined' && t.status === false && t.select === false) {
          t.status = false;
          t.select = true;
          this.rules.push(t.id);
          this.reversal(index, true);
        } else {
          t.status = false;
          t.select = false;
          this.reversal(index, false);
          this.removeByValue(t.id);
        }
      },
      //反转所有
      reversal(index, select) {
        //我TMD 就不知道为什么第一个元素是空的
        if (this.rules && this.rules[0] === '') {
          this.rules.splice(0, 1);
        }
        for (let item of this.list[index].children) {
          if (select === true) {
            this.rules.push(item.id);
          } else {
            this.removeByValue(item.id);
          }
          item.select = select;
        }
      },
      //删除指定值
      removeByValue(val) {
        for (let i = 0; i < this.rules.length; i++) {
          if (this.rules[i] === val) {
            this.rules.splice(i, 1);
            break;
          }
        }
      },
      //检查值是否已存在
      inArray(value) {
        var i = this.rules.length;
        while (i--) {
          if (this.rules[i] === value) {
            return true;
          }
        }
        return false;
      },
      //请求后端数据
      getData() {
        let id = this.$route.params.id;
        if (id) {
          this.request('Authorize', {id: id}).then((res) => {
            if (res.status) {
              this.list = res.data.list;
              this.rules = res.data.select;
              this.name = res.data.name;
            } else {
              this.$Message.error(res.msg);
            }
          }).catch((response) => {
          });
        }
      },
      //提交保存数据
      save() {
        let id = this.$route.params.id;
        if (id) {
          this.request('SaveAuth', {id: id, rules: this.rules, name: this.name}).then((res) => {
            if (res.status) {
              this.$Message.success(res.msg);
              //判断是否有新的权限拉回来
              if (res.data.menu) {
                this.$store.commit('ADD_SIDE_MENU', res.data.menu);
              }
              this.$router.go(-1);
            } else {
              this.$Message.error(res.msg);
            }
          }).catch((response) => {
          });
        }
      },
    }
  }
</script>
