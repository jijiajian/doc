1. 导入相关jar包(springfox-swagger2,swagger-annotations,springfox-swagger-ui)  
```
        
        <!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
		<dependency>
			<groupId>io.springfox</groupId>
			<artifactId>springfox-swagger2</artifactId>
			<version>2.6.1</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/io.swagger/swagger-annotations -->
		<dependency>
			<groupId>io.swagger</groupId>
			<artifactId>swagger-annotations</artifactId>
			<version>1.5.12</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
		<dependency>
			<groupId>io.springfox</groupId>
			<artifactId>springfox-swagger-ui</artifactId>
			<version>2.6.1</version>
		</dependency>

```

2. 在启动类中添加注解(@EnableSwagger2)开启swagger

3. 启动类中添加方法配置swagger  

```
/**
 * 启动类中对swagger的配置方法,该方法与main方法平行
 * 此处主要对Controller实现类所在的包进行扫描
 */

@Bean
public Docket config() {
    return new Docket(DocumentationType.SWAGGER_2)
            .useDefaultResponseMessages(false)
            .select()
            .apis(RequestHandlerSelectors.basePackage("com.jezoe.yuntask.admin.security"))
            .build();
}
```

4. 编写Controller的接口  

`注意:在Controller实现类中各个方法的@RequestMapping一定要指明method`

```
/**
 * Controller接口示例
 */
@Api(description = "userController的描述")
public interface ApidemoInterface {
    @ApiOperation(value="对应的url,和Controller的@RequestMapping对应", notes="方法描述")
    @ApiImplicitParams({
        //
        /**
         * name  需要与参数名对应
         * value 为参数描述
         * paramType：参数放在哪个地方header,query,path,body（不常用）,form（不常用）
         * required: 是否必填
         * dataType: 数据类型
         */
            @ApiImplicitParam(name = "id", value = "用户名",paramType="query" ,required = true, dataType = "int"),
    })
    User getUser(Integer id);
}

```
