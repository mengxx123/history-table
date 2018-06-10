<template>
    <my-page title="纪年换算">
        <div class="search-bar">
            <input class="input" v-model="keyword" placeholder="输入年份或年号搜索" />
            <ui-icon-button icon="close" color="#999" v-if="keyword.length" @click="keyword = ''" />
        </div>
        <ui-article class="article">
            <p v-if="!filterDisplay.length">暂无搜索结果~</p>
            <p v-if="filterDisplay.length">注：暂时只支持元朝之后的朝代。</p>
            <table v-if="filterDisplay.length">
                <tr>
                    <th>公元</th>
                    <th>名称</th>
                </tr>
                <tr v-for="item in filterDisplay">
                    <td>{{ item.year }}</td>
                    <td>{{ item.name }}</td>
                </tr>
            </table>
        </ui-article>
    </my-page>
</template>

<script>
    import data from '../util/data'

    export default {
        data () {
            return {
                keyword: '',
                data: data,
                display: []
            }
        },
        computed: {
            filterDisplay() {
                if (!this.keyword) {
                    return this.display
                }
                return this.display.filter(item => {
                    return ('' + item.year).includes(this.keyword) ||
                        item.name.includes(this.keyword)
                })
            }
        },
        mounted() {
            for (let item of this.data) {
                let idx = 1
                if (!item.endYear) {
                    item.endYear = item.startYear + item.last
                }

                for (let i = item.startYear; i <= item.endYear; i++) {
                    this.display.push({
                        year: i,
                        name: item.name + idx++ + '年'
                    })
                }
            }
        }
    }
</script>

<style lang="scss" scoped>
$divider: rgba(0,0,0,.12);

.search-bar {
    position: absolute;
    top: 0;
    left: 2px;
    right: 0;
    z-index: 10000;
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 64px;
    background-color: #fff;
    border-bottom: 1px solid $divider;
    .input {
        flex-grow: 1;
        height: 100%;
        padding: 0 16px;
        line-height: 64px;
        border: none;
        outline: none;
    }
}
.article {
    margin-top: 64px;
}
</style>
