<template>

    <div class="animated fadeIn">
        <Table :columns="columns1" :data="data1"></Table>
        <Page :total="100" @on-change="gopage" align="center"></Page>
        <Modal
                v-model="modal1"
                title="编辑用户"
                @on-ok="ok"
                @on-cancel="cancel" width="60%">
            <Form ref="formCustom" :model="formCustom" :rules="ruleCustom" :label-width="80">
                <FormItem label="姓名" prop="userName">
                    <Input type="text" v-model="formCustom.userName"></Input>
                </FormItem>
                <FormItem label="年龄" prop="age">
                    <Input type="password" v-model="formCustom.age"></Input>
                </FormItem>
                <FormItem label="地址" prop="addr">
                    <Input type="text" v-model="formCustom.addr" number></Input>
                </FormItem>
                <FormItem>
                    <Button type="primary" @click="handleSubmit('formCustom')">保存</Button>
                    <Button type="ghost" @click="handleReset('formCustom')" style="margin-left: 8px">Reset</Button>
                </FormItem>
            </Form>
        </Modal>

    </div>
</template>
<script>
    /* eslint-disable indent,comma-dangle,arrow-body-style,space-before-function-paren */


    export default {
        data() {
            const validateUser = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error('请输入用户名'));
                }
            };
            const validateAddr = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error('请输入地址'));
                }
            };
            const validateAge = (rule, value, callback) => {
                if (value==='') {
                    return callback(new Error('年龄不能为空'));
                }
                // 模拟异步验证效果
                setTimeout(() => {
                    if (!Number.isInteger(value)) {
                        callback(new Error('请输入整数'));
                    } else {
                        if (value < 18) {
                            callback(new Error('必须年满18！'));
                        } else {
                            callback();
                        }
                    }
                }, 1000);
            };
            return {
                columns1: [
                    {
                        title: '姓名',
                        key: 'name'
                    },
                    {
                        title: '年龄',
                        key: 'age'
                    },
                    {
                        title: '地址',
                        key: 'addr'
                    },
                    {
                        title: '操作',
                        key: 'action',
                        align: 'center',
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
                                            this.show(params.index)
                                        }
                                    }
                                }, '查看'),
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
                                            this.edit(params.index)
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
                            ]);
                        }
                    }
                ],
                self: this,
                columns7: [
                    {
                        title: '姓名',
                        key: 'name',
                        render (row, column, index) {
                            return `<Icon type="person"></Icon> <strong>${row.name}</strong>`;
                        }
                    },
                    {
                        title: '年龄',
                        key: 'age'
                    },
                    {
                        title: '地址',
                        key: 'addr'
                    },
                    {
                        title: '操作',
                        key: 'action',
                        width: 150,
                        align: 'center',
                        render (row, column, index) {
                            return `<i-button type="primary" size="small" @click="show(${index})">查看</i-button> <i-button type="error" size="small" @click="remove(${index})">删除</i-button>`;
                        }
                    }
                ],
                
                data1: [
                    {
                        name: '王小明',
                        age: 18,
                        addr: '北京市朝阳区芍药居'
                    },
                    {
                        name: '张小刚',
                        age: 25,
                        addr: '北京市海淀区西二旗'
                    },
                    {
                        name: '李小红',
                        age: 30,
                        addr: '上海市浦东新区世纪大道'
                    },
                    {
                        name: '周小伟',
                        age: 26,
                        addr: '深圳市南山区深南大道'
                    }
                ],
                modal1: false,
                formCustom: {
                    userName: '',
                    addr: '',
                    age: ''
                },
                ruleCustom: {
                    userName: [
                        { validator: validateUser, trigger: 'blur' }
                    ],
                    addr: [
                        { validator: validateAddr, trigger: 'blur' }
                    ],
                    age: [
                        { validator: validateAge, trigger: 'blur' }
                    ]
                }
            }
        },
        methods: {
            show (index) {
                this.$Modal.info({
                    title: '用户信息',
                    content: `姓名：${this.data1[index].name}<br>年龄：${this.data1[index].age}<br>地址：${this.data1[index].addr}`
                })
            },
            edit (index) {
                this.modal1=true;
            },
            remove (index) {
                this.data1.splice(index, 1);
            },
            ok () {
                this.modal1 = false,
                    this.$Message.info('点击了确定');
            },
            cancel () {
                this.$Message.info('点击了取消');
            },
            show (index) {
                this.$Modal.info({
                    title: '用户信息',
                    content: `姓名：${this.data1[index].name}<br>年龄：${this.data1[index].age}<br>地址：${this.data1[index].addr}`
                })
            },
            remove (index) {
                this.data1.splice(index, 1);
            },
            handleSubmit (name) {
                this.$refs[name].validate((valid) => {
                    if (valid) {
                        this.$Message.success('Success!');
                    } else {
                        this.$Message.error('Fail!');
                    }
                })
            },
            handleReset (name) {
                this.$refs[name].resetFields();
            },
            gopage(page){
                console.log(page);
            }
        },
    }
</script>