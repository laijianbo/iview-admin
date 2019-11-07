<template>
  <Form ref="formCustom" :model="fromData" :rules="ruleValidate" :label-width="90" :label-colon="true">

    <FormItem label="角色名称" prop="role_name">
      <Input type="text" v-model="fromData.role_name"></Input>
    </FormItem>
    <FormItem label="角色说明">
      <Input v-model="fromData.desc" type="textarea" :autosize="{minRows: 3,maxRows: 5}" show-word-limit
             maxlength="00"
             placeholder="请输入100字以内的备注信息..."></Input>
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
    name: 'RoleDetails',
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
      }
    }
  }
</script>
