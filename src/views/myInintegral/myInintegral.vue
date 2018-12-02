<template>
    <div>
        <Header title="我的积分" :left="true" :isabsolute="true"></Header>
        <MyIntegral></MyIntegral>
        <p class="detailtitle">积分明细</p>
        <x-table :cell-bordered="false" :content-bordered="false" style="background-color:#fff;">
            <thead>
                <tr style="background-color: #F7F7F7">
                    <th width="150">时间</th>
                    <th>姓名</th>
                    <th>积分数</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(item, index) in integralDetail.integralDetailList" :key="index">
                    <td>{{ item.time }}</td>
                    <td>{{ item.name }}</td>
                    <td>{{ item.integralAccount }}</td>
                </tr>
            </tbody>
        </x-table>
        <Page :total="integralDetail.total" :current-page="integralDetail.current" :display="integralDetail.display"  @pagechange="pagechange"></Page>
        <Footer></Footer>
    </div>
</template>

<script>
    import { XTable } from 'vux';
    import MyIntegral from '../home/components/myIntegral';
    import { userResource } from '../../api';
    import Page from '../my-components/page/page';
    import Header from '../my-components/app-header/app-header';
    import Footer from '../my-components/app-footer/app-footer';

    export default {
        components: {
            Header,
            Footer,
            XTable,
            MyIntegral,
            Page
        },
        data() {
          return  {
              integralDetail: {}
          }
        },
        mounted() {
            this.pagechange(1);
        },
        methods: {
            pagechange:function(currentPage){
                let { id } = this.$route.query;
                userResource.getIntegralDetailList(
                    id,
                    currentPage,
                    10
                ).then(response => {
                    let {code, data} = response;
                    if (code === 0) {
                        this.integralDetail = data;
                    }
                })
                // console.log(currentPage);
                // ajax请求, 向后台发送 currentPage, 来获取对应的数据

            }
        }
    }
</script>

<style lang="less" scoped>
    .detailtitle {
        height: 70px;
        line-height:70px;
        padding:0 30px;
        color: #333;
        font-size: 34px;
        border-bottom:1px solid #efefef;
        padding-top: 30px;
    }
</style>