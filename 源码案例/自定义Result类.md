# 一、要素

1. **success**

   > 请求状态flag

2. **code**

   > 自定义响应码

3. **message**

   > 响应消息

4. **data**

   > 响应数据，用 **hashmap** 实现



# 二、例子

```java
public class Result {

    // 请求状态
    private  Boolean success;
    // 自定义响应码
    private Integer code;
    // 响应消息（解释响应码）
    private String message;
    // 响应数据
    private Map<String, Object> data=new HashMap<>();

    public Boolean getSuccess() {
        return success;
    }

    public void setSuccess(Boolean success) {
        this.success = success;
    }

    public Integer getCode() {
        return code;
    }

    public void setCode(Integer code) {
        this.code = code;
    }

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }

    public Map<String, Object> getData() {
        return data;
    }

    public void setData(Map<String, Object> data) {
        this.data = data;
    }
    
    private Result(){}

    // 生成成功响应
    public static Result ok(){

        Result r =new Result();
        r.setSuccess(true);
        r.setCode(2000);
        r.setMessage("请求成功");
        return r;

    }

    // 生成错误响应
    public static Result error(){
        Result r =new Result();
        r.setSuccess(false);
        r.setCode(1000);
        r.setMessage("请求失败");
        return r;
    }

    // 存数据
    public Result data(String key,Object value){
        this.data.put(key,value);
        return this;
    }
}
```

