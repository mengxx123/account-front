<template>
    <div class="page page-home">
        <header class="page-header">
            <ui-appbar title="添加文章">
                <ui-icon-button icon="arrow_back_ios" slot="left" @click="$router.go(-1)" />
            </ui-appbar>
        </header>
        <main class="page-body">
            <ui-content-block>
                <input v-model="title">
                <textarea v-model="content"></textarea>
                <button @click="add">添加</button>
            </ui-content-block>
        </main>
        <ui-float-button class="ui-float-btn" icon="add" to="/me/articles/add"/>
    </div>
</template>

<script>
    export default {
        data () {
            return {
                title: '这是标题',
                content: '这是内容'
            }
        },
        mounted() {
            console.log(new Date().getTime())
        },
        methods: {
            add() {
                let userId = this.$storage.get('user').id
                this.$http.post(`/articles`, {
                    user_id: userId,
                    title: this.title,
                    content: this.content
                })
                    .then(response => {
                        let data = response.data
                        console.log(data)
                        if (data.code === 0) {
                            this.$router.go(-1)
                        }
                    },
                    response => {
                        console.log(response)
                    })
            }
        }
    }
</script>

<style scoped>
</style>
