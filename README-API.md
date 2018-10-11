# 儿童线api相关

## 接口返回的data对象格式
```
res_code: 0, // 0表示成功，1表示错误，并在res_msg中返回错误信息
res_msg: "success",
data: { //相关数据}
```

## 登录相关接口 

### 1. user_info？user_login?, @params //iv ？signature ？
```
{
    token: '' //或者其他识别信息
}
```

## 页面相关接口

### 1.景点导览list的数据
```
[
    {
        spot_id: '',
        spot_name: '',
        spot_image: '',
        longitude: '', // 经度
        latitude: '',  // 纬度
    }
]
```

### 2.列表导览的景点详情数据，@params:spot_id
```
{   
    spot_id: '',
    image_url: '',   // 文章详情为一张图片，该图片的路径
    audio_url: '',   // 音频地址
    video_url: '',   // 视频地址
    share_title: ''  // 分享标题
    share_image: ''  // 分享略缩图地址
}
```

### 3.地图导览的景点详情页底部列表
```
[
    {   
        spot_id: '',
        spot_image: '',
        spot_image_active: '',
    }
]
```

### 4.地图导览的景点详情页图片，@params:spot_id，longitude，latitude 可选传参id或者经纬度
```
{   
    spot_id: '',
    image_url: ''
}
```

### 5.拍照识别 @params: image_url
```
{
    webview_url: ''
}
```

### 6.问答列表
```
[
    {   
        quiz_id: '',
        quiz_title: '',
        quiz_image: '',
        passed: 0, //0为未通过，1为已通过
    }
]
```

### 7.问答详情 @params: quiz_id
```
{
    title: '',
    hint: '',
    image_list: [
        {
            url: '',
            correct: 1 //1为正确答案，0为错误
        }
    ]
}
```

### 8. 问答结果 @params: quiz_id, token
```
{
    reward: '获得勋章',
    reward_image: ''
}
```

### 9. 个人成就
```
{
    stage_number: '',
    reward_number: '', //勋章数量
    stage_status: []   //闯关记录
}
```

### 10. 我的消息
```
[
    {
        id: '',
        title: '',
        desc: '',
        image: ''
        time: ''
    }
]
```

### 11. 意见反馈
```
{
    msg: 'success'
}
```

