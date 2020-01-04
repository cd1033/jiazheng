<template>
    <div>
        <el-button type="primary" @click="toAddHandler">添加</el-button>
        <el-button type="danger">批量删除</el-button>

    <el-table :data="orders">
        <el-table-column prop="id" label="编号"></el-table-column>
        <el-table-column prop="orderTime" label="订单接收时间"></el-table-column>
        <el-table-column prop="total" label="订单总数"></el-table-column>
        <el-table-column prop="status" label="已完成"></el-table-column>
        <el-table-column prop="remark" label="备注"></el-table-column>
        <el-table-column prop="customerId" label="顾客信息"></el-table-column>
        <el-table-column prop="waiterId" label="员工信息"></el-table-column>
        <el-table-column prop="addressId" label="地址"></el-table-column> 
        <el-table-column label="操作">
            <template v-slot="slot">
                <a href="" @click.prevent="toDeleteHandler(slot.row.id)" >删除</a>
                <a href="" @click.prevent="toUpdateHandler(slot.row)">修改</a>
            </template>    
        </el-table-column>   
    </el-table>
        <!-- 分页 -->
        <el-pagination layout="prev, pager, next" :total="50">
        </el-pagination>
        <!-- 分页结束 -->

        <!-- 模态框-->
        <el-dialog
        :title.sync="title" 
        :visible.sync="visible" width="60%"> 
        <el-form :model="form" label-width="80px">
        <el-form-item label="订单接收时间">
          <el-input v-model="form.orderTime"></el-input>
        </el-form-item>
        <el-form-item label="订单总数">
          <el-input v-model="form.total"></el-input>
        </el-form-item>
        <el-form-item label="已完成">
          <el-input v-model="form.status"></el-input>
        </el-form-item>
        <el-form-item label="备注">
          <el-input v-model="form.remark"></el-input>
        </el-form-item>
        <el-form-item label="顾客信息">
          <el-input v-model="form.customerId"></el-input>
        </el-form-item>
        <el-form-item label="员工信息">
          <el-input v-model="form.waiterId"></el-input>
        </el-form-item>
        <el-form-item label="地址">
          <el-input v-model="form.addressId"></el-input>
        </el-form-item>                
        </el-form>
        <span slot="footer" class="dialog-footer">
        <el-button size="small" @click="closeModalHandler">取 消</el-button>
        <el-button size="small" @click="submitHandler" >确 定</el-button>
        </span>
        </el-dialog>
        <!--模态框-->

    </div>

</template>

<script>
import request from '@/utils/request'
import querystring from 'querystring' 
export default {
    data(){
        return{
        form:{},
        orders:[],
        visible:false
        }  
    },
    methods:{
        closeModalHandler(){
        this.visible=false;
        },
        toDeleteHandler(id)
        {
        this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          //调用后台接口完成删除操作
          let url="http://localhost:6677/order/deleteById?id="+id;
          request.get(url).then((response)=>{
            //刷新数据
            this.loaddata();
            //提示结果
             this.$message({
              type: 'success',
              message :response.message
    
            });
          });
     
        })
        },
        toUpdateHandler(){
        this.form=row;
        this.visible=true;

        },
        submitHandler(){
            let url="http://localhost:6677/order/saveOrUpdate"
            request({
                url,
                method:"post",
                //告诉给后台有我的请求体中放的使查询字符串
                headers:{
                    "Content-Type":"application/x-www-form-urlencoded"
                },
                data:querystring.stringify(this.form)
            }).then((response)=>{
                this.closeModalHandler();
                this.loaddata();
                this.$message({type:"success",message:response.message})
            })          
        },
        loaddate(){
            let url="http://localhost:6677/order/findAll"
            request.get(url).then((response)=>{
                this.orders = response.data
            })
        },
        toAddHandler(){
        this.form={
            type:"order"
        }
        // this.title="录入订单信息";
        this.visible=true;
        },

    },
    created(){
        this.loaddate();
    }
}
</script>