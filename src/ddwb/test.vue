<template>
    <div class="artel-wrapper">
        <div class="header">
            <span>
                {{equipmentName}}
            </span>
        </div>
        <div class="content">
            <div class="flex-wrapper">
                <div class="main-content">
                    <div class="button-wrapper" @click="() => this.$router.go(-1)">
                        <span>返回</span>
                    </div>
                    <content-chart class="content-charts" :dataObj='airData' @changeTab='(obj, index) => getTab(obj, index, "air")'>
                        <a-carousel class="lunbo-wrapper" :dots='false' ref="airLunboWrapper">
                            <div class="lunbo" v-for="(item,index) in airData.arr" :key="index">
                                <artechart :item="item" :dataSource='item.data'></artechart>
                            </div>
                        </a-carousel>
                    </content-chart>
                    <content-chart class="content-charts" :dataObj='baseData' @changeTab='(obj, index) => getTab(obj, index, "base")'>
                        <a-carousel class="lunbo-wrapper" :dots='false' ref="baseLunboWrapper">
                            <div class="lunbo" v-for="(item,index) in baseData.arr" :key="index">
                                <arthchart :itemObj='item' :dataSource='item.data'></arthchart>
                            </div>
                        </a-carousel>
                    </content-chart>
                </div>
                <div class="main-content">
                    <div class="middle-content map">
                        <div class="content-header">
                            <div class="bar"></div>
                            <span>{{equipmentName}}站点图</span>
                        </div>
                        <div class="map-content" id="mapcontent"></div>
                    </div>
                    <div class="middle-content state">
                        <div class="content-header">
                            <div class="bar"></div>
                            <span>{{equipmentName}}系统状态/参数</span>
                            <div class="date-wrapper">
                                <span>更新时间</span>
                                <span>{{equipmentObj.updated_at}}</span>
                            </div>
                        </div>
                        <div class="table-content" v-if="equipmentObj.status">
                            <div class="item item1">
                                <span>经度坐标：</span>
                                <span>{{equipmentObj.status.long}}</span>
                            </div>
                            <div class="item item2">
                                <span>设备状态：</span>
                                <span style="color: #FFCB15">{{equipmentObj.status.deviceState}}</span>
                            </div>
                            <div class="item item3">
                                <span>纬度坐标：</span>
                                <span>{{equipmentObj.status.lat}}</span>
                            </div>
                            <div class="item item4">
                                <span>通信状态：</span>
                                <span style="color: #FFCB15">{{equipmentObj.status.communicationState}}</span>
                            </div>
                            <div class="item item5">
                                <div class="text-wrapper" v-for="(item,index) in iconData" :key="index">
                                    <img :src="item.imgUrl" width="25" height="30" class="icon">
                                    <div class="text-flex">
                                        <span>{{item.text.title}}</span>
                                        <span>{{item.text.num}}</span>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="main-content">
                    <content-chart style="margin-top:50px" class="content-charts" :dataObj='rainData' @changeTab='(obj, index) => getTab(obj, index, "rain")'>
                        <a-carousel class="lunbo-wrapper" :dots='false' ref="rainLunboWrapper">
                            <div class="lunbo" v-for="(item,index) in rainData.arr" :key="index">
                                <artechart :item="item" :dataSource='item.data' :chartType='"降雨量(mm)"'></artechart>
                            </div>
                        </a-carousel>
                    </content-chart>
                    <content-chart style="margin-top:20px" class="content-charts" :dataObj='soilData' @changeTab='(obj, index) => getTab(obj, index, "soil")'>
                        <a-carousel class="lunbo-wrapper" :dots='false' ref="soilLunboWrapper">
                            <div class="lunbo" v-for="(item,index) in soilData.arr" :key="index">
                                <artechart :item="item" :dataSource='item.data'></artechart>
                            </div>
                        </a-carousel>
                    </content-chart>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
const airData = {
    arr: [
        {
            name: '温度(℃)'
        },
        {
            name: '压力(kPa)'
        },
        {
            name: '湿度(%)'
        }
    ]
}
const baseData = {
    arr: [
        {
            name: '日',
            id: 'day'
        },
        {
            name: '月',
            id: 'month'
        },
        {
            name: '年',
            id: 'year'
        },
        {
            name: '空',
            id: 'null'
        }
    ]
}
const rainData = {
    arr: [
        {
            name: '小时'
        },
        {
            name: '日'
        },
        {
            name: '月'
        }
    ]
}
const soilData = {
    arr: [
        {
            name: '温度(℃)'
        },
        {
            name: '湿度(%)'
        },
        {
            name: 'PH值'
        }
    ]
}
const iconData = [
    {
        imgUrl: require('../../public/artel/大气温度.png'),
        text: {
            title: '大气温度',
            num: '24.1',
            key: 'ap_temp',
            unit: '℃'
        }
    },
    {
        imgUrl: require('../../public/artel/风速.png'),
        text: {
            title: '风速',
            num: '4.2',
            key: 'ap_wsp',
            unit: 'm/s'
        }
    },
    {
        imgUrl: require('../../public/artel/土壤温度.png'),
        text: {
            title: '土壤温度',
            num: '26.5',
            key: 'soil_temp',
            unit: '℃'
        }
    },
    {
        imgUrl: require('../../public/artel/大气湿度.png'),
        text: {
            title: '大气湿度',
            num: '61.3',
            key: 'ap_humi',
            unit: '%'
        }
    },
    {
        imgUrl: require('../../public/artel/风向.png'),
        text: {
            title: '风向',
            num: '',
            key: 'ap_wdd',
            unit: '°'
        }
    },
    {
        imgUrl: require('../../public/artel/土壤湿度.png'),
        text: {
            title: '土壤湿度',
            num: '85.5',
            key: 'soil_humi',
            unit: '%'
        }
    },
    {
        imgUrl: require('../../public/artel/大气压力.png'),
        text: {
            title: '大气压力',
            num: '101.8',
            key: 'ap_pre',
            unit: 'kPa'
        }
    },
    {
        imgUrl: require('../../public/artel/今日降雨量.png'),
        text: {
            title: '今日降雨量',
            num: '0',
            key: 'ap_rain',
            unit: 'mm'
        }
    },
    {
        imgUrl: require('../../public/artel/PH.png'),
        text: {
            title: '土壤PH值',
            num: '5.9',
            key: 'soil_ph',
            unit: ''
        }
    }
]
import { getLongitudeById } from '@/api/allapi'
import contentChart from '@/components/contentChart/contentChart'
import artechart from '@/components/artelEcharts/artechart'
import arthchart from '@/components/artelEcharts/arthchart'
import axios from 'axios'
import MapLoader from '@/utils/loadMap.js'
export default {
    name: 'artel',
    data () {
        return {
            airData,
            baseData,
            rainData,
            soilData,
            iconData,
            date: '',
            map:null,
            basename:'',
            equipmentName: '',
            equipmentObj: {},
            diqu:['兴宁市','五华县','丰顺县','大埔县','平远县','蕉岭县','梅江区','梅县区'],
        }
    },
    components: {
        contentChart,
        artechart,
        arthchart
    },
    created(){
        this._inits();
        this.equipmentName = window.sessionStorage.equipmentName
        axios.get("http://mziot.suntrans-cloud.com/getData").then(res => {
            if (res.status === 200) {
                res.data.data.forEach(item => {
                    if (item.id == this.$route.query.equipmentId) {
                        this.equipmentObj = item
                    }
                })
                if (this.equipmentObj.current) {
                    this.iconData.forEach(item => {
                        item.text.num = this.equipmentObj.current[item.text.key] + item.text.unit
                    })
                }
            }
        })
        this.$set(this.airData, 'name', this.equipmentName + '大气环境')
        this.$set(this.baseData, 'name', this.equipmentName)
        this.$set(this.rainData, 'name', this.equipmentName + '降雨量')
        this.$set(this.soilData, 'name', this.equipmentName + '土壤环境')
    },
    mounted(){
        this._drawCityMap();
        let that = this;
        window.addEventListener('done11', function(){
            getLongitudeById({baseId:that.$route.query.baseId}).then(res=>{
                if(res.success){
                    that.basename=res.result.baseName;
                    let lng=res.result.longitude;
                    let lat=res.result.latitude;
                    let marker1= new  AMap.CircleMarker({
                        radius:8,
                        topWhenClick:true,
                        zIndex:999,
                        fillColor:'#9d8d20',
                        strokeColor: '#9d8d20',
                        center: new AMap.LngLat(lng, lat),
                    });
                    that.map.add(marker1);
                    let marker= new  AMap.CircleMarker({
                        radius:5,
                        topWhenClick:true,
                        zIndex:999,
                        fillColor:'#ffcb15',
                        strokeColor: '#ffcb15',
                        center: new AMap.LngLat(lng, lat),
                    });
                    that.map.add(marker);
                    let text= new AMap.Text({
                        text:that.basename,
                        anchor:'top-center',
                        position:new AMap.LngLat(lng, lat),
                        offset:new AMap.Pixel(0,20),
                    })
                    that.map.add(text);
                    that.map.setFitView([marker1,marker,text]);
                }
            })
        })
    },
    methods: {
        _drawCityMap() {
            let that = this
            MapLoader().then(AMap => {
                let googleLayer = new AMap.TileLayer({
                    getTileUrl: 'http://mt{1,2,3,0}.google.cn/vt/lyrs=s&hl=zh-CN&gl=cn&x=[x]&y=[y]&z=[z]&s=Galile'
                });//定义谷歌卫星切片图层
                let roadNetLayer = new AMap.TileLayer.RoadNet({
                    opacity:0
                }); //定义一个路网图层
                that.map = new AMap.Map('mapcontent', {
                    zooms: [9,10],
                    layers:[googleLayer,roadNetLayer],
                    zoomEnable:false,
                    dragEnable: false,
                });

                AMap.plugin('AMap.DistrictSearch',function () {
                    var district=new AMap.DistrictSearch({
                        extensions:'all',
                        subdistrict:0
                    })
                    district.search('梅州市',function(status,result){
                        var bounds = result.districtList[0].boundaries
                        var outer = [
                            new AMap.LngLat(-360,90,true),
                            new AMap.LngLat(-360,-90,true),
                            new AMap.LngLat(360,-90,true),
                            new AMap.LngLat(360,90,true),
                        ];
                        var pathArray = [
                            outer
                        ];
                        pathArray.push.apply(pathArray,bounds)
                        var polygon = new AMap.Polygon( {
                            path:pathArray,
                            strokeColor: '#000000',
                            strokeWeight: 1,
                            fillColor: '#0f3b39',
                            fillOpacity: 1
                        });
                        polygon.setPath(pathArray);
                        that.map.add(polygon)
                        var polygons=[];
                        if (bounds) {
                            for (var i = 0, l = bounds.length; i < l; i++) {
                                //生成行政区划polygon
                                var polygon1 = new AMap.Polygon({
                                    map: that.map,
                                    strokeWeight: 3,
                                    path: bounds[i],
                                    fillOpacity: 0,
                                    fillColor: '#CCF3FF',
                                    strokeColor: '#13B068'
                                })
                                polygons.push(polygon1)
                            }
                            // 地图自适应
                            that.map.setFitView(polygons)
                        }
                    })
                    for(let i=0;i<that.diqu.length;i++){
                        district.search(that.diqu[i],function(status,result){
                            let bounds = result.districtList[0].boundaries
                            let polygons = new AMap.Polygon({
                                strokeWeight: 2,
                                path: bounds[0],
                                fillOpacity: 0,
                                fillColor: '#CCF3FF',
                                strokeColor: '#13B068'
                            })
                            that.map.add(polygons);     
                        })
                    }
                    that.map.on("complete", function(){
                        var myEvent = new CustomEvent('done11',{});
                        if(window.dispatchEvent) {
                            window.dispatchEvent(myEvent);
                        } else {
                            window.fireEvent(myEvent);
                        }
                    });
                })
            })
        },
        _inits() {
            axios.get("http://mziot.suntrans-cloud.com/getAtmosphereData",{params:{type:'1'}}).then((res)=>{
                if (res.status === 200) {
                    this.$set(this.airData.arr[0], 'data', res.data.data)
                }
            });
            axios.get("http://mziot.suntrans-cloud.com/getAtmosphereData",{params:{type:'2'}}).then((res)=>{
                if (res.status === 200) {
                    this.$set(this.airData.arr[1], 'data', res.data.data)
                }
            });
            axios.get("http://mziot.suntrans-cloud.com/getAtmosphereData",{params:{type:'3'}}).then((res)=>{
                if (res.status === 200) {
                    this.$set(this.airData.arr[2], 'data', res.data.data)
                }
            });
            //玫瑰图 wds_fqy 风速  wdd_fqy 风向
            axios.get("http://mziot.suntrans-cloud.com/chartWsrose").then((res)=>{
                if (res.status === 200) {
                    let arr = res.data.data.filter(item => {
                        return item.id == this.$route.query.equipmentId
                    })
                    if (arr.length > 0) {
                        this.$set(this.baseData.arr[0], 'data', arr[0].list.day)
                        this.$set(this.baseData.arr[1], 'data', arr[0].list.month)
                        this.$set(this.baseData.arr[2], 'data', arr[0].list.year)
                    }
                }
            });
            //降雨量
            axios.get("http://mziot.suntrans-cloud.com/getRainData").then((res)=>{
                if (res.status === 200) {
                    this.$set(this.rainData.arr[0], 'data', res.data.data.day)
                    this.$set(this.rainData.arr[1], 'data', res.data.data.month)
                    this.$set(this.rainData.arr[2], 'data', res.data.data.year)
                }
            });
            //土壤温度
            axios.get("http://mziot.suntrans-cloud.com/getSoilData",{params:{type:'1'}}).then((res)=>{
                if (res.status === 200) {
                    this.$set(this.soilData.arr[0], 'data', res.data.data)
                }
            });
            //土壤湿度
            axios.get("http://mziot.suntrans-cloud.com/getSoilData",{params:{type:'2'}}).then((res)=>{
                if (res.status === 200) {
                    this.$set(this.soilData.arr[1], 'data', res.data.data)
                }
            });
            //土壤ph
            axios.get("http://mziot.suntrans-cloud.com/getSoilData",{params:{type:'3'}}).then((res)=>{
                if (res.status === 200) {
                    this.$set(this.soilData.arr[2], 'data', res.data.data)
                }
            });
        },
        getDate() {
            setInterval(() => {
                let date = new Date().toString().split(' ')
                let month = new Date().getMonth() + 1
                let str = ''
                this.date = str + date[3] + '/' + month + '/' + date[2] + ' ' + date[4]
            }, 1000)
        },
        getTab(obj, index, prefix) {
            let str = prefix + 'LunboWrapper'
            this.$refs[str].goTo(index)
        }
    },
}
</script>
<style lang="stylus" scoped>
    @media screen and (max-width 1400px)
        .artel-wrapper
            .header
                font-size 35px !important
    .artel-wrapper
        background-image url('../../public/artel/background.png')
        height 100%
        overflow hidden
        color #13B068
        min-width 1400px
        padding-bottom 100px
        .header
            margin-top 40px
            height 70px
            background-image url('../../public/artel/header.png')
            background-repeat no-repeat
            text-align center
            font-size 40px
            width 100%
            position relative
            font-weight bold
            letter-spacing 8px
            span
                position absolute
                top -25px
                left 44%
        .content
            margin 17px 20px 0
            .button-wrapper
                width: 60px
                span
                    display inline-block
                    text-align center
                    background-color #13B068
                    border-radius 6px
                    width 100%
                    height 30px
                    line-height 30px
                    color #ffffff
                    font-weight 400
                    cursor pointer
            .flex-wrapper
                display flex
                justify-content space-between
    .main-content
        flex 1
        text-align center
        overflow hidden
        .lunbo
            height 390px
            width 100%
        .map
            height 610px
        .state
            margin-top 20px
            height 400px
            .date-wrapper
                flex 1
                display flex
                flex-direction column
                text-align right
                span
                    font-size 14px
                    height 15px
                    line-height 15px
                span:nth-of-type(1)
                    color #13B068
                    margin-bottom 5px
    .table-content
        display grid
        height: 310px
        grid-template-columns repeat(2, 50%)
        grid-template-rows: 65px 65px 180px
        gap 3px
        .item
            font-size 16px
            color #ffffff
            text-align center
            line-height 65px
            background-color #124440
        .item5
            display grid
            grid-column 1 / 3
            grid-template-columns repeat(3, 33.33%)
            grid-template-rows: repeat(3, 33.33%)
            .text-wrapper
                padding 15px 15px
                display flex
                font-size 14px
                img
                    margin-right 15px
                .text-flex
                    display flex
                    flex 1
                    text-align left
                    flex-direction column
                    line-height 15px
                    white-space nowrap
                    span:nth-of-type(1)
                        margin-bottom 5px
                        color #13B068
    .middle-content
        width 100%
        padding 20px 15px
        background-image url('../../public/artel/content.png')
        background-size 100% 100%
        background-repeat no-repeat
        .map-content
            width 100%
            height 530px
        .content-header
            height 30px
            display flex
            color #fff
            margin-bottom 20px
            letter-spacing 1px
            .bar
                height 100%
                width 5px
                border-radius 3px
                background-color #13B068
                margin-right 6px
            span
                line-height 30px
                font-size 16px
    .main-content:nth-of-type(2)
        margin 0 40px
</style>
