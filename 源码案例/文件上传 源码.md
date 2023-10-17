# 文件上传

## 静态资源访问

``````properties
#配置请求过滤字符串
#这里表示只有静态资源的访问路径为/**时，才会处理请求。
spring.mvc.static-path-pattern=/**

#配置静态资源的路径
#这里表示springboot将在classpth:/static查找静态资源
spring.web.resources.static-locations=classpth:/static
``````



```java
@RestController
public class UploadController {


    @PostMapping("/upload")
    public String saveFiles(String nickname ,MultipartFile file,HttpServletRequest request) throws IOException {

        System.out.println(nickname);
        System.out.println(file.getOriginalFilename());
        System.out.println(file.getContentType());

        
        String path=request.getServletContext().getRealPath("/upload");
        System.out.println(path);
        save(file,path);


        return "上传成功";
    }


    public void  save(MultipartFile file, String path) throws IOException {


        File dir=new File(path);

        if (!dir.exists()){


            dir.mkdir();
        }


        File test=new File(path + File.separator + "test.jpg");
        file.transferTo(test);

    }

}
```

