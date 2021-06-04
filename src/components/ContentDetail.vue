<template>
<!-- boar에서 고유 아이디값을 가지고 들어온 Detail vue -->
  <div class="container">
    <b-card>
      <div class="content-detail-content-info">
        <div class="content-detail-content-info-left">
          <div class="content-detail-content-info-left-number">
            {{this.contentId}}
          </div>
          <div class="content-detail-content-info-left-subject">
            {{this.title}}
          </div>
        </div>
        <div class="content-detail-content-info-right">
          <div class="content-detail-content-info-right-user">
            글쓴이: {{this.user}}
          </div>
          <div class="content-detail-content-info-right-created">
            등록일: {{this.created}}
          </div>
        </div>
      </div>
      <div class="content-detail-content">
        {{this.context}}
      </div>
      <div class="content-detail-button">
        <b-button variant="primary" @click="updateData">수정</b-button>
        <b-button variant="success" @click="deleteData">삭제</b-button>
      </div>
      <div class="content-detail-comment">
        <CommentList :contentId="contentId"/>
      </div>
    </b-card>
  </div>
</template>

<script>
import data from "@/data";
import CommentList from './CommentList';

export default {
  name: "ContentDetail",
  components:{
    CommentList
  },
  data() {
    const contentId = Number(this.$route.params.contentId); //글쓴값
    const contentData = data.Content.filter(item => item.content_id === contentId)[0] //Id값으로 가져온 고유데이터
//console.log(contentData)
    return {
      contentId:contentId,
      title: contentData.title,
      context: contentData.context,
      user: '',
      created: contentData.create_at,
    };
  },
  mounted(){
    const contentId = Number(this.$route.params.contentId); //글쓴값
    const contentData = data.Content.filter(item => item.content_id === contentId)[0] //Id값으로 가져온 고유데이터
    this.user = data.User.filter(item => item.user_id === contentData.user_id)[0].name
//console.log('ds',this.user)
  },
  methods: {
    updateData() {
//console.log(1)
      this.$router.push({    
        //잊지말자....data 는 this.로 불러야 한다........
        path: `/board/free/create/${this.contentId}`
      })
    },
    deleteData() {
      const content_index = data.Content.findIndex(item => item.content_id === this.contentId);
//console.log(content_index)
//[배열명].splice([항목 위치], [삭제할 항목 수], [추가할 항목] ㆍㆍㆍ);
//console.log(data)
      data.Content.splice(content_index, 1)
      this.$router.push({
        path: '/board/free'
      })
    },
  }
};
</script>

<style scoped>
.content-detail-content-info {
  display: flex;
  justify-content: space-between;
  background-color: lightcyan;
}
.content-detail-content-info-left {
  width: 720px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
}
.content-detail-content-info-right {
  width: 300px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 1rem;
}
.content-detail-content {
  margin-top: 1rem;
  padding-top: 1rem;
  min-height: 720px;
  
}
.content-detail-button {
  margin-top: 1rem;
  padding: 2rem;
}
.content-detail-comment {
  margin-top: 1rem;
  padding: 2rem;
  background-color: lightblue;
  border-radius: 20px;
}
</style>