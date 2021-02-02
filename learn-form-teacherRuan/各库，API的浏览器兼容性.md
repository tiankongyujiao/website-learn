## 各库，API的浏览器兼容性
### 1.fetch的浏览器兼容性
fetch不兼容的浏览器：    
IE全版本 Edge 13及以下（支持到ie14 edge）    
Firefox 38及以下    
Chrome 42及以下    
Safari 10及以下    
Opera 28及以下    

由于Fetch API是基于Promise设计，旧浏览器不支持Promise，需要使用pollyfill es6-promise
