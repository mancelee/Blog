Java json解析的第三方实现有比较多的选择，但是不同的实现使用方式略有不同，记录一下自己使用过程遇到的问题。

### 解析json中的嵌套json对象

- net.sf.json.JSONObject
json.getJSONObject("test") 会报错

- com.alibaba.fastjson.JSON 可以正确解析
object.getJSONObject("test")