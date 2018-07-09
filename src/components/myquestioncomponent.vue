<style scoped>
#question{
    height:auto;
}
</style>
<template>
    <div>
       <Row>
            <i-col span="10" offset="6">
                <i-input :readonly="true" id="question" type="textarea" :autosize="{ minRows: 4, maxRows: 6 }" placeholder="请输入..." v-model="question"></i-input>
            </i-col>
       </Row>
    </div>
</template>
<script>
    export default {
        props:{
            question: String //定义传值类型
        },
        data(){
           return{
           }
       }
    };
</script>