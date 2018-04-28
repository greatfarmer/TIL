# Spring MVC Annotation (Cont.)

## TEST_6 File Upload

### web.xml
> [Spring MVC Annotation](https://github.com/greatfarmer/TIL/blob/master/Spring/Spring-MVC-annotation.md) 과 동일

### dispatcher-servlet.xml
```xml
<!-- /WEB-INF/dispatcher-servlet.xml -->
...
<!-- TEST_6
import org.springframework.web.multipart.commons.CommonsMultipartResolver;

CommonsMultipartFile을 사용하기 위해서는
반드시 CommonsMultipartResolver 빈 객체가 IOC 컨테이너 안에 있어야 한다

CommonsMultipartResolver : Upload 파일 정보를 관리 (파일 크기 ....)

의존객체: org.apache.commons.fileupload
파일 작업: com.springsource.org.apache.commons.io
-->
    <bean class="com.controller.ImageController"></bean>

    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    	<property name="maxUploadSize" value="10485760"></property>
    </bean>
...
```

### Photo.java (DTO)
```java
// /src/com/model/Photo.java
package com.model;

import org.springframework.web.multipart.commons.CommonsMultipartFile;

public class Photo {
	private String name;
	private int age;
	private String image; //업로드한 파일명 (a.jpg , b.png ...)
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public String getImage() {
		return image;
	}
	public void setImage(String image) {
		this.image = image;
	}

	//POINT (파일 정보를 가지는 객체)
	private CommonsMultipartFile file;
	public CommonsMultipartFile getFile() {
		return file;
	}
	public void setFile(CommonsMultipartFile file) {
		this.file = file;
	}
}
```

### ImageController.java
```java
// /src/com/controller/ImageController.java
package com.controller;

import java.io.FileOutputStream;
import java.io.IOException;

import javax.servlet.http.HttpServletRequest;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.multipart.commons.CommonsMultipartFile;

import com.model.Photo;

@Controller
@RequestMapping("/image/upload.do")
public class ImageController {

	@RequestMapping(method=RequestMethod.GET)
	public String form() {
		System.out.println("image.jsp 출력");
		return "image/image";
	}

	@RequestMapping(method=RequestMethod.POST)
	public String submit(Photo photo , HttpServletRequest request) throws IOException {
		CommonsMultipartFile imagefile = photo.getFile();

		String filename = imagefile.getOriginalFilename();

        // WebContent/upload 폴더가 생성되어 있어야함
		String path = request.getServletContext().getRealPath("/upload");

		String fpath = path + "\\" + filename;

		FileOutputStream fs = new FileOutputStream(fpath);
		fs.write(imagefile.getBytes());
		fs.close();

		return "image/image";
	}
}
```

### image.jsp
```
중요사항: form태그 내 enctype="multipart/form-data" 작성
```
```html
<!-- /WEB-INF/views/image/image.jsp -->
...
<body>
	<form method="post" enctype="multipart/form-data">
		이름:<input type="text" name="name"><br>
		나이:<input type="text" name="age"><br>
		사진:<input type="file" name="file"><br>
		<input type="submit" value="파일 업로드">
	</form>
</body>
...
```
