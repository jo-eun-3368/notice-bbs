<template>
    <div>
        <div class="comment-list-item">
            <div class="comment-list-item-name">
                <div>{{this.user.name}}</div>
                <div>{{this.commentObj.created_at}}</div>
            </div>
            <div class="comment-list-item-context">{{this.commentObj.context}}</div>
            <div class="comment-list-item-button">
                <b-button variant="info" @click="updateData">수정</b-button>
                <b-button variant="info" @click="$emit('deleteData',this.user)">삭제</b-button>
                <b-button variant="info" @click="subCommentToggle">덧글달기</b-button>
            </div>
        </div>
        <template v-if="this.subCommentList.length>0">
            <div
                class="comment-list-item-subcomment-list"
                :key="item.subcomment_id"
                v-for="item in subCommentList"
                >
                <div class="comment-list-item-name">
                <div>{{item.user_name}}</div>
                <div>{{item.created_at}}</div>
            </div>
            <div class="comment-list-item-context">{{item.context}}</div>
            <div class="comment-list-item-button">
                <b-button variant="coral">수정</b-button>
                <b-button variant="coral">삭제</b-button>
            </div>
            </div>
        </template>
        <template v-if="subCommentCreateToggle">
            <CommentCreate :list="subCommentList" :isSubComment="true" :commentId="commentObj.comment_id" :reloadSubComments="reloadSubComments" :subCommentToggle="subCommentToggle"/>
        </template>
    </div>
</template>
<script>
import data from '@/data';
import CommentCreate from './CommentCreate';

export default {
    name:'CommentListItem',
    components:{
        CommentCreate
    },
    props:{
        commentObj:Object,
        //타입선언
    },
     data() {
        return {
            user : {},
            name : '',
            subCommentList : '',
            subCommentCreateToggle:false,
        };
    },
    mounted() {
        this.user = data.User.filter(item=>item.user_id === this.commentObj.user_id)[0];

        this.subCommentList = data.subComment.filter(item=> { return item.comment_id === this.commentObj.comment_id });
        
        // subCommentfilter.map((filter) => {
            
        //     if(filter.user_id === this.user.id) {
        //         this.subCommentList.push(filter)
        //     }
        // })

        // this.subCommentList = data.SubComment.filter(
        //         item=>item.comment_id === this.commentObj.comment_id
        //         ).map((subCommentItem)=>({
        //         ...subCommentItem,
        //         user_name:data.User.filter(
        //             item=>item.user_id===subCommentItem.user_id
        //         )[0].name}));


    },
    methods:{
        subCommentToggle(){
            this.subCommentCreateToggle = !this.subCommentCreateToggle;
        },
        reloadSubComments(){
            this.subCommentList=data.subComment.filter(
                item=>item.comment_id===this.commentObj.comment_id
            ).map(subCommentItem=>({
                ...subCommentItem,
                user_name: data.User.filter(
                    item=>item.user_id===subCommentItem.user_id
                )[0].name
            }));
            console.log("methods.subCommentList",this.subCommentList)
        },
        //댓글수정로직
        updateData(){
            console.log(1)

        },
        //댓글삭제로직()
        deleteData(){
            console.log(this.commentObj)

            //this.$emit('remove:',)
        }
    },
}
</script>
<style scoped>
.comment-list-item {
  display: flex;
  justify-content: space-between;
  padding-bottom: 1em;
}
.comment-list-item-name {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: cornflowerblue;
  color: white;
  width: 15%;
  padding: 0.5em;
  border-radius: 10px;
}
.comment-list-item-context {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 50em;
}
.comment-list-item-button {
  align-items: center;
  width: 30%;
  display: flex;
}
.btn {
  width: 100px;
  margin-right: 10px;
}
.comment-list-item-subcomment-list {
  display: flex;
  justify-content: space-between;
  margin-left: 10em;
  background-color: lightblue;
  border-radius: 10px;
}
</style>
