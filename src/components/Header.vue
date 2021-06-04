<template>
    <div>
        <b-navbar  toggleable="lg" type="dark" variant="info">
            <div class="container">
                <b-navbar-brand href="/">김조은 게시판 만들기</b-navbar-brand>
                <b-navbar-toggle target="nav_collapse"/>
                <b-collapse is-nav id="nav_collapse">
                    <b-navbar-nav>
                        <b-nav-item href="#">공지사항</b-nav-item>
                        <b-nav-item to="/board/free">자유게시판</b-nav-item>
                        <b-nav-item href="#">구인구직</b-nav-item>
                    </b-navbar-nav>
                </b-collapse>
            </div>
        </b-navbar>
    </div>
</template>

<script>
export default {
    name:"Header",
    data(){
        return {};
    }
}
</script>