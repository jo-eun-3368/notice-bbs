<template>
    <div class="container" style="padding:40px 0 ">
        <b-input v-model="subject" placeholder="제목을 입력해 주세요" style="margin-bottom:10px"></b-input>
        <b-form-textarea
        id="textarea"
        v-model="context"
        placeholder="내용을 입력해 주세요"
        rows="3"
        max-rows="6" style="margin-bottom:10px"
        >
        </b-form-textarea>
        <b-button @click="updateMode ? updateContent() : uploadContent()">저장</b-button>
        <b-button @click="cancle">취소</b-button>
    </div>
</template>
<script>
import data from '@/data'


export default {
    name:'Create',
    data(){
        return{
            subject:'',
            context:'',
            userId: 1, //Id값을 반환해주어야 함 => data값에 고정값으로 넣어주었기때문에 1로 고정해놓음
            createdAt: '2019-04-17 11:32:42',
            updateAt:null,
            updateObject:null,
            //기존에 있던 것을 수정해주는로직 params 의 id값이 
            updateMode:this.$route.params.contentId>0?true:false
        }
    }, 
    //빈보드가 띄어지ㅣ자마자
    created(){
        //contentId를 가져왔을경우 진행
        if(this.$route.params.contentId>0){
            const contentId = Number(this.$route.params.contentId)
            this.updateObject=data.Content.filter(item=>item.content_id===contentId)[0]
            //item객체중에 id값이 같은걸 가져오고
            this.subject= this.updateObject.title;
            this.context=this.updateObject.context;
        }
    },
    methods:{
        uploadContent(){
             let items = data.Content.sort((a,b)=>{return b.content_id - a.content_id}) //역순으로 정렬할때
             //content_id 1씩 증가시키기
             const content_id = items[0].content_id + 1
            //data에 content배열로 새로생성되도록 넣어주어야한다.
            data.Content.push({
                content_id:content_id,
                user_id:this.userId,
                title:this.subject, 
                context:this.context,
                created_at:this.createdAt,
                updated_at:null,
            })
            this.$router.push({
                path:'/board/free/'
            })
        },
        //가져온 객체의 title을 subject로 대체한다
        updateContent(){
            this.updateObject.title=this.subject;
            this.updateObject.context=this.context;
            this.$router.push({
                path:'/board/free/'
            })
        },
        cancle(){
            this.$router.push({
                path:'/board/free'
            })
        },
    }
}
</script>