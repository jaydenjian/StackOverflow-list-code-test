<template>
<div class="comment" >
    <a :href="item.link" class="commentTitle" target="__blank">{{item.title}}</a>
    
    <div class="commentInfo">
        <div class="score">
            <div class="numTitle">Score</div>
            <div class="commentScore numbers" :class="{'text_red':item.score<0}">{{item.score}}</div>
        </div>
        <div class="answers">
            <div class="numTitle">Answers</div>
            <div class="commentAnswers numbers" v-if="item.answer_count<1">{{item.answer_count}}</div>
            <div class="commentAnswers numbers" v-if="item.answer_count>0" :class="[item.is_answered?'bg_green':'border_green']">{{item.answer_count}}</div>            
        </div>
        <div class="viewed">
            <div class="numTitle">Viewed</div>
            <div class="commentViewed numbers">{{item.view_count}}</div>
        </div>
    </div>    
    <div class="owner">
        <img :src="item.owner.profile_image" alt="" class="ownerPic">
        <div class="ownerName">{{item.owner.display_name}}</div>
    </div>
    <!-- <p>{{item.body}}</p> -->
</div>
</template>

<script>
export default {
    name: "comment",
    props:{
        item: Object,
        // tags: Array,
        // owner: Object,
        // view_count: Number,
        // answer_count: Number,
        // score: Number,
        // title: String,
        // question_id: Number,
    },

}
</script>

<style scoped>
/* *{
    border: solid 1px red;
} */
.comment{
    width:100%;
    margin-top: 15px;
    /* height: 130px; */
    border-bottom: solid 1px grey;
    position: relative;
    display: flex;
    height: 150px;    
}
.commentTitle{
    font-size: 20px;
    height: 80px;
    width: 600px;
    text-decoration: none;
    color: black;
}
.commentTitle:hover{
    color: gray;
}
.commentInfo{
    position: absolute;
    bottom: 0;
    width: 80%;
    height: 60px;
    display: flex;
    justify-content: space-evenly;
}
.numTitle{
    color: red;
    font-weight: bold;
}
.numbers{
    margin-top: 5px;
    text-align: center;
}
.text_red{
    color: red;
}
.border_green{
    border: solid 1px #008100;
    color: #008100;
}
.bg_green{
    background-color: #008100;
    color: white;
}

.owner{
    display:flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    /* position: absolute;
    right:0; */
    width:150px;
    margin-left: auto;
    
}
.ownerPic{
    width: 100px;
    border-radius: 50%;
    
}
.ownerName{
    width: 100px;
    text-align: center;
    word-break: break-word;
}
</style>