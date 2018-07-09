<template>
    <div class="page">
       <Row>
            <i-col span="10" offset="6">
                <Page :total="10" :page-size="1" :current="current" @on-change="changePage"></Page>
            </i-col>
       </Row>
    </div>    
</template>
<script>
    export default {
        data(){
            return{
                
            }
        },
        props:{
            current: Number,
            changePage:Function
        },
        methods:{
           
        }
    }
</script>
<style>
.page{
    margin-top: 10px;
    text-align: right;
}
</style>
