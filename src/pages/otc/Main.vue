<template>
    <div class="content-wrap">
        <div class="container" id="List" style='min-height:700px;background-color:#222327;margin-bottom:20px;'>
            <Row>
                <Col span="4">
                <Menu ref="navMenu" mode="vertical" width="auto" theme="light" :active-name="activeMenuName" @on-select="menuSelected">
                    <MenuGroup :title="$t('otc.transaction')" class='tradelist'>
                        <template v-for="(coin,index) in coins">
                        <MenuItem :name="'coin-'+index">
                            {{coin.unit}}
                        </MenuItem>
                        </template>
                    </MenuGroup>
                    <MenuItem name="ad">{{$t('otc.myad.title')}}</MenuItem>
                    <MenuItem name="order">{{$t('otc.myorder')}}</MenuItem>
                </Menu>
                </Col>
                <Col span="20">
                <!-- <keep-alive> -->
                <router-view></router-view>
                <!-- </keep-alive> -->
                </Col>
            </Row>
        </div>
    </div>
</template>
<style>
.ivu-table{
    background-color:transparent;
}
.list-table{
    background-color:transparent;
}
.ivu-table td,.ivu-table th{
    background-color:transparent !important;
    border-color:transparent;
}
.ivu-table-wrapper{
    border-color:transparent !important;
}
.ivu-table-row{
    border:1px solid transparent;
}
.tradeCenter .ivu-table-stripe .ivu-table-body tr.ivu-table-row-hover td,.tradeCenter .ivu-table-stripe .ivu-table-fixed-body tr.ivu-table-row-hover td{
    background-color:transparent;
}

.tradelist.ivu-menu-item-group .ivu-menu-item-group-title{
    padding-left: 0px;
    background-color:#323031;
    color:#D58C2A;
}
.ivu-menu-vertical .ivu-menu-item:hover, .ivu-menu-vertical .ivu-menu-submenu-title:hover{
     background-color:#323031;
}
</style>

<style scoped>
.content-wrap {
    background: #191c23;
    min-height: 450px;
    padding-top:20px;
}
.container {
    width: 80%;
    margin: 0 auto;
    min-width: 1200px;
    /* background:white; */
    padding-top: 30px;
}

</style>
<script>
export default {
    data() {
        return {
            coins:[],
            activeMenuName:'coin-0'
        }
    },
    methods: {
        init() {
            this.$store.commit('navigate','nav-otc');
            this.$http.post(this.host+this.api.otc.coin).then(response=>{
                if(response.body.code == 0){
                    this.coins = response.body.data;
                }
            });

        },
        menuSelected(menuName){
            if(menuName.startsWith('coin')){
                var coin = this.coins[menuName.split('-')[1]];
                this.$router.push('/otc/trade/'+coin.unit);
            }
            else{
                this.$router.push('/otc/'+menuName);
            }
        }
    },
    created: function() {
        this.init();
    },
    mounted:function(){
    }
}
</script>
