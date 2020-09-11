# 开发Swagger api转bean的as插件

## 纪要

1、一般查看swagger api需要登录，而登录可能会是基本的Authorization Basic认证

    使用http api时，可以在http header中添加Authorization: Basic 用户名:密码的base64编码
    
    
    
## 创建自己的插件仓库

1、尝试使用gradle-intellij-plugin的publishPlugin，有问题，需要自定义需要支持对应的api操作才行

    具体实现的api如下：
    interface PluginRepositoryService {
      @Multipart
      @Headers("Accept: text/plain")
      @POST("/plugin/uploadPlugin")
      fun upload(
        @Part("pluginId") pluginId: Int,
        @Part("channel") channel: RequestBody?,
        @Part("notes") notes: RequestBody?,
        @Part file: MultipartBody.Part
      ): Call<ResponseBody>
    
      @Multipart
      @Headers("Accept: text/plain")
      @POST("/plugin/uploadPlugin")
      fun uploadByXmlId(
        @Part("xmlId") pluginXmlId: RequestBody,
        @Part("channel") channel: RequestBody?,
        @Part("notes") notes: RequestBody?,
        @Part file: MultipartBody.Part
      ): Call<ResponseBody>
    
      @Multipart
      @POST("/api/plugins/{family}/upload")
      fun uploadNewPlugin(
        @Part file: MultipartBody.Part,
        @Path("family") family: String,
        @Part("licenseUrl") licenseUrl: RequestBody,
        @Part("cid") category: Int
      ): Call<PluginBean>
    
      @Streaming
      @GET("/plugin/download")
      fun download(
        @Query("pluginId") pluginId: String,
        @Query("version") version: String,
        @Query("channel") channel: String?
      ): Call<ResponseBody>
    
      @Streaming
      @GET("/plugin/download")
      fun download(@Query("updateId") updateId: Int): Call<ResponseBody>
    
      @Streaming
      @GET("/pluginManager?action=download")
      fun downloadCompatiblePlugin(
        @Query("id") pluginId: String,
        @Query("build") ideBuild: String,
        @Query("channel") channel: String?
      ): Call<ResponseBody>
    
      @GET("/plugins/list/")
      fun listPlugins(
        @Query("build") ideBuild: String,
        @Query("channel") channel: String?,
        @Query("pluginId") pluginId: String?
      ): Call<XmlPluginRepositoryBean>
    
      @GET("/api/plugins/{family}/{pluginXmlId}")
      fun getPluginByXmlId(@Path("family") family: String, @Path("pluginXmlId") pluginXmlId: String): Call<PluginBean>
    
      @GET("/api/plugins/{id}")
      fun getPluginById(@Path("id") id: Int): Call<PluginBean>
    
      @GET("/api/plugins/{id}/developers")
      fun getPluginDevelopers(@Path("id") id: Int): Call<List<PluginUserBean>>
    
      @GET("/api/plugins/{id}/channels")
      fun getPluginChannels(@Path("id") id: Int): Call<List<String>>
    
      @GET("/api/plugins/{id}/compatible-products")
      fun getPluginCompatibleProducts(@Path("id") id: Int): Call<List<ProductEnum>>
    
      @GET("/api/plugins")
      fun getPluginXmlIdByDependency(
        @Query("dependency") dependency: String,
        @Query("includeOptional") includeOptional: Boolean
      ): Call<List<String>>
    
      @GET("/api/search")
      fun searchPluginsXmlIds(
        @Query("build") build: String,
        @Query("max") max: Int,
        @Query("offset") offset: Int,
        @Query("search") query: String
      ): Call<List<String>>
    
      @GET("/api/search/updates")
      fun searchUpdates(
        @Query("build") build: String,
        @Query("pluginXMLId") xmlId: PluginXmlId
      ): Call<List<UpdateBean>>
    
      @GET("/files/pluginsXMLIds.json")
      fun getPluginsXmlIds(): Call<List<String>>
    
      @POST("/api/search/compatibleUpdates")
      @Headers("Content-Type: application/json")
      fun searchLastCompatibleUpdate(@Body body: CompatibleUpdateRequest): Call<List<UpdateBean>>
    
      @GET("/api/plugins/{id}/updates")
      fun getUpdatesByVersionAndFamily(
        @Path("id") xmlId: String,
        @Query("version") version: String,
        @Query("family") family: String
      ): Call<List<PluginUpdateBean>>
    
      @GET("/api/updates/{id}")
      fun getUpdateById(@Path("id") id: Int): Call<PluginUpdateBean>
    
      @GET("/api/plugins/{id}/updateVersions")
      fun getPluginVersions(@Path("id") id: Int): Call<List<PluginUpdateVersion>>
    
      @GET("/files/{pluginId}/{updateId}/meta.json")
      fun getIntelliJUpdateMeta(@Path("pluginId") pluginId: Int, @Path("updateId") updateId: Int): Call<IntellijUpdateMetadata>
    
    }
    
2、尝试手动添加插件，添加updatePlugins.xml，指定所在的插件路径

    插件只能保存在不能认证的http web服务器上，否则到最后下载到插件包后，就提示filename不正确校验安装失败。。
    
    如我的updatePlugins.xml配置如下：
    <?xml version="1.0" encoding="UTF-8"?>
    <plugins>
      <plugin id="com.test.myplugin" url="http://127.0.0.1:8080/MyPlugin-1.0.0.zip?token=xxx" version="1.0.0">
      </plugin>
    </plugins>
    
    这个时候，可以下载到MyPlugin-1.0.0.zip插件包，但是安装时就会提示和服务的插件名称不匹配,
    修改url="http://127.0.0.1:8080/MyPlugin-1.0.0.zip?"就正确了。。这样说明不能使用有认证信息的url。。。。。
    
如何在as添加自定义插件仓库索引，可以参考[Custom plugin repositories](https://www.jetbrains.com/help/idea/managing-plugins.html#repos)   
