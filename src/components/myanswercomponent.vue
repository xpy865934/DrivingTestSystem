<template>
    <div class="answer">
       <Row>
            <i-col span="10" offset="6">
                 <Radio-group v-if="answerseen" v-model="answer" @on-change="selectChange" vertical>
                    <Radio label="A">
                        <span>{{ option1 }}</span>
                    </Radio>
                    <Radio label="B">
                        <span>{{ option2 }}</span>
                    </Radio>
                    <Radio label="C">
                        <span>{{ option3 }}</span>
                    </Radio>
                    <Radio label="D">
                        <span>{{ option4 }}</span>
                    </Radio>
                 </Radio-group>
                 <Radio-group v-else v-model="answer" @on-change="selectChange" vertical>
                    <Radio label="对">
                        <span>对</span>
                    </Radio>
                    <Radio label="错">
                        <span>错</span>
                    </Radio>
                 </Radio-group>
            </i-col>
       </Row>
    </div>    
</template>
<script>
    export default {
        data(){
            return {
                answer:this.selecAnswer   //创建props属性selectAnswer的副本
            }
        },
        props:{
            selecAnswer:String,
            selectChange:Function,
            option1:String,
            option2:String,
            option3:String,
            option4:String,
            answerseen:Boolean
        },
        watch:{
            //这边使用侦听器，防止Avoid mutating a prop directly since the value will be overwritten whenever the parent component re-
            selecAnswer:function(val){
                //外部对属性selectAnswer变更
                var self = this;
                self.answer =val;
            },
            answer:function(val){
                //内部对answer的变更
                //this.$emit("on-answer-change",val);//组件内对answer变更后向外部发送事件通知
            }
        }
    }
</script>
<style>
.answer{
    margin-top: 10px;
}
</style>
