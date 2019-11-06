<template>
  <Form ref="formCustom" :model="fromData" :rules="ruleValidate" :label-width="90" :label-colon="true">
    <FormItem label="登录账号" prop="account">
      <Input type="text" v-model="fromData.account"></Input>
    </FormItem>
    <FormItem label="登录密码" prop="password">
      <Input type="password" v-model="fromData.password"></Input>
    </FormItem>
    <FormItem label="所属角色" prop="role_id">
      <Select v-model="fromData.role_id" placeholder="请选择所属角色" @on-change="selectedRole">
        <Option value="1">系统管理员</Option>
      </Select>
    </FormItem>

    <FormItem label="联系电话">
      <Input type="text" v-model="fromData.mobile" number></Input>
    </FormItem>
    <FormItem label="邮箱地址">
      <Input type="text" v-model="fromData.email"></Input>
    </FormItem>
    <FormItem label="备注">
      <Input v-model="fromData.remark" type="textarea" :autosize="{minRows: 3,maxRows: 5}"
             placeholder="请输入50字以内的备注信息..."></Input>
    </FormItem>
    <FormItem>
      <Button type="primary" @click="handleSubmit()">提交</Button>
      <Button @click="handleReset" style="margin-left: 8px">取消</Button>
    </FormItem>
  </Form>
</template>

<script>
  export default {
    name: 'UserInfo',
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
      selectedRole(val) {
        this.fromData.role_id = val
        // TODO 循环角色列表获取真实角色名称
        this.fromData.role_name = "管理员角色"
      }
    }
  }
</script>
