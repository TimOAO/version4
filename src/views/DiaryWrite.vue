<template>
    <div>
        
        <Nav showText navText="新增心情日記" />
        <div class="background">
            <div style="padding-left: 6.4vw;padding-right: 6.4vw;">
                <!--事件名稱-->
                <div>
                    <div class="icon"><img  src="@/assets/svg/icon_diary_pen.svg"></div>
                    <input id="intput_event_name" type="text" placeholder="事件名稱" autocomplete="off" v-model="eventname">
                </div>
                <!--日期-->
                <div class="datebox" style="margin-top:15px;">
                    <div class="icon"><img  src="@/assets/svg/icon_diary_schedule.svg"></div>
                    <input class="form-date__input" type="date" id="input-date" name="input-date-start" v-model="date"/>
                </div>
                <!--心情評分-->
                <form id="rating">
                    <div class="icon"><img  src="@/assets/svg/icon_diary_rating_mood.svg"></div>
                    <input type="radio" id="very-happy" name="emoji_rating" value="VeryHappy" v-model="mood">
                    <label for="very-happy"></label>
            
                    <input type="radio" id="happy" name="emoji_rating" value="Happy" v-model="mood">
                    <label for="happy"></label>
            
                    <input type="radio" id="neutral" name="emoji_rating" value="Neutral" v-model="mood">
                    <label for="neutral"></label>

                    <input type="radio" id="sad" name="emoji_rating" value="Sad" v-model="mood">
                    <label for="sad"></label>

                    <input type="radio" id="very-sad" name="emoji_rating" value="VerySad" v-model="mood">
                    <label for="very-sad"></label>
                </form>
                <!--事情的經過-->
                <div class="datebox" style="margin-top:25px;">
                    <div class="icon"><img  src="@/assets/svg/icon_diary_schedule.svg"></div>
                    <span class="text">事情的經過/想法</span>   
                </div>
                <textarea  placeholder="請輸入內容" autocomplete="off" v-model="course"></textarea>

                <!--事情的結果-->
                <div class="datebox" style="margin-top:15px;">
                    <div class="icon"><img  src="@/assets/svg/icon_diary_schedule.svg"></div>
                    <span class="text">結果/處理方式</span>   
                </div>
                <textarea  placeholder="請輸入內容" autocomplete="off" v-model="diaryresult"></textarea>
            </div>


            <!--送出按鈕/取消按鈕-->
            <b-container style="margin-top:25px;">
                <b-row class="text-center">
                    <b-col>
                        <router-link to="/feeling">
                            <div class="btn_submit" @click="clickSubmit" >送出</div>
                        </router-link>
                    </b-col>
                    <b-col>
                        <router-link to="/feeling">
                            <div class="btn_cancel">取消</div>
                        </router-link>
                    </b-col>
                        
                </b-row>
            </b-container> 
            
        </div>    
        
        
    </div>
    
</template>
<script>
    import Nav from '@/components/Nav.vue'
    
    export default {
        name: 'Feeling',
        data() {
            return {
                //userid,eventname,date,time,category,mood,course,result,ispublic,additional,number,hug
                eventname: '',
                date: '',
                time: '',
                category: "學業",
                mood: '',
                course: '',
                diaryresult: '',
                ispublic: 'y',
                additional: 'test',
                number: 0,
                hug: 0,
                comment: 0,
            }
        },
        methods: {
            clickSubmit: function () {
                this.time = this.getTime();
                console.log(localStorage.getItem("isWater"));
                localStorage.setItem("isWater","T");
                this.$http
                .post("/api/diaryWrite", {
                    userID: this.$store.state.userName,
                    eventname: this.eventname,
                    date: this.date,
                    time: this.time,
                    category: this.category,
                    mood: this.mood,
                    course: this.course,
                    diaryresult: this.diaryresult,
                    ispublic: this.ispublic,
                    additional: this.additional,
                    number: this.number,
                    hug: this.hug,
                    comment: this.comment,
                    comment_notRead: 'n',
                }).then((res) => {
                    console.log(res.body);
                });            
            },
            getTodayDate() {
                var fullDate = new Date();
                var yyyy = fullDate.getFullYear();
                var MM = (fullDate.getMonth() + 1) >= 10 ? (fullDate.getMonth() + 1) : ("0" + (fullDate.getMonth() + 1));
                var dd = fullDate.getDate() < 10 ? ("0"+fullDate.getDate()) : fullDate.getDate();
                var today = yyyy + "-" + MM + "-" + dd;
                return today;
            },
            getTime() {
                var fullTime = new Date();
                var hh = fullTime.getHours() < 10 ? ("0"+fullTime.getHours()) : fullTime.getHours();
                var mm = fullTime.getMinutes() < 10 ? ("0"+fullTime.getMinutes()) : fullTime.getMinutes();
                var now = hh + ":" + mm;
                return now;
            }
        },
        created() {
            this.date = this.getTodayDate();
        },
        components:{
            Nav,
            
        }
    }
</script>
<style scoped>
a{
    text-decoration:none;
}
.btn_cancel{
    height: 43px;
    width: 152px;
    background: #FFFFFF;
    border: 1px solid #20E2D7;
    box-sizing: border-box;
    box-shadow: 0px 4px 17px -1px rgba(107, 182, 177, 0.51);
    border-radius: 33px;

    font-family: Taipei Sans TC Beta;
    font-style: normal;
    font-weight: normal;
    font-size: 16px;
    line-height: 43px;
    color: #8E8E8E;
    margin: 0 auto;
}
.btn_submit{
    height: 43px;
    width: 152px;
    background: #20E2D7;
    box-shadow: 0px 4px 17px -1px rgba(107, 182, 177, 0.51);
    border-radius: 33px;

    font-family: Taipei Sans TC Beta;
    font-style: normal;
    font-weight: normal;
    font-size: 16px;
    line-height: 43px;
    color: #FFFFFF;
    margin: 0 auto;
    

}
textarea {
    margin-top: 8px;
    padding-left: 15px;
    padding-right: 15px;
    padding-top: 10px;

    resize : none;
    width: 100%;
    height: 121px;
    background: linear-gradient(136.56deg, #FFFFFF -16.84%, #FFFFFF 102.5%);
    box-shadow: inset -3px -4px 8px rgba(255, 255, 255, 0.2), inset 3px 4px 10px rgba(54, 116, 101, 0.1);
        


    /*使用 border-image 最大的問題在於，設置的 border-radius 會失效。 */
    background: 
    linear-gradient(136.56deg, #FFFFFF -16.84%, #FFFFFF 102.5%) padding-box, /*this is your grey background*/
    linear-gradient(127.69deg, #C3DAD2 -23.43%, rgba(227, 227, 227, 0.22) 105.04%) border-box;

    border: 1px solid transparent;
    border-radius:10px;
    display:inline-block;
}
.text{
    font-family: Taipei Sans TC Beta;
    font-style: normal;
    font-weight: normal;
    font-size: 15px;
    vertical-align: bottom;
    /* 深灰 */

    color: #5C5C5C;

    

}

.datebox{
    position: relative;
}
#input-date{
    height: 38px;
    width: 118px;
    text-align: center;
    font-family: Taipei Sans TC Beta;
    font-style: normal;
    font-weight: normal;
    font-size: 16px;


    color: #5C5C5C;

}
/* 隱藏原本的icon */
.form-date__input[type="date"]::-webkit-calendar-picker-indicator {
		width: 100%;
		height: 100%;
		position: absolute;
		top: 0;
		left: 0;
		color: transparent;
		background: 0 0;
		margin: 0;
		opacity: 0;
		padding: 0;
		pointer-events: auto;
}
/*webkit瀏覽器專用*/
::-webkit-input-placeholder { 
    font-family: Taipei Sans TC Beta;
    font-style: normal;
    font-weight: normal;
    font-size: 15px;
    color: #C7C7C7; }
/*Firefox 4-18瀏覽器專用*/
input::-moz-placeholder { 
    font-family: Taipei Sans TC Beta;
    font-style: normal;
    font-weight: normal;
    font-size: 15px;
    color: #C7C7C7; }
/*Firefox 19+瀏覽器專用*/
input::-moz-placeholder{
    font-family: Taipei Sans TC Beta;
    font-style: normal;
    font-weight: normal;
    font-size: 15px;
    color: #C7C7C7;}
/*IE10瀏覽器專用*/
:-ms-input-placeholder{
    font-family: Taipei Sans TC Beta;
    font-style: normal;
    font-weight: normal;
    font-size: 15px;
    color: #C7C7C7;}
#intput_event_name{
    height: 38px;
    width: 280px;
    padding-left: 15px;
}
input{
    background: linear-gradient(136.56deg, #FFFFFF -16.84%, #FFFFFF 102.5%);
    box-shadow: inset -3px -4px 8px rgba(255, 255, 255, 0.2), inset 3px 4px 10px rgba(54, 116, 101, 0.1);
    


    /*使用 border-image 最大的問題在於，設置的 border-radius 會失效。 */
    background: 
     linear-gradient(136.56deg, #FFFFFF -16.84%, #FFFFFF 102.5%) padding-box, /*this is your grey background*/
     linear-gradient(127.69deg, #C3DAD2 -23.43%, rgba(227, 227, 227, 0.22) 105.04%) border-box;

    border: 1px solid transparent;
    border-radius:10px;
    display:inline-block;
    

}
.icon{
    display: inline-block;
    margin-right: 15px;
    width: 17px;
    height: auto;
    margin-left: 5px;
}
.background{    
    height: 637px;
    background: linear-gradient(180deg, #FFFFFF 42.2%, #9FFBDF 203.98%, #40E9D2 270.1%);
    overflow:hidden;
    
    padding-top: 35px;
}
#rating{
    margin-top: 15px;
}
#rating label{
    width: 28px;
    height: 28px;
    vertical-align: middle;
    margin-right: 5px;
    
}
#very-happy+label{
    background: url("../assets/svg/icons_face-very-happy.svg") no-repeat center;
    background-size:contain;
    
}
#very-happy+label:hover{
    background: url("../assets/svg/Aicons_face-very-happy.svg") no-repeat center;
    background-size:contain;
    
}
#very-happy:checked +label{
    background: url("../assets/svg/Aicons_face-very-happy.svg") no-repeat center;
    background-size:contain;
    
}
#happy+label{
    background: url("../assets/svg/icons_face-happy.svg") no-repeat center;
    background-size:contain;
    
}
#happy+label:hover{
    background: url("../assets/svg/Aicons_face-happy.svg") no-repeat center;
    background-size:contain;
    
}
#happy:checked +label{
    background: url("../assets/svg/Aicons_face-happy.svg") no-repeat center;
    background-size:contain;
    
}

#neutral+label{
    background: url("../assets/svg/icons_face-neutral.svg") no-repeat center;
    background-size:contain;
    
}
#neutral+label:hover{
    background: url("../assets/svg/Aicons_face-neutral.svg") no-repeat center;
    background-size:contain;
    
}
#neutral:checked +label{
    background: url("../assets/svg/Aicons_face-neutral.svg") no-repeat center;
    background-size:contain;
    
}

#sad+label{
    background: url("../assets/svg/icons_face-sad.svg") no-repeat center;
    background-size:contain;
    
}
#sad+label:hover{
    background: url("../assets/svg/Aicons_face-sad.svg") no-repeat center;
    background-size:contain;
    
}
#sad:checked +label{
    background: url("../assets/svg/Aicons_face-sad.svg") no-repeat center;
    background-size:contain;
    
}

#very-sad+label{
    background: url("../assets/svg/icons_face-very-sad.svg") no-repeat center;
    background-size:contain;
    
}
#very-sad+label:hover{
    background: url("../assets/svg/Aicons_face-very-sad.svg") no-repeat center;
    background-size:contain;
    
}
#very-sad:checked +label{
    background: url("../assets/svg/Aicons_face-very-sad.svg") no-repeat center;
    background-size:contain;
    
}
input[type="text"]:focus{
    outline: none;
}
textarea:focus{
    outline: none;
}

input[type="radio"] {
	display: none;
}


</style>
