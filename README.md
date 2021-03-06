# ali-cloud-video
#### 阿里云视频点播SDK

### 使用方法
```bash
npm install ali-cloud-video -S
```
```javascript
const AliCloudVideo = require('ali-cloud-video')

const ali = new AliCloudVideo({
  AccessKeyId: '',
  AccessKeySecret: ''
})

const videoId = 'e51aa1941e3b46648f4812dbcf5c175d'

ali.getPlayAuth(videoId, (err, result) => {
  console.log(result)
})
```

#### 方法

- [AliCloudVideo(options)](#alicloudvideo) 构造函数
- [getPlayAuth(videoId, callback)](#getplayauth)
播放视频前获取播放地址和播放凭证
- [getPlayAddress(options, callback)](#getplayaddress)
获取视频播放地址
- [getUploadAuth(options, callback)](#getuploadauth)
上传视频前获取上传凭证和上传地址
- [uploadFile(options, callback)](#uploadfile)
上传视频文件到视频点播服务器
- [deleteFiles(options, callback)](#deletefiles)
删除上传的视频文件
- [getVideoInfo(videoId, callback)](#getvideoinfo)
获取视频信息
- [getVideoList(options, callback)](#getvideolist)
获取视频信息列表，最多支持获取前5000条
- [updateVideoInfo(options, callback)](#updatevideoinfo)
修改视频信息。
- [addCategory(options, callback)](#addcategory)
创建视频分类。
- [getCategories(options, callback)](#getcategories)
获取视频分类及其子分类。
- [updateCategory(options, callback)](#updatecategory)
修改分类
- [deleteCategory(cateId, callback)](#deletecategory)
删除分类
- [getUploadImageAuth(options, callback)](#getuploadimageauth)
上传图片前先获取上传地址和上传凭证
- [refreshUploadAuth(videoId, callback)](#refreshuploadauth)
上传凭证失效后需刷新上传凭证

### AliCloudVideo

`new AliCloudVideo(options)` 构造方法，传入配置对象。

#### options 对象属性

| 名称 | 类型 | 必填项 | 描述 |
| --- | --- | ------ | --- |
| AccessKeyId | String | 是 | 阿里云颁发给用户的访问服务所用的密钥ID。|
| AccessKeySecret | String | 是 | AccessKeySecret |
| Format | String | 否 | 返回值的类型，支持JSON与XML，默认为JSON。 |
| Version | String | 否 | API版本号，为日期形式：YYYY-MM-DD，本版本对应为2017-03-21。 |
| SignatureMethod | String | 否 | 签名方式，目前支持HMAC-SHA1。 |
| SignatureVersion | String | 否 | 签名算法版本，目前版本是1.0。 |

### getPlayAuth

播放视频前获取播放地址和播放凭证

#### 传入参数

| 名称 | 类型 | 必填项 | 描述 |
| --- | --- | ------ | --- |
| videoId | String | 是 | 视频ID |
| callback | Function | 是 | 回调函数 |

#### 返回参数

| 名称 | 类型 | 描述 |
| --- | --- | ---- |
| err | Object | 错误对象 |
| result | Object | 结果 |
 
#### result 对象

| 名称 | 类型 | 描述 |
| --- | --- | ---- |
| RequestId | String | 请求ID |
| VideoMeta | Object | 视频Meta信息 |
| PlayAuth | String | 视频播放凭证 |

#### VideoMeta 对象

| 名称 | 类型 | 描述 |
| --- | --- | ---- |
| CoverURL | String | 视频封面 |
| VideoId | String | 视频ID |
| Duration | Float | 视频时长(秒) |
| Title | String | 视频标题 |
| Status | String | 视频状态，Uploading(上传中)，UploadFail(上传失败)，UploadSucc(上传完成)，Transcoding(转码中)，Checking(审核中)，TranscodeFail(转码失败)，Blocked(屏蔽)，Normal(正常) |

### getPlayAddress

获取视频播放地址。

#### 传入参数

| 名称 | 类型 | 必填项 | 描述 |
| --- | --- | ------ | --- |
| options | Object | 是 | 配置对象 |
| callback | Function | 是 | 回调函数 |

#### options 对象属性

| 名称 | 类型 | 必填项 | 描述 |
| --- | --- | ------ | --- |
| VideoId | String | 是 | 视频上传后的videoId |
| Formats | String | 否 | 视频流格式，多个用逗号分隔，支持格式mp4,m3u8,mp3，默认获取所有格式的流 |
| AuthTimeout | String | 否 | 播放鉴权过期时间，默认为1800秒，支持设置最小值为1800秒 |

#### 返回参数

| 名称 | 类型 | 描述 |
| --- | --- | ---- |
| err | Object | 错误对象 |
| result | Object | 结果 |
 
#### result 对象

| 名称 | 类型 | 描述 |
| --- | --- | ---- |
| RequestId | String | 请求ID |
| VideoBase | Object | 视频基本信息 |
| PlayInfoList | Object | 视频流信息列表 |

#### VideoBase 对象属性

| 名称 | 类型 | 描述 |
| --- | --- | ---- |
| CreationTime | String | 视频创建时间，为UTC时间 |
| CoverURL | String | 视频封面 |
| MediaType | String | 媒体文件类型，取值：video(视频)，audio(纯音频) |
| Status | String | 视频状态，Uploading(上传中)，UploadFail(上传失败)，UploadSucc(上传完成)，Transcoding(转码中)，Checking(审核中)，TranscodeFail(转码失败)，Blocked(屏蔽)，Normal(正常) |
| VideoId | String | 视频ID |
| Duration | String | 视频时长(秒) |
| Title | String | 视频标题 |

#### PlayInfoList 对象属性

| 名称 | 类型 | 描述 |
| --- | --- | ---- |
| PlayInfo | Array | 视频流信息列表 |

#### PlayInfo 数组内对象属性

| 名称 | 类型 | 描述 |
| --- | --- | ---- |
| Format | String | 视频流格式，若媒体文件为视频则取值：mp4, m3u8，若是纯音频则取值：mp3 |
| Duration | String | 视频流长度，单位秒 |
| Height | Number | 视频流高度，单位px |
| Width | Number | 视频流宽度，单位px |
| Size | Number | 视频流大小，单位Byte |
| Encrypt | Number | 视频流是否加密流，取值：0(否)，1(是) |
| PlayURL | String | 视频流的播放地址 |
| Fps | String | 视频流帧率，每秒多少帧 |
| Bitrate | String | 视频流码率，单位Kbps |
| Definition | String | 视频流清晰度定义, 取值：FD(流畅)，LD(标清)，SD(高清)，HD(超清)，OD(原画)，2K(2K)，4K(4K) |

### getUploadAuth

上传视频前获取上传凭证和上传地址

#### 传入参数

| 名称 | 类型 | 必填项 | 描述 |
| --- | --- | ------ | --- |
| options | Object | 是 | 配置对象 |
| callback | Function | 是 | 回调函数 |

#### options 对象

| 名称 | 类型 | 必填 | 描述 |
| --- | --- | --- | --- |
| Title | String | 否 | 视频标题，长度不超过128个字节，UTF8编码。默认生成为new_video_[timestamp] |
| FileName | String | 否 | 视频源文件名，必须带扩展名，且扩展名不区分大小写, 支持的扩展名参见[上传概述](https://help.aliyun.com/document_detail/55396.html?spm=5176.doc55407.2.1.xKk4gJ)的限制部分。默认为[Title].mp4 |
| FileSize | String | 否 | 视频文件大小，单位：字节。 |
| Description | String | 否 | 视频描述，长度不超过1024个字节，UTF8编码 |
| CoverUrl | String | 否 | 自定义视频封面URL地址 |
| CateId | Number | 否 | 视频分类ID，请在“点播控制台-全局设置-分类管理”里编辑或查看分类的ID |
| Tags | String | 否 | 视频标签，单个标签不超过32字节，最多不超过16个标签。多个用逗号分隔，UTF8编码 |

#### 返回参数

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| err | Object | 错误对象 |
| result | Object | 结果 |

#### result 对象

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| RequestId | String | 请求ID |
| VideoId | String | 视频ID |
| UploadAddress | String | 上传地址 |
| UploadAuth | String | 上传凭证 |

> 请注意：
> - 该接口不会真正上传视频文件，您需要拿到上传凭证和地址后使用上传SDK进行文件上传；
> - 如果视频上传凭证失效（有效期3600秒），请调用刷新视频上传凭证接口重新获取上传凭证。

### uploadFile

上传视频文件到视频点播服务器。

#### 传入参数

| 名称 | 类型 | 必填项 | 描述 |
| --- | --- | ------ | --- |
| options | Object | 是 | 配置对象 |
| callback | Function | 是 | 回调函数 |

#### options 对象

| 名称 | 类型 | 必填 | 描述 |
| --- | --- | --- | --- |
| FilePath | String | 是 | 视频文件的路径。 |
| progress | Function | 否 | 进度事件回调函数。参数是上传进度，从0到1。 |
| Title | String | 否 | 视频标题，长度不超过128个字节，UTF8编码。默认生成为new_video_[timestamp] |
| FileName | String | 否 | 视频源文件名，必须带扩展名，且扩展名不区分大小写, 支持的扩展名参见[上传概述](https://help.aliyun.com/document_detail/55396.html?spm=5176.doc55407.2.1.xKk4gJ)的限制部分。默认为[Title].mp4 |
| FileSize | String | 否 | 视频文件大小，单位：字节。 |
| Description | String | 否 | 视频描述，长度不超过1024个字节，UTF8编码 |
| CoverUrl | String | 否 | 自定义视频封面URL地址 |
| CateId | Number | 否 | 视频分类ID，请在“点播控制台-全局设置-分类管理”里编辑或查看分类的ID |
| Tags | String | 否 | 视频标签，单个标签不超过32字节，最多不超过16个标签。多个用逗号分隔，UTF8编码 |

#### 返回参数

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| err | Object | 错误对象 |
| videoId | String | 视频Id |

### deleteFiles

删除上传的视频文件。

#### 传入参数

| 名称 | 类型 | 必填项 | 描述 |
| --- | --- | ------ | --- |
| videos | Array | 是 | 视频id数组 |
| callback | Function | 是 | 回调函数 |

#### 返回参数

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| err | Object | 错误对象 |
| result | Object | 结果对象 |

### getVideoInfo

获取视频信息。

#### 传入参数

| 名称 | 类型 | 必填项 | 描述 |
| --- | --- | ------ | --- |
| videoId | String | 是 | 视频id |
| callback | Function | 是 | 回调函数 |

#### 返回参数

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| err | Object | 错误对象 |
| result | Object | 结果对象 |

#### result 对象

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| RequestId | String | 请求ID |
| Video | String | 视频信息对象 |

#### Video 对象

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| VideoId | String | 视频ID |
| Title | String | 视频标题 |
| Description | String | 视频描述 |
| Duration | Number | 视频时长,单位：秒 |
| CoverURL | String | 视频封面 |
| Status | String | 视频状态 |
| CreationTime | String | 视频创建时间 |
| Size | Number | 视频体积，单位：Byte |
| CateId | Number | 视频分类Id |
| CateName | String | 分类名 |
| Tags | String | 视频标签，逗号分隔 |
| Snapshots | Object | 视频截图，子属性`Snapshot`为数组，内容是图片链接 |
| ModifyTime | String | 视频修改时间 |

### getVideoList

获取视频信息列表。

#### 传入参数

| 名称 | 类型 | 必填项 | 描述 |
| --- | --- | --- | --- |
| options | Object | 是 | 配置对象 |
| callback | Function | 是 | 回调函数 |

#### options 对象属性

| 名称 | 类型 | 必填项 | 描述 |
| --- | --- | --- | --- |
| Status | String | 否 | 视频状态，默认获取所有视频，多个可以用逗号分隔，如：Uploading,Normal，取值包括：Uploading(上传中)，UploadFail(上传失败)，UploadSucc(上传完成)，Transcoding(转码中)，TranscodeFail(转码失败)，Blocked(屏蔽)，Normal(正常) |
| StartTime | String | 否 | CreationTime（创建时间）的开始时间，为开区间(大于开始时间)。日期格式按照ISO8601标准表示，并需要使用UTC时间。格式为：YYYY-MM-DDThh:mm:ssZ 例如，2017-01-11T12:00:00Z（为北京时间2017年1月11日20点0分0秒） |
| EndTime | String | 否 | CreationTime的结束时间，为闭区间(小于等于结束时间)。日期格式按照ISO8601标准表示，并需要使用UTC时间。格式为：YYYY-MM-DDThh:mm:ssZ 例如，2017-01-11T12:00:00Z（为北京时间2017年1月11日20点0分0秒）|
| CateId | String | 否 | 视频分类ID |
| PageNo | Number | 否 | 页号，默认1 |
| PageSize | Number | 否 | 可选，默认10，最大不超过100 |
| SortBy | String | 否 | 结果排序，范围：CreationTime:Desc、CreationTime:Asc，默认为CreationTime:Desc（即按创建时间倒序）|

#### 返回参数

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| err | Object | 错误对象 |
| result | Object | 结果对象 |

#### result 对象

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| RequestId | String | 请求ID |
| VideoList | Object | 视频信息对象，子属性`Video`为数组，数组项为单个视频信息对象 |
| Total | Number | 视频总条数 |

#### Video 对象

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| VideoId | String | 视频ID |
| Title | String | 视频标题 |
| Description | String | 视频描述 |
| Duration | Number | 视频时长,单位：秒 |
| CoverURL | String | 视频封面 |
| Status | String | 视频状态 |
| CreationTime | String | 视频创建时间 |
| Size | Number | 视频体积，单位：Byte |
| CateId | Number | 视频分类Id |
| CateName | String | 分类名 |
| Tags | String | 视频标签，逗号分隔 |
| Snapshots | Object | 视频截图，子属性`Snapshot`为数组，内容是图片链接 |
| ModifyTime | String | 视频修改时间 |

### updateVideoInfo

修改视频信息。

> 注意：传入参数则更新相应字段，否则该字段不会被覆盖或更新。

#### 传入参数

| 名称 | 类型 | 必填项 | 描述 |
| --- | --- | --- | --- |
| options | Object | 是 | 配置对象 |
| callback | Function | 是 | 回调函数 |

#### options 对象属性

| 名称 | 类型 | 必填项 | 描述 |
| --- | --- | --- | --- |
| VideoId | String | 是 | 视频Id |
| Title | String | 否 | 视频标题，长度不超过128个字节，UTF8编码 |
| Description | String | 否 | 视频描述，长度不超过1024个字节，UTF8编码 |
| CoverURL | String | 否 | 视频封面URL地址 |
| CateId | String | 否 | 视频分类ID |
| Tags | String | 否 | 视频标签，单个标签不超过32字节，最多不超过16个标签。多个用逗号分隔，UTF8编码 |

#### 返回参数

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| err | Object | 错误对象 |
| result | Object | 结果对象 |

#### result 对象

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| RequestId | String | 请求ID |

### addCategory

创建视频分类。最大支持三级分类，每个分类最多支持创建100个子分类。

#### 传入参数

| 名称 | 类型 | 必填项 | 描述 |
| --- | --- | --- | --- |
| options | Object | 是 | 配置对象 |
| callback | Function | 是 | 回调函数 |

#### options 对象属性

| 名称 | 类型 | 必填项 | 描述 |
| --- | --- | --- | --- |
| CateName | String | 是 | 分类名称 |
| ParentId | String | 否 | 父分类ID，若不填，则默认生成一级分类，根节点分类ID为-1 |

#### 返回参数

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| err | Object | 错误对象 |
| result | Object | 结果对象 |

#### result 对象

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| RequestId | String | 请求ID |
| Category | Object | 视频分类信息 |

#### Category 对象

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| CateId | Number | 分类ID |
| ParentId | Number | 父分类Id |
| CateName | String | 分类名称 |
| Level | Number | 分类层级 |

### getCategories

获取视频分类及其子分类。

#### 传入参数

| 名称 | 类型 | 必填项 | 描述 |
| --- | --- | --- | --- |
| options | Object | 是 | 配置对象 |
| callback | Function | 是 | 回调函数 |

#### options 对象属性

| 名称 | 类型 | 必填项 | 描述 |
| --- | --- | --- | --- |
| CateId | String | 否 | 分类ID，默认为根节点分类ID即-1 |
| PageNo | String | 否 | 子分类列表页号，默认1 |
| PageSize | String | 否 | 子分类列表页长，默认10，最大不超过100 |

#### 返回参数

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| err | Object | 错误对象 |
| result | Object | 结果对象 |

#### result 对象

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| RequestId | String | 请求ID |
| Category | Object | 视频分类信息 |
| SubTotal | Number | 子分类总数 |
| SubCategories | Object | 子分类列表 |

### updateCategory

修改分类。

#### 传入参数

| 名称 | 类型 | 必填项 | 描述 |
| --- | --- | --- | --- |
| options | Object | 是 | 配置对象 |
| callback | Function | 是 | 回调函数 |

#### options 对象属性

| 名称 | 类型 | 必填项 | 描述 |
| --- | --- | --- | --- |
| CateId | String | 是 | 分类ID |
| CateName | String | 是 | 分类名称，不能超过64个字节，UTF8编码 |

#### 返回参数

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| err | Object | 错误对象 |
| result | Object | 结果对象 |

#### result 对象

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| RequestId | String | 请求ID |

### deleteCategory

删除分类。

#### 传入参数

| 名称 | 类型 | 必填项 | 描述 |
| --- | --- | --- | --- |
| cateId | String | 是 | 分类ID |
| callback | Function | 是 | 回调函数 |

#### 返回参数

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| err | Object | 错误对象 |
| result | Object | 结果对象 |

#### result 对象

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| RequestId | String | 请求ID |

### getUploadImageAuth

上传图片前先获取上传地址和上传凭证

#### 传入参数

| 名称 | 类型 | 必填 | 描述 |
| --- | --- | ------ | --- |
| options | Object | 是 | 配置对象 |
| callback | Function | 是 | 回调函数 |

#### options 对象

| 名称 | 类型 | 必填 | 描述 |
| --- | --- | --- | --- |
| ImageType | String | 否 | 图片类型，可选值 cover：封面，watermark：水印。默认cover。 |
| ImageExt | String | 否 | 图片文件扩展名，可选值 png，jpg，jpeg，默认 png |

#### 返回参数

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| err | Object | 错误对象 |
| result | Object | 结果 |

#### result 对象

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| RequestId | String | 请求ID |
| ImageURL| String | 图片地址 |
| UploadAddress | String | 上传地址 |
| UploadAuth | String | 上传凭证 |

> 请注意：
> - 该接口不会真正上传图片文件，您需要拿到上传凭证和地址后使用上传SDK进行文件上传（和视频上传相同）；
> - 如果图片上传凭证失效（有效期900秒），请重新调用此接口获取上传地址和凭证；
> - 如果发现返回的ImageURL在浏览器无法访问（403），那是因为您开启了点播域名的鉴权功能，可工单联系我们关闭或自助生成鉴权签名。

### refreshUploadAuth

上传凭证失效后需刷新上传凭证

###### 传入参数

| 名称 | 类型 | 必填项 | 描述 |
| --- | --- | ------ | --- |
| videoId | String | 是 | 视频ID |
| callback | Function | 是 | 回调函数 |

##### 返回参数

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| err | Object | 错误对象 |
| result | Object | 结果 |

##### result 对象

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| RequestId | String | 请求ID |
| UploadAuth | String | 上传凭证 |
