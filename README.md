<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Google Mapsで位置情報を表示する</title>
    <style>
        #map {
            height: 600px;
            width: 100%;
        }
    </style>
</head>
<body>
    <h1>Google Mapsで位置情報を表示する</h1>
    <div id="map"></div>

    <script>
        // 初期化関数
        function initMap() {
            // デフォルトの位置（東京駅）
            const defaultLocation = { lat: 35.681236, lng: 139.767125 };
            const map = new google.maps.Map(document.getElementById("map"), {
                zoom: 15,
                center: defaultLocation,
            });

            let markers = [];

            // 現在位置を取得して地図を更新する関数
            function updateMapWithCurrentLocation(position) {
                const currentPosition = {
                    lat: position.coords.latitude,
                    lng: position.coords.longitude
                };

                // 新しいピンを立てる
                const marker = new google.maps.Marker({
                    position: currentPosition,
                    map: map,
                    title: '現在地 (' + new Date().toLocaleTimeString() + ')',
                });
                markers.push(marker);

                // ピンを立てる位置情報
                const locations = [
                    { lat: 35.681236, lng: 139.767125, description: '東京駅' },
                    { lat: 35.6585805, lng: 139.7454329, description: '渋谷駅' },
                    // ここにPythonスクリプトで収集した位置情報を追加する
                ];

                // 固定の場所にピンを立てる
                locations.forEach(location => {
                    const marker = new google.maps.Marker({
                        position: { lat: location.lat, lng: location.lng },
                        map: map,
                        title: location.description,
                    });
                    markers.push(marker);
                });

                // 現在位置にセンタリングする
                map.setCenter(currentPosition);
            }

            // 位置情報を取得してマップを更新する関数を10分ごとに実行する
            function getLocationAndUpdateMap() {
                if (navigator.geolocation) {
                    navigator.geolocation.getCurrentPosition(
                        updateMapWithCurrentLocation,
                        function() {
                            alert('現在位置を取得できませんでした。デフォルトの位置（東京駅）を表示します。');
                        },
                        {
                            enableHighAccuracy: true,  // 高精度の位置情報を要求する
                            timeout: 5000,  // 5秒以内に位置情報を取得する
                            maximumAge: 0   // キャッシュを使用しない
                        }
                    );
                } else {
                    alert('このブラウザは位置情報に対応していません。デフォルトの位置（東京駅）を表示します。');
                }
            }

            // 初回の位置情報取得と地図更新
            getLocationAndUpdateMap();
            // 10分毎に位置情報を更新
            setInterval(getLocationAndUpdateMap, 10 * 60 * 1000); // 10分 = 10 * 60 * 1000ミリ秒
        }
    </script>
    <!-- Google Maps JavaScript APIを読み込む -->
    <script src="https://maps.googleapis.com/maps/api/js?key=AIzaSyDdn6b6L1wCTCgXAapOqxYdMm0ENd_J3ls&callback=initMap" async defer></script>
</body>
</html>

