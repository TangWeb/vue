<!--项目中的车辆轨迹vue层-->
<template>
    <div>
        <div class="hello">
            <h1>{{ msg }}</h1>
            <h2>Essential Links</h2>
            <ul>
                <li><a href="https://vuejs.org" target="_blank">Core Docs</a></li>
                <li><a href="https://forum.vuejs.org" target="_blank">Forum</a></li>
                <li><a href="https://gitter.im/vuejs/vue" target="_blank">Gitter Chat</a></li>
                <li><a href="https://twitter.com/vuejs" target="_blank">Twitter</a></li>
                <br>
                <li><a href="http://vuejs-templates.github.io/webpack/" target="_blank">Docs for This Template</a></li>
            </ul>
            <h2>Ecosystem</h2>
            <ul>
                <li><a href="http://router.vuejs.org/" target="_blank">vue-router</a></li>
                <li><a href="http://vuex.vuejs.org/" target="_blank">vuex</a></li>
                <li><a href="http://vue-loader.vuejs.org/" target="_blank">vue-loader</a></li>
                <li><a href="https://github.com/vuejs/awesome-vue" target="_blank">awesome-vue</a></li>
            </ul>
        </div>

        <div id="amap-cointainer">

        </div>

        <div class="amap-page-container">
            <el-amap vid="amap" :zoom="zoom" :plugin="plugin" :center="center" :rotate-enable="rotateEnable" :map-style="mapStyle" :events="events">
                <el-amap-marker v-for="marker in markers" :position="marker.position" :events="marker.events" :visible="marker.visible" :draggable="marker.draggable" :title="marker.title" :content="marker.content"></el-amap-marker>
                <el-amap-polyline :editable="polyline.editable" :path="polyline.path" :stroke-weight="polyline.strokeWeight" :stroke-color="polyline.strokeColor" :events="polyline.events"></el-amap-polyline>
            </el-amap>
            <button type="button" name="button" v-on:click="toggleVisible">toggle first marker</button>
            <button type="button" name="button" v-on:click="changePosition">change position</button>
            <button type="button" name="button" v-on:click="chnageDraggle">change draggle</button>
            <button type="button" name="button" v-on:click="addMarker">add marker</button>
            <button type="button" name="button" v-on:click="removeMarker">remove marker</button>
        </div>
    </div>

</template>

<script>
    import { lazyAMapApiLoaderInstance } from 'vue-amap';
    import { AMapManager } from 'vue-amap';
    let amapManager = new AMapManager();
    const POLYGON_ID = 'POLYGON_ID';
    export default {
        data() {
            return {
                zoom: 14,
                center: [118.7810261011, 32.0392496684],
                rotateEnable: true,
                mapStyle: 'normal',
                events : {    //鼠标右击事件，回到中心点
                    'rightclick': (e) => {
                    console.log('ss');
            this.center = [this.center[0], this.center[1]];
        },
        },
//                plugin: [{    //定位插件，暂时没运行起来
//                    pName: 'Geolocation',
//                    events: {
//                        init(instance) {
//                            o.getCurrentPosition((status, result) => {
//                                this.center = [result.position.lng, result.position.lat];
//                            });
//                        },
//                        showMarker : true
//                    }
//                }],
            markers: [    //标记点
                {
                    position: [118.7810261011, 32.0392496684],
                    events: {
                        click: () => {
                        alert('click marker');
        },
            dragend: (e) => {
                this.markers[0].position = [e.lnglat.lng, e.lnglat.lat];
            }
        },
            visible: true,
                draggable: true,
                content : '<p style="width: 40px; color:red">19:27:20</p>',   //这里可以自定义项目中时间和坐标点的样式
                title : 'ss'
        },
            {
                position: [118.7850681011,32.0408566684],
                    events: {
                click: () => {
                    alert('click marker');
                },
                dragend: (e) => {
                    this.markers[0].position = [e.lnglat.lng, e.lnglat.lat];
                }
            },
                visible: true,
                    draggable: true,
                title : 'ss'
            },
            {
                position: [118.7945993323, 32.0394681003],
                    events: {
                click: () => {
                    alert('click marker');
                },
                dragend: (e) => {
                    this.markers[0].position = [e.lnglat.lng, e.lnglat.lat];
                }
            },
                visible: true,
                    draggable: true,
                title : 'ss'
            }

        ],
            polyline: {       //项目中可用这个作为车辆轨迹
                vid: POLYGON_ID,
                    path: [[118.7810261011, 32.0392496684], [118.7850681011,32.0408566684],[118.7945993323, 32.0394681003]],
                    events: {
                    click(e) {
                        alert('click polyline');
                    },
                    end: (e) => {
                        let newPath = e.target.getPath().map(point => [point.lng, point.lat]);
                        console.log(newPath);
                    }
                },
                strokeColor : '#000000',
                    editable: false,
                    strokeWeight :5

            }
        };
        },
        methods: {
            changePosition() {
                let position = this.markers[0].position;
                this.markers[0].position = [position[0] + 0.002, position[1] - 0.002];
            },
            chnageDraggle() {
                let draggable = this.markers[0].draggable;
                this.markers[0].draggable = !draggable;
            },
            toggleVisible() {
                let visableVar = this.markers[0].visible;
                this.markers[0].visible = !visableVar;
            },
            addMarker() {
                let marker = {
                    position: [121.5273285 + (Math.random() - 0.5) * 0.02, 31.21515044 + (Math.random() - 0.5) * 0.02]
                };
                this.markers.push(marker);
            },
            removeMarker() {
                if (!this.markers.length) return;
                this.markers.splice(this.markers.length - 1, 1);
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    h1, h2 {
        font-weight: normal;
    }

    ul {
        list-style-type: none;
        padding: 0;
    }

    li {
        display: inline-block;
        margin: 0 10px;
    }

    a {
        color: #42b983;
    }

    #amap-cointainer {
        height: 100%;
    }
    .amap-page-container{
        height: 400px;
        margin-top: 20px;
    }
</style>
