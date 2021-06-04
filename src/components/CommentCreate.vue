<template>
    <div class="comment-create">
        <b-input-group class="mt-3">
            <input type="text" placeholder="닉네임을 입력해주세요" v-model="name">
            <b-form-textarea
            id="textarea"
            v-model="context"
            :placeholder="'코멘트를 달아주세요'"
            rows="3"
            max-rows="6"
            @keyup.enter="createComment"
            >
            </b-form-textarea>
            <b-input-group-append>
                <b-button variant="info" @click="isSubComment ? createSubComment() : createComment()">작성하기</b-button>
            </b-input-group-append>
        </b-input-group>
    </div>
</template>
<script>
import data from '@/data';

export default {
    name:'CommentCreate',
    props:{
        contentId:Number,
        commentId: Number,
        isSubComment:Boolean,
        reloadComment: Function,
        subCommentToggle:Function,
        reloadSubComments:Function,
        list:Array
    },
    data() {
        return {
            name:"",
            context:""
        };
    },
    mounted(){

    },
    methods:{
        createComment(){
            if(this.context===''){
                alert('값을 입력해주세요')
            }else{
            let today = new Date();
            data.Comment.push({
                comment_id:data.Comment[data.Comment.length-1].comment_id+1,
                content_id:this.contentId,
                context:this.context,
                created_at:today.getFullYear(),
                updated_at:null
            });
            data.User.push({
                name:this.name,
            })
            alert('0');
            this.reloadComment();
            this.context = "";
            this.name="";
            }
            
        },
        createSubComment(){
            let today = new Date();

            this.list.push({
                subcomment_id:data.subComment[data.subComment.length-1].subcomment_id+1,
                user_id:1,
                comment_id:this.commentId,
                context:this.context,
                created_at:today.getFullYear(),
                updated_at:null
            });

            console.log(data.subComment);
            this.subCommentToggle();
            this.reloadSubComments
            this.context = "";
        }
    }
}
</script>
</script>