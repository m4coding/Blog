# Android Compose使用相关

## 注意点

1、padding相关

    modifier引入顺序不同，会有不同显示效果
    (1)   Box(modifier = Modifier
            .background(Color.Blue)
            .padding(10.dp)) {
            
        }

    (2)    Box(modifier = Modifier
            .padding(10.dp)
            .background(Color.Blue)) {

        }

    （1）会出现设置的背景色不包括padding，（2）设置的背景色会在padding中出现
    设置的点击出现点击效果同理