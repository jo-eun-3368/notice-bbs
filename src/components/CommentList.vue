<template>
<!-- commendId갑을 Detail에서 전달받앗음 -->
    <div> 
        <!-- 바인딩 속성은 무조건 div로 감싸서 사용해주어야함...... -->
        <div :key="item.comment_id" v-for="item in comments">
            <!-- 속성 넘겨주기 -->
            <CommentListItem @deleteData="deleteData" :commentObj="item"/>
        </div>
        <CommentCreate :contentId="contentId"  :reloadComment="reloadComment" />
    </div>
</template>
<script>
import data from '@/data';
import CommentListItem from './CommentListItem';
import CommentCreate from './CommentCreate';

export default {
    name:'CommentList',
    
    components:{
        CommentListItem,
        CommentCreate,
    },
    props:{
        //contentId를 가져온다.
        contentId: Number,
    },
    data() {
        return {
            comments: '',
            commentObj:'',
        }
    },
    mounted(){
        //console.log("리스트",data.Comment) //2
        //console.log("리스트",this.contentId) //2
        this.comments = data.Comment.filter(item => item.content_id === this.contentId);
        //console.log("리스트",this.comments)
    },
    methods:{
        //댓글이 입력시 바로생성 로직
        reloadComment(){
            this.comments = data.Comment.filter(item => item.content_id === this.contentId) 
        },
        deleteData(){
            console.log('emit')
            console.log(this.user)
        }
    }
}
</script> 