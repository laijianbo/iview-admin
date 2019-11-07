<template>
  <Form ref="formCustom" :model="fromData" :rules="ruleValidate" :label-width="90" :label-colon="true">
    <FormItem label="所属节点" prop="pid">
      <Select v-model="fromData.pid" placeholder="请选择所属节点" @on-change="selectedNode">
        <Option value="0">根节点</Option>
      </Select>
    </FormItem>
    <FormItem label="节点名称" prop="title">
      <Input type="text" v-model="fromData.title"></Input>
    </FormItem>
    <FormItem label="节点图标" prop="icon">
      <Input type="text" v-model="fromData.icon" :style="{width:'80px',}"></Input>
      <Icon type="md-add"/>

    </FormItem>
    <FormItem label="路由地址" prop="url">
      <Input type="text" v-model="fromData.url"></Input>
    </FormItem>
    <FormItem label="排序" prop="sort">
      <Input type="number" v-model="fromData.sort"></Input>
    </FormItem>
    <FormItem>
      <Row>
        <i-col span="8" offset="16">
          <Button type="primary" @click="handleSubmit()">提交</Button>
          <Button @click="handleReset" style="margin-left: 8px">取消</Button>
        </i-col>
      </Row>
    </FormItem>
  </Form>
</template>

<script>
  export default {
    name: 'NodeDetails',
    props: ['info'],
    data: () => {
      return {
        fromData: {},
        ruleValidate: {
          account: [
            {required: true, message: '请输入登录账号', trigger: 'blur'}
          ],
          password: [
            {required: true, message: '请输入登录密码', trigger: 'blur'}
          ],
          role_id: [
            {required: true, message: '请选择所属角色', trigger: 'change'},
          ],
        },
      }
    },
    watch: {
      info: {
        handler(n) {
          if (n !== undefined) {
            this.fromData = n
          }
        },
        deep: true
      }
    },
    methods: {
      handleSubmit() {
        this.$refs['formCustom'].validate((valid) => {
          if (valid) {
            this.$Message.success('Success!');
            this.$emit('closeModal')
          } else {
            this.$Message.error('请完善必填信息!');
          }
        })
      },
      handleReset() {
        this.$emit('closeModal')
      },
      selectedNode(val) {
        this.fromData.pid = val
        // TODO 循环角色列表获取真实角色名称
        this.fromData.p_title = "根节点"
      }
    }
  }
</script>
