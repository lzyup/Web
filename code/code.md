### 分割数组
```
arrayChunk(arr = [],size = 2){
    let groups = [];
    if(arr && arr.length > 0){
        groups = arr.map((e,i)=>{
            return i % size === 0 ? arr.slice(i,i+size):null;
        }).filter(e=>{
            return e;
        })
    }
    return groups;
}
```


### 数组去重
```
var array = [1,2,1,1,1,'1'];
function unnique(array){
    var res = array.filter(function(item,index,array){
        return array.indexOf(item) === index;
    })
    return res;
}
```