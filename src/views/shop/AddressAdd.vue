<template>
    <div class="page page-home">
        <header class="page-header">
            <ui-appbar title="添加新地址">
                <ui-icon-button icon="arrow_back_ios" slot="left" @click="$router.go(-1)" />
                <ui-flat-button color="white" label="保存" slot="right" @click="save"/>
            </ui-appbar>
        </header>
        <main class="page-body">
            <ui-content-block>
                <div>
                    <ui-text-field v-model="address.receiver" label="收货人" labelFloat/><br/>
                    <ui-text-field v-model="address.phone" label="联系电话" labelFloat/><br/>
                    <ui-text-field v-model="address.county" label="所在地区" labelFloat/><br/>
                    <ui-text-field v-model="address.area" label="街道" labelFloat/><br/>
                    <ui-text-field v-model="address.detail" label="详细地址" labelFloat/><br/>
                    <ui-checkbox label="设为默认地址" class="demo-checkbox"/> <br/>
                </div>
            </ui-content-block>
        </main>
    </div>
</template>

<script>
    export default {
        data () {
            return {
                address: {
                    receiver: '小云',
                    phone: '15602221234',
                    county: '广东省 广州市 天河区',
                    area: '中山大道西105号',
                    detail: '起点大厦1024',
                    isDefault: true
                }
            }
        },
        mounted() {
        },
        methods: {
            save() {
                let userId = this.$storage.get('user').id
                this.$http.post(`/addresses`, {
                    user_id: userId,
                    receiver: this.address.receiver,
                    area_id: '1', // TDOO
                    detail: this.address.detail,
                    phone: this.address.phone,
                    is_default: this.address.isDefault ? 1 : 0,
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
