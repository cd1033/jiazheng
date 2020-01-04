<template>
    <div>
        <el-buttton size="samall" type="success" @click="toAddHandler">添加</el-buttton>
        <el-buttton size="samall" type="danger">删除</el-buttton>

        <el-table >

        </el-table>
        <!--模态框开始-->
        <el-dialog title="录入栏目信息"
        :visible.sync="visible"
        width="60%">
        </el-dialog>
        <!--模态框结束-->
    </div>
</template>

<script>
export default {
    methods:{
        toAddHandler(){
            this.form={
                type:"category"
            }
            this.visible=true;
        },
    }
}
</script>

<style scoped>

</style>