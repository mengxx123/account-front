<template>
    <div class="page page-home">
        <header class="page-header">
            <ui-appbar title="帮助与反馈">
                <ui-icon-button icon="arrow_back" slot="left" @click="$router.go(-1)" />
            </ui-appbar>
        </header>
        <main class="page-body">
            <article>
                <h1>账号</h1>
                <h2>FAQ</h2>
                <p></p>
            </article>

            <div>我会前端/安卓，如何快速开发出一个 app？</div>

            <div>开发流程</div>
            <article>
                <h2>第一步：</h2>
                <p><a href="http://yunser.com/register" target="_blank">注册</a>云设账号</p>
                <p>登录开发者平台</p>
                <p>填写信息资料</p>
                <p>成为开发者</p>
                <p>创建应用（基本资料、图片上传、版权资料）</p>
                <p>应用上线</p>
                <p>开发者服务（数据监测，推广合作）</p>
                <hr>
                <div>开发者交流群：xxx</div>
            </article>
            <ui-list>
                <ui-list-item title="新手上路" to="/about"></ui-list-item>
                <ui-list-item title="常见问题" to="/about"></ui-list-item>
            </ui-list>
        </main>
    </div>
</template>

<script>
    import im from '@/util/im'

    export default {
        data () {
            return {
                messages: [],
                roster: [],
                groups: [],
                username: '15602229283',
                password: '123456',
                name: '15602229283',
                open: false,
                docked: true
            }
        },
        mounted() {
            im.init()

            this.username = this.$storage.get('username') || '15602229283'

            // 自动登录
            console.log('登录'+localStorage.token)
            if (!localStorage.user) {
//                this.$router.push('/login')
            }


            // 获取好友列表
            im.getFriends().then(roster => {
                console.log('121212')
                console.log(roster)
                this.roster = roster

                for (var i = 0, l = roster.length; i < l; i++) {
                    var ros = roster[i];
                    //ros.subscription值为both/to为要显示的联系人，此处与APP需保持一致，才能保证两个客户端登录后的好友列表一致
                    if (ros.subscription === 'both' || ros.subscription === 'to') {

                    }
                }
            }, err => {
                console.log(err)
            })

            // 列出当前登录用户加入的所有群组
//            conn.getGroup({
//                success: resp => {
//                    console.log("Response: ", resp)
//                    this.groups = resp.data
//                },
//                error: err => {
//                    console.log('获取群组失败')
//                }
//            })
            im.getGroups().then(groups => {
                this.groups = groups
            }, err => {
                console.log('获取群组失败', err)
            })

            // 获取本地消息列表
            this.messages = im.getMessages()

            im.setListener(() => {
                console.log('首页更新')
                this.messages = im.getMessages()
                console.log(this.messages)
            })
        },
        methods: {
            dealMessage(message) {
                if (message.type == 'text') {
                    return message.data
                }
                switch (message.type) {
                    case 'red_packet':
                        return '[微信红包]恭喜发财，大吉大利'
                    case 'location':
                        return '[位置]'
                    case 'image':
                        return '[图片]'
                    case 'video':
                        return '[视频]'
                    case 'link':
                        return '[链接]'
                }
            },
            toggle (flag) {
                this.open = !this.open
                this.docked = !flag
            },
            chat(message) {
                this.$router.push('/users/' + message.from + '/chat')
            },
            chatTo(ro) {
                console.log(ro)
                this.$router.push('/users/' + ro.name + '/chat')
            },
            groupChat(group) {
                console.log(group)
                this.$router.push(`/groups/${group.groupid}/chat`)
            },
            handleChange (val) {
                this.bottomNav = val
            },
            addFriend() {
                conn.subscribe({
                    to: this.name,
                    // Demo里面接收方没有展现出来这个message，在status字段里面
                    message: '加个好友呗!'
                });
            },
            removeMessage(e, message) {
                e.stopPropagation()
                im.removeMessage(message.from).then(() => {
                    this.messages = im.getMessages()
                })
            }
        }
    }
</script>

<style scoped>

</style>
