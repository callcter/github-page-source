---
title: 使用CameraRoll（iOS）
tags:
  - react-native
url: 134.html
id: 134
categories:
  - 移动开发
date: 2016-07-20 17:17:37
---

import React, { Component } from 'react';
import {
  StyleSheet,
  Text,
  View,
  TouchableOpacity,
  CameraRoll,
  Image,
  Dimensions,
} from 'react-native';
var screenWidth = Dimensions.get('window').width;
var image_url = '';
export default class ReleaseTab extends Component{
  constructor(props) {
    super(props);
    this.state = {
      pageIndex: 0,
      images: \[\],
      fetchParams: {
        first: 5
      }
    };
    this.goPage0 = this.goPage0.bind(this);
    this.goPage1 = this.goPage1.bind(this);
  }
  goPage0(uri) {
    this.setState({
      pageIndex: 0,
    });
    image_url = uri;
  }
  goPage1() {
    this.setState({
      pageIndex: 1
    });
    this.listImages();
  }
  //使用CameraRoll显示本地图片
  listImages() {
    CameraRoll.getPhotos(this.state.fetchParams)
      .then((data)=>{
        let assets = data.edges;
	let images = assets.map((asset) => asset.node.image);
	this.setState({
	  images: images,
	});
      },(e)=>{
	console.log(err);
      });
  }
  render() {
    switch(this.state.pageIndex){
      case 0:
	return (
	  <View style={styles.container}>
	    {image_url ? <Text></Text> :
              <TouchableOpacity style={styles.btn__save} onPress={this.goPage1}>
                <Text style={styles.btn\_\_save\_text}>选择图片</Text>
              </TouchableOpacity>
            }
            {image_url ?
              <View>
                <Image source={ {uri:image_url}} style={styles.image} />
              </View> :
              <Text style={styles.tip__text}>尚未选择图片</Text>
             }
           </View>
         );
         break;
       case 1:
         return (
           <View style={styles.container}>
             <View style={styles.imageGrid}>
	       {this.state.images.map((image)=>
	<TouchableOpacity key={image.uri} onPress={this.goPage0.bind(null,image.uri)}>
          <Image key={image.uri} style={styles.image} source={ {uri:image.uri}}/>
	</TouchableOpacity>)}
      </View>
    </View>
  );
  break;
}
}
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    marginTop: 20,
    justifyContent: 'flex-start',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  imageGrid: {
  	flex: 1,
  	flexDirection: 'row',
  	flexWrap: 'wrap',
  	justifyContent: 'center'
  },
  image: {
  	width: 100,
  	height: 100,
  	margin: 10
  },
  btn__save: {
  	marginTop: 10,
  	width: screenWidth-10,
  	backgroundColor: '#fc3',
  	borderRadius: 5,
  	overflow: 'hidden'
  },
  btn\_\_save\_text: {
  	color: "#fff",
  	fontSize: 16,
  	paddingTop: 10,
  	paddingBottom: 10,
  	textAlign: 'center'
  },
  tip__text: {
  	marginTop: 10,
  	color: '#999',
  	fontSize: 14
  }
});

遇到报错：Cannot read property 'getPhotos' of undefined 找寻原因，发现这是一个原生模块，需要自己添加 将node\_modules/react-native/Libraries/CameraRoll下的RCTCameraRoll.xcodeproj文件加入项目的Libraries目录下，然后添加静态库依赖 添加方法： Build Phases -> Link Binary With Libraries （对iOS开发来说很简单，但是我不知道呀 T\_T…） 然后就可以正常使用 CameraRoll 组件了 issue地址：[https://github.com/facebook/react-native/issues/3950](https://github.com/facebook/react-native/issues/3950)