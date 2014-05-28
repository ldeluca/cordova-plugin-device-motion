<!---
    Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.
-->

# org.apache.cordova.device-motion

这个插件提供了对设备的加速度计的访问。 加速度计是动作感应器检测到的更改 (*三角洲*) 在相对于当前的设备方向，在三个维度沿*x*、 *y*和*z*轴运动。

## 安装

    cordova plugin add org.apache.cordova.device-motion
    

## 支持的平台

*   亚马逊火 OS
*   Android 系统
*   黑莓 10
*   火狐浏览器操作系统
*   iOS
*   Tizen
*   Windows Phone 7 和 8
*   Windows 8

## 方法

*   navigator.accelerometer.getCurrentAcceleration
*   navigator.accelerometer.watchAcceleration
*   navigator.accelerometer.clearWatch

## 对象

*   加速器

## navigator.accelerometer.getCurrentAcceleration

沿着*x*、 *y*和*z*轴，获取当前的加速度数据。

这些加速度值将返回到 `accelerometerSuccess` 回调函数。

    navigator.accelerometer.getCurrentAcceleration(accelerometerSuccess, accelerometerError);
    

### 示例

    function onSuccess(acceleration) {
        alert('Acceleration X: ' + acceleration.x + '\n' +
              'Acceleration Y: ' + acceleration.y + '\n' +
              'Acceleration Z: ' + acceleration.z + '\n' +
              'Timestamp: '      + acceleration.timestamp + '\n');
    };
    
    function onError() {
        alert('onError!');
    };
    
    navigator.accelerometer.getCurrentAcceleration(onSuccess, onError);
    

### iOS 的怪癖

*   iOS 不确定在任何给定的点获取当前加速器的概念。

*   你必须在给定的时间间隔内，查看加速器和捕获的数据。

*   因此，这个`getCurrentAcceleration`函数产生最后一个自 `watchAccelerometer` 请求的值。

## navigator.accelerometer.watchAcceleration

检索该设备的当前 `Acceleration` 间隔时间定期，执行 `accelerometerSuccess` 回调函数每次。 指定的时间间隔，以毫秒为单位通过 `acceleratorOptions` 对象的 `frequency` 参数。

返回的观看 ID 引用加速度计的手表时间间隔，并可以用 `navigator.accelerometer.clearWatch` 来停止看加速度计。

    var watchID = navigator.accelerometer.watchAcceleration(accelerometerSuccess,
                                                           accelerometerError,
                                                           [accelerometerOptions]);
    

*   **accelerometerOptions**： 具有以下可选的键的对象： 
    *   **frequency**：多久在千分之一秒内检索这个 `Acceleration`。 *(Number)* (Default: 10000)

### 示例

    function onSuccess(acceleration) {
        alert('Acceleration X: ' + acceleration.x + '\n' +
              'Acceleration Y: ' + acceleration.y + '\n' +
              'Acceleration Z: ' + acceleration.z + '\n' +
              'Timestamp: '      + acceleration.timestamp + '\n');
    };
    
    function onError() {
        alert('onError!');
    };
    
    var options = { frequency: 3000 };  // Update every 3 seconds
    
    var watchID = navigator.accelerometer.watchAcceleration(onSuccess, onError, options);
    

### iOS 的怪癖

API 调用在请求时间间隔内的成功回调函数，但到这个设备请求的限制范围是在 40ms和1000ms之间。 例如，如果请求的时间间隔为 3 秒(3000ms)， API 每隔 1 秒从设备中请求数据，但只有每隔 3 秒才执行成功回调。

## navigator.accelerometer.clearWatch

停止查看 `Acceleration` 引用的 `watchID` 参数。

    navigator.accelerometer.clearWatch(watchID);
    

*   **watchID**： 由返回的 ID`navigator.accelerometer.watchAcceleration`.

### 示例

    var watchID = navigator.accelerometer.watchAcceleration(onSuccess, onError, options);
    
    // ... later on ...
    
    navigator.accelerometer.clearWatch(watchID);
    

## 加速度

包含 `Accelerometer` 特定时间点采集到的加速器数据。 加速度值包括引力的影响 (9.81 m/s ^2)，因此当设备谎言平面和面朝上， *x*、 *y*，和*z*返回的值应该是 `` ， `` ，和`9.81`.

### 属性

*   **x**： 在 x 轴上的加速度量。(in m/s ^2)*(Number)*
*   **y**： 在 y 轴上的加速度量。(in m/s ^2)*(Number)*
*   **z**： 在 z 轴上的加速度量。(in m/s ^2)*(Number)*
*   **timestamp**： 创建时间以毫秒为单位。*(DOMTimeStamp)*