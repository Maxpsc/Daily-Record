#彭斯诚的天天小记

##最近

###2017.2.4
####温习数组排序的各种方法
使用JS实现：
```
//冒泡排序
function bubbleSort(arr){
	if(arr.length<=1){return arr;}

	for(var i = arr.length;i>0; i--){
		for(var j = 0;j<i;j++){
			var left = arr[j];
			var right = arr[j+1];
			if(left > right){
				arr[j] = right;
				arr[j+1] = left;
			}
		}
	}
	return arr;
}

//快速排序
function quickSort(arr){
	if(arr.length<=1){return arr;}

	var halfIndex = Math.floor(arr.length / 2);
	var halfNum = arr.splice(halfIndex,1)[0];
	var left = [];
	var right = [];
	arr.forEach(function(num){
		if(num < halfNum){
			left.push(num);
		}else{
			right.push(num);
		}
	});

	return quickSort(left).concat(halfNum,quickSort(right));
}
```
####将url参数解析为key:value型的obj
```
function urlQuery(url){
	url = url==null ? window.location.href : url;
	var queryArr = url.substring(url.lastIndexOf("?")+1).split("&");
	var res = {};
	queryArr.forEach(function(item,index){
		var param = item.indexOf("=");
		var key = decodeURIComponent(item.substring(0,param));
		var val = decodeURIComponent(item.substring(param+1));
		key ? (res[key] = val) : null;
	})
	console.log(res);
	return res;
}
```