---
title: 基于Web的AR学习
date: 2018-01-28 16:53:55
tags: AR
---

#### 思路

- 获取移动设备的摄像头
- 截取视频流并展示在网页上
- 在视频画面上渲染图像
- 分析源，并识别出特定的Marker
- 将模型展示在Marker之上

#### 获取摄像头

##### WebRTC 技术

WebarRTC主要包含3部分：
- MediaStream
- RTCPeerConnection
- RTCDataChannel
这里借助于getUserMedia来获取MediaStream

MDN：https://developer.mozilla.org/zh-CN/docs/Web/API/WebRTC_API

##### example

在网页上获取截图：https://github.com/mdn/samples-server/tree/master/s/webrtc-capturestill

##### 处理兼容

![getUserMedia](web-ar/getUserMedia.png)

*chrome47版本后只支持https页面拉起摄像头

```javascript
navigator.getMedia = ( navigator.getUserMedia ||
                       navigator.webkitGetUserMedia ||
                       navigator.mozGetUserMedia ||
                       navigator.msGetUserMedia);

```

##### 需引入的库

这里采用了artoolkit，
artoolkit是个相对比较成熟的虚拟现实库：
[artoolkit](https://archive.artoolkit.org/)

##### 几种模型

- .mmd ------ MMDLoader
- .obj ------ OBJLoader
- .mtl ------ MTLLoader

##### demo
```javascript
window.ARThreeOnLoad = function() {

        ARController.getUserMediaThreeScene({maxARVideoSize: 320, cameraParam: './camera_para-iPhone.dat', facing: { exact: 'environment'},
            onSuccess: function(arScene, arController, arCamera) {

                document.body.className = arController.orientation;

                var renderer = new THREE.WebGLRenderer({antialias: true});
                if (arController.orientation === 'portrait') {
                    var w = (window.innerWidth / arController.videoHeight) * arController.videoWidth;
                    var h = window.innerWidth;
                    renderer.setSize(w, h);
                    renderer.domElement.style.paddingBottom = (w-h) + 'px';
                } else {
                    if (/Android|mobile|iPad|iPhone/i.test(navigator.userAgent)) {
                        renderer.setSize(window.innerWidth, (window.innerWidth / arController.videoWidth) * arController.videoHeight);
                    } else {
                        renderer.setSize(arController.videoWidth, arController.videoHeight);
                        document.body.className += ' desktop';
                    }
                }

                document.body.insertBefore(renderer.domElement, document.body.firstChild);


                // Create a couple of lights for our AR scene.
                var light = new THREE.PointLight(0xffffff);
                light.position.set(40, 40, 40);
                arScene.scene.add(light);

                var light = new THREE.PointLight(0xff8800);
                light.position.set(-40, -20, -30);
                arScene.scene.add(light);


                var markerObject3D = new THREE.Object3D()
                arScene.scene.add(markerObject3D);


                var mtlLoader = new THREE.MTLLoader();
                mtlLoader.setPath('/');
                mtlLoader.load('final-diandiandian.mtl', function(materials) {

                    materials.preload();

                    var objLoader = new THREE.OBJLoader();
                    objLoader.setMaterials(materials);
                    objLoader.setPath('/');
                    objLoader.load('final-diandiandian.obj', function(object) {
                        object.scale.set(2, 2, 2).multiplyScalar(1 / 20);
                        object.rotation.x = Math.PI / 2;
                        markerObject3D.add(object);
                    }, onProgress, onError);

                });

                function onProgress(xhr) {
                    console.log( ( xhr.loaded / xhr.total * 100 ) + '% loaded' );
                }

                function onError() {
                    console.log( 'An error happened' );
                }

                var flag = 1;

								// 添加事件监听， 转动坐标轴
                renderer.domElement.addEventListener('click', function(ev) {
                    markerObject3D.rotation.z= flag * Math.PI/2;
                    if(flag > 4) {
                        var imgNode=document.createElement("IMG");
                        imgNode.src = './red.png';
                        imgNode.className = 'redbag';
                        document.body.appendChild(imgNode);
                    } else {
                        flag++;
                    }
                }, false);

                arController.loadMarker('meituan.patt', function(markerId) {
                    var markerRoot = arController.createThreeMarker(markerId);
                    markerRoot.add(markerObject3D);
                    arScene.scene.add(markerRoot);
                });


                var tick = function() {
                    arScene.process();

                    arScene.renderOn(renderer);
                    requestAnimationFrame(tick);
                };

                tick();

            }});

        delete window.ARThreeOnLoad;

    };

    if (window.ARController && ARController.getUserMediaThreeScene) {
        ARThreeOnLoad();
    }
```

##### 参考

- [Web前端也能做的AR互动](http://tgideas.qq.com/webplat/info/news_version3/804/7104/7106/m5723/201612/537832.shtml)
- [实现一个简单但有趣的AR效果（Web）](https://aotu.io/notes/2017/03/24/webar/index.html)
- [Augmented Reality in 10 Lines of HTML](https://medium.com/arjs/augmented-reality-in-10-lines-of-html-4e193ea9fdbf)
