<template>
    <div class="playlists-container">
        <!-- 同步 -->
        <div class="top-card">
            <div class="icon-wrap">
                <!-- 封面 -->
                <img :src="topList.coverImgUrl" alt="" />
            </div>
            <div class="content-wrap">
                <div class="tag">精品歌单</div>
                <!-- 标题 -->
                <div class="title">
                    {{ topList.name }}
                </div>
                <!-- 介绍 -->
                <div class="info">
                    {{ topList.description }}
                </div>
            </div>
            <!-- 背景 -->
            <img :src="topList.coverImgUrl" alt="" class="bg" />
            <div class="bg-mask"></div>
        </div>
        <div class="tab-container">
            <!-- tab栏 顶部 -->
            <div class="tab-bar">
                <div v-for="(genre, index) in tags" :key="index">
                    <span class="item" :class="{ active: tag == genre }" @click="tag = genre"> {{ genre }}</span>
                </div>
            </div>
            <!-- tab的内容区域 -->
            <div class="tab-content">
                <div class="items">
                    <div class="item" v-for="(item, index) in list" :key="index">
                        <div class="img-wrap">
                            <div class="num-wrap">
                                播放量:
                                <span class="num">{{ item.playCount }}</span>
                            </div>
                            <img :src="item.coverImgUrl" alt="" />
                            <span class="iconfont icon-play"></span>
                        </div>
                        <p class="name">{{ item.name }}</p>
                    </div>
                </div>
            </div>
        </div>
        <!-- 分页器
      total 总条数
      current-page 当前页
      page-size 每页多少条数据
      current-change 页码改变事件
     -->
        <el-pagination
            @current-change="handleCurrentChange"
            background
            layout="prev, pager, next"
            :total="total"
            :current-page="page"
            :page-size="10"
        ></el-pagination>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'recommend',
    data() {
        return {
            // 总条数
            total: 0,
            // 页码
            page: 1,
            topList: {},
            tags: ['全部', '欧美', '华语', '流行', '说唱', '摇滚', '民谣', '电子', '轻音乐', '影视原声', 'ACG', '怀旧'],
            lists: [],
            tag: '全部',
        }
    },
    watch: {
        tag() {
            // console.log(this.tag)
            // 顶部的 精品歌单
            this.topData()
            // 歌单列表
            this.listData(),
                // 修改页码为1
                (this.page = 1)
        },
    },
    created() {
        // 顶部的 精品歌单
        this.topData()
        // 歌单列表
        this.listData()
    },
    methods: {
        topData() {
            axios({
                url: 'https://autumnfish.cn/top/playlist/highquality',
                method: 'get',
                params: {
                    limit: 1,
                    // 分类数据
                    cat: this.tag,
                },
            }).then(res => {
                // console.log(res)
                this.topList = res.data.playlists[0]
            })
        },
        // 抽取的方法2 列表数据
        listData() {
            axios({
                url: 'https://autumnfish.cn/top/playlist/',
                method: 'get',
                params: {
                    limit: 10,
                    // 起始的值 （页码-1）*每页多少条数据
                    offset: (this.page - 1) * 10,
                    // 分类数据
                    cat: this.tag,
                },
            }).then(res => {
                // console.log(res)
                // 保存总条数
                this.total = res.data.total
                // 保存数据
                this.list = res.data.playlists
            })
        },
        handleCurrentChange(val) {
            // console.log(`当前页: ${val}`)
            // 保存页码
            this.page = val
            // 重新获取数据
            this.listData()
        },
    },
}
</script>

<style >
</style>
