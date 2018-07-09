<style scoped>
.tijiao{
text-align:center;margin-top:10px;
}
.tijiao p{
    text-align: center;
    margin-top: 20px;
    font-size: 26px;
}
</style>
<template>
<div>
    <my-title-component></my-title-component>
    <my-question-component :question="question"></my-question-component>
    <my-answer-component :selecAnswer="selectAnswer" :selectChange="selectChange" :option1="option1" :option2="option2" :option3="option3" :option4="option4" :answerseen="answerseen"></my-answer-component>
    <my-page-component :current="currentPage" :changePage="changePage"></my-page-component>
    <div class="tijiao">
        <Row>
            <i-col span="10" offset="6">
                 <Button type="success" @click="tijiao">提交</Button>
                
                 <p v-html="scoreMsg" v-if="seen"></p>
            </i-col>
       </Row>
       
    </div>
</div>
</template>
<script>
    import axios from 'axios'
    import 'jquery'
    import myTitleComponent from '../components/mytitlecomponent.vue'
    import myQuestionComponent from '../components/myquestioncomponent.vue'
    import myPageComponent from '../components/myPageComponent.vue'
    import myAnswerComponent from '../components/myanswercomponent.vue'
    export default {
        data(){
            return{
                question:'',
                questions:[],
                answers:['','','','','','','','','',''],
                correctanswers:[],
                answeroption:[],
                currentPage:1,
                selectAnswer:'',
                scoreMsg:"你的得分是<font color='red'>100</font>分",
                seen:false,
                option1:'',
                option2:'',
                option3:'',
                option4:'',
                answerseen:true
            }
        },
        components:{
           myTitleComponent,
           myQuestionComponent,
           myPageComponent,
           myAnswerComponent
       },
       mounted() {
           this.getQuestion();
       },
       methods:{
           //获取题库问题
           getQuestion:function(){
               var self= this;
               axios.get('http://jisujiakao.market.alicloudapi.com/driverexam/query?pagenum=1&&pagesize=10&&sort=normal&&subject=1&&type=C1',{
                   headers:{
                        Authorization:'APPCODE f2e05eca479f480ca0c389b2f493d1d7'
                    }
               }).then(function(response){
                   //获取成功,设置题目
                    var list = response.data.result.list;
                    list.forEach(element => {
                        self.questions.push(element.question);
                    });

                    //设置答案
                    list.forEach(element => {
                        self.correctanswers.push(element.answer);
                    });

                    //设置选项option
                    list.forEach(element => {
                        var options=[];
                        options.push(element.option1);
                        options.push(element.option2);
                        options.push(element.option3);
                        options.push(element.option4);
                        self.answeroption.push(options);
                    });

                    //设置第一个问题
                    if($.inArray(self.correctanswers[0],['A','B','C','D']) != -1){
                        self.answerseen = true;
                        self.option1 = self.answeroption[0][0];
                        self.option2 = self.answeroption[0][1];
                        self.option3 = self.answeroption[0][2];
                        self.option4 = self.answeroption[0][3];
                    }else{
                        self.answerseen = false;
                    }

                    self.question = self.questions[self.currentPage-1];
               }).catch(function(response){
                   //获取失败

               });
           },
           //改变页码时修改问题
           changePage:function(current){
               var self= this;
               self.currentPage = current;
               self.question = self.questions[current-1];

                if($.inArray(self.correctanswers[current-1],['A','B','C','D']) != -1){
                    self.answerseen = true;
                    self.option1 = self.answeroption[current-1][0];
                    self.option2 = self.answeroption[current-1][1];
                    self.option3 = self.answeroption[current-1][2];
                    self.option4 = self.answeroption[current-1][3];
                }else{
                    self.answerseen = false;
                }

                //将原来的答案填写到单元按钮中去
                self.selectAnswer = self.answers[current-1]
           },
           //选择变化时
           selectChange:function(current){
               var self = this;
               self.answers[self.currentPage-1] = current;
               self.selectAnswer = current;
           },
           //提交，计算分数
           tijiao:function(){
               var score = 0;
               var self = this;
               self.answers.forEach((element,index) => {
                   if(element == self.correctanswers[index]){
                    score +=10;
                   }
               });

               self.scoreMsg = "你的得分是<font color='red'>"+ score +"</font>分";
               self.seen = true;
           }
       }
    };
</script>