<template>
    <div>
        <h1 class="title">山口県 登山MAP</h1>
        <!-- ここにGooglemapを表示 -->
        <div id="map" style="height:500px; width:1100px;"></div>
    </div>
</template>

<style>
.title {
    text-align: center;
}
</style>

<script>
import { Loader } from "@googlemaps/js-api-loader";
// import { API_KEY } from "./googlemapapi.js";

export default {
    data() {
        return {
            loader: new Loader({
                apiKey: "AIzaSyBIFXaIXYU0ZghFK-pn6qHRiyeNYlxE450",
                version: "weekly",
                libraries: ["places"],
            }),
            mapOptions: {
                center: {
                    lat:35.6809591,
                    lng:139.7673068
                },
                zoom:15
            },
        };
    },

    mounted() {
        this.loader.load().then((google) => {
            if (navigator.geolocation) {
                // Geolocation API に対応している場合は現在地からGoogleMapを表示する
                navigator.geolocation.getCurrentPosition(
                    this.success,
                    this.error
                );
                console.log('きてるよ');
                return;
            }
            // Geolocation API に対応していない場合、デフォルトの値でGoogleMapを表示する
            this.createMap(this.mapOptions);
        });
    },
    methods: {
        success(position) {
            // 現在地の緯度・経度を取得する
            const latitude = position.coords.latitude;
            const longitude = position.coords.longitude;
            const center = { lat: latitude, lng: longitude };

            // マップ作成
            const mapOptions = { zoom: 15, center: center };
            console.log('A');
            const map = this.createMap(mapOptions);

            // 検索ボックスの初期化
            // this.initSearchBox(map);
        },
        error(position) {},
        createMap(mapOptions) {
            return new google.maps.Map(
                document.getElementById('map'),
                mapOptions
            );
        },
    },
};
</script>
