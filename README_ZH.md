# AdvancedLuban
[![build](https://img.shields.io/badge/build-1.1.3-brightgreen.svg?maxAge=2592000)](https://bintray.com/shaohui/maven/AdvancedLuban)
[![license](https://img.shields.io/badge/license-Apache%202-blue.svg?maxAge=2592000)](https://github.com/shaohui10086/AdvancedLuban/blob/master/LICENSE)


`AdvancedLuban` —— 是一个方便简约的 `Android` 图片压缩工具库，提供多种压缩策略（包括`Luban`原有的压缩策略），多种调用方式，自定义压缩，多图同步压缩，专注更好的图片压缩使用体验


照片数量 | 原图总大小 | 压缩后总大小 | 总耗时
--- | --- | --- | ---
1 | 4.3Mb | 85Kb | 0.23s
4 | 14.22Mb | 364Kb | 1.38s
9 | 36.23Mb | 745Kb | 4.43s

## Import

Maven

    <dependency>
      <groupId>me.shaohui.advancedluban</groupId>
      <artifactId>library</artifactId>
      <version>1.1.3</version>
      <type>pom</type>
    </dependency>

    
or Gradle

	compile 'me.shaohui.advancedluban:library:1.1.3'

## Usage


### `Listener`方式

`AdvancedLuban`内部采用`Computation`线程进行图片压缩，外部调用只需设置好结果监听即可：

    Luban.get(this)                     // 初始化Luban
        .load(File)                     // 传人要压缩的图片
        .putGear(Luban.THIRD_GEAR)      // 设定压缩模式，默认 THIRD_GEAR
        .launch(listener);              // 启动压缩并设置监听

### `RxJava`方式

`RxJava`调用方式同样默认`Computation`线程进行压缩，也可以自己定义任何线程，可在任意线程观察：

    Luban.get(this)                                     
            .load(file)                               
            .putGear(Luban.CUSTOM_GEAR)                 
            .asObservable()                             // 生成Observable
            .subscribe(successAction, errorAction)      // 订阅压缩事件

### 压缩模式

    
#### 1. CUSTOM_GEAR

`AdvancedLuban`增加的个性化压缩，根据限制要求对图片进行压缩，可以限制：图片的宽度、高度以及图片文件的大小
    
        Luban.get(this)
                .load(mFile)
                .setMaxSize(500)                // 限制最终图片大小（单位：Kb）
                .setMaxHeight(1920)             // 限制图片高度
                .setMaxWidth(1080)              // 限制图片宽度
                .putGear(Luban.CUSTOM_GEAR)     // 使用 CUSTOM_GEAR 压缩模式
                .asObservable()

#### 2. THIRD_GEAR 

主要使用`Luban`的算法，提供了类似微信的压缩效果，适用于普通压缩，没有文件大小限制以及图片的宽高限制

#### 3. FIRST_GEAR

`THIRD_GEAR`的简化版本，压缩之后的图片分辨率小于 1280 x 720, 文件最后小于60Kb，特殊情况下，小于原图片的1/5，适用于快速压缩，不计较最终图片品质

## 多图同步压缩

如果你选择的调用方式的是`Listener`方式:

        Luban.get(this)
                .putGear(Luban.CUSTOM_GEAR)             
                .load(fileList)                     // 加载所有图片
                .launch(multiCompressListener);     // 传入一个 OnMultiCompressListener 

`RxJava` 方式：

        Luban.get(this)
                .putGear(Luban.CUSTOM_GEAR)             
                .load(fileList)                     // 加载所有图片
                .asListObservable()                 // 生成Observable<List> 返回压缩成功的所有图片结果

## Issue
    
大家可以根据自己的需求选择不同的压缩模式以及调用方式 ｂ（￣▽￣）ｄ ！最后，欢迎大家提Issue

## Thanks For
- https://github.com/Curzibn/Luban
- https://github.com/ReactiveX/Rxjava
- https://github.com/ReactiveX/RxAndroid

## License

    Copyright 2016 shaohui10086

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
	
 
