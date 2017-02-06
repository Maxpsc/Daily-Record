#天天小记
最近10天内容
##2017.2.4
###温习数组排序的各种方法
以下以升序为例，使用JS实现：<br>
**冒泡排序**：通过两两比较，左边若大于右边数字，则左右交换，最小的数字可通过不断交换，“冒泡”到最右边
```
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
```
**快速排序**：取数组中间数字，遍历其他数字，若小于则放在左边数组，大于或等于则放在右边数组，以此递归，将小数组依次拼接成排好序的数组
```
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
**插入排序**：将没排好的数字依次与排好数组中的数字比较，若小于则插到其前面，否则继续比较直到放到最后
```
function insertSort(arr){
	if(arr.length<=1){return arr;}

	var res = [arr[0]];
	for(var i=1;i<arr.length;i++){
		var cNum = arr[i];
		for(var j=0;j<res.length;j++){
			var num = res[j];
			if(cNum<num){
				res.splice(j,0,cNum);
				break;
			}
			if(j==res.length-1){
				res.push(cNum);
				break;
			}
		}
	}
	return res;
}
```
**选择排序**：每一次将待排序中最小数取出，放到已排序的最后一个
```
function selectSort(arr){
	if(arr.length<=1){return arr;}
	for(var i=0;i<arr.length;i++) {
		var min = arr[i];
		for (var j=i+1;j<arr.length;j++) {
			if(arr[j]<min) {
				var temp = min;
				min = arr[j];
				arr[j] = temp;
			}
		}
		arr[i] = min;
	}
	return arr;
}
```
Js自带: Array.sort()
```
function sortNumber(a,b){
	return a-b;
}
console.log([1,10,2,40,3].sort(sortNumber));//[1,2,3,10,40]
```
###将url参数解析为key:value型的obj
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

##2017.2.6
###手写Function.bind函数
bind函数核心：改变函数上下文this,<br>
关注点：保持this，arguments，返回值<br>

MDN的标准polyfill
```
if (!Function.prototype.bind) {
  Function.prototype.bind = function (oThis) {
    if (typeof this !== "function") {
      // closest thing possible to the ECMAScript 5
      // internal IsCallable function
      throw new TypeError("Function.prototype.bind - what is trying to be bound is not callable");
    }

    var aArgs = Array.prototype.slice.call(arguments, 1), 
        fToBind = this, 
        fNOP = function () {},
        fBound = function () {
          return fToBind.apply(this instanceof fNOP
                                 ? this
                                 : oThis || this,
                               aArgs.concat(Array.prototype.slice.call(arguments)));
        };

    fNOP.prototype = this.prototype;
    fBound.prototype = new fNOP();

    return fBound;
  };
}
```
