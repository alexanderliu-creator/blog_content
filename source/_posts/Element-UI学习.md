---
title: Element-UI学习
date: 2021-03-25 09:29:23
tags:
	前端
---



# Element-UI相关学习，[Element-UI官网](https://element.eleme.cn/#/zh-CN)~~，其实光看官网好像也就够了~~



<!--more-->





！！！我貌似发现了有对应于vue-cli@4.5版本的全新格式！！！

[Element-UI+链接](https://element-plus.gitee.io/#/zh-CN/component/quickstart)

`Vue.use(全局变量)`这种形式失效了，我之前不知道怎么改来着，结果发现了宝藏！！！

![image-20210325093617629](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210325093617629.png)





下面学习分为两个阶段，一个阶段是根据视频来学习栅格布局类似的东西，第二个阶段是自己去官网摸索一下，做点东西出来嗷！！！



# 视频学习

## 布局

- 总共有24列
- 高度可以调整

![image-20210325190500890](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210325190500890.png)

- 如上：左边偏移的列数是4列(offset属性指定)，使用了16列，中间实际上是塞入了一个容器嗷。
- 行的高度可以使用height属性来调整的。

- **gutter是指栅格间间隔（行间距），offset是指栅格左侧的间隔格数**
- 可以利用一个空行来做成上面的那段空间。
- 布局里面塞入了容器，容器里面还可以塞容器。

![image-20210325191027623](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210325191027623.png)

- 这些使用，官网上都找的到嗷！！！



## 表单制作例子

布局分析：

![image-20210325191307850](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210325191307850.png)

- 对于Vue中的style属性里面，首先scoped是非常常用的嗷！！！，其次，对于不同的元素处理也是不一样的嗷。
  - 对于element-ui中的结点元素，要用`.el-row`这种方式才可以选中
  - 对于原生结点，`h1{}`这种形式实际上就可以对于特定的结点进行处理。
- 上网查组件例子然后拼凑一下
- 在Element-ui中，由于多层嵌套，逻辑会非常乱，这个时候可以，使用`this.username`不一定可以打印出username，这个时候可以`console.log(this)`。看看其中的属性和方法，找到我们需要的值就可、
- 例如`this.ValidateForm.username`
- 记得循环的时候要用的`v-for`去循环，而且要绑定名字`:name=...`
- css原生功能仍然存在，可以使用的

代码样例：

```vue
<template>
    <!--由于这里要保证位置，我们按照布局分析来操作-->
    <el-row type="flex" justify="center">
        <el-col :span="6">
            <div class="grid-content"></div>
        </el-col>
    </el-row>


    <el-row type="flex" justify="center">
        <el-col :span="6">
            <el-card shadow="always">
                <h1 style="text-align: center;">活动表格</h1>
                <el-divider></el-divider>
                <!-- form表单 -->
                <el-form :model="ValidateForm" ref="ValidateForm" label-width="100px" class="demo-ruleForm">

                    <!-- 用户名 -->
                    <el-form-item
                        label="用户名"
                        prop="username"
                        :rules="[
                        { required: true, message: '用户名不能为空'},
                        { type: 'string', message: '用户名必须为字符串'}
                        ]"
                    >
                    <el-input placeholder="请输入用户名" type="string" v-model.string="ValidateForm.username" autocomplete="off"></el-input>
                    </el-form-item>


                    <!-- 密码 -->
                    <el-form-item
                        label="密码"
                        prop="password"
                        :rules="[
                        { required: true, message: '密码不能为空'},
                        { type: 'number', message: '密码必须为数字值'}
                        ]"
                    >
                    <el-input placeholder="请输入密码" type="password" v-model.number="ValidateForm.password" autocomplete="off" show-password></el-input>
                    </el-form-item>

                    <el-radio-group v-model="radio">
                        <el-radio :label=false>仅前端展示</el-radio>
                        <el-radio :label=true>仅后端展示</el-radio>
                    </el-radio-group>


                    <!-- 按钮 -->
                    <el-form-item style="justify-content: center;">
                        <el-button type="primary" @click="submitForm('ValidateForm')">提交</el-button>
                        <el-button @click="resetForm('ValidateForm')">重置</el-button>
                    </el-form-item>
                </el-form>
            </el-card>
        </el-col>
    </el-row>
</template>


<script>
export default {
    name:"Form",
    data() {
        return {
        ValidateForm: {
            username: "",
            password: "",
        },
        radio: false,
        };
    },
    methods: {
        submitForm(formName) {
        this.$refs[formName].validate((valid) => {
            if (valid) {
            // alert('username: '+this.username+",password: "+this.age);
            console.log(this);
            alert("name: "+this.ValidateForm.username+",password: "+this.ValidateForm.password);
            
            } else {
            console.log('error submit!!');
            return false;
            }
        });
        },
        resetForm(formName) {
        this.$refs[formName].resetFields();
        }
    }
    }
</script>

<style scoped>
    /* h1{
        text-align:left;
    } */
    .el-button{
        align-content: center;
        /* text-align: center; */
    }

    .el-radio-group{
        display: flex;
        margin: 20px auto;
        justify-content: center;
    }

    .el-form-item{
        /* display: flex; */
        margin: 20px auto;
        justify-content: center;
        margin: 20px
    }
</style>

```







