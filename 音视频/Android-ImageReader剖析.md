# Android ImageReader剖析

ImageReader可以构造出Surface，并从其中获取出图像数据，可以用于屏幕截屏


使用步骤：

    int width = 720;
    int height = 1280;
    /**
    *  width - 宽度
    *  height - 高度
    *  format - 图像颜色格式  可以是android.graphics.ImageFormat或android.graphics.PixelFormat中的一个格式
    *  maxImages - 最大的访问图像数量
    */
    ImageReader imageReader = ImageReader.newInstance(width, height,
                                    PixelFormat.RGBA_8888, 1);    
                                    
    imageReader.setOnImageAvailableListener(new ImageReader.OnImageAvailableListener() {
                                @Override
                                public void onImageAvailable(ImageReader reader) {
                                       
                                       //获取最新的image
                                        Image image = mImageReader.acquireLatestImage();
                                        if (image != null） {
                                            final Image.Plane[] planes = image.getPlanes();
                                            if (planes.length > 0) {
                                                //分析planes，对于PixelFormat.RGBA_8888格式，planes只有一个
                                                final ByteBuffer buffer = planes[0].getBuffer();
                                                int pixelStride = planes[0].getPixelStride();
                                                int rowStride = planes[0].getRowStride();
                                                int rowPadding = rowStride - pixelStride * width;
                                                
                                                。。。处理数据buffer。。。
                                            }
                                            image.close();
                                        }
                                       
                                }
                            }, 这个参数指定执行Handler，也即onImageAvailable回调所在线程);
                            
                            
    Surface surface = imageReader.getSurface();
    
    获取到surface即可渲染数据，然后onImageAvailable就会回调
    
    
Image.Plane分析

    为了弄清PixelStride、RowStride、RowPadding所代表的含义，现对比下
    android.graphics.ImageFormat和android.graphics.PixelFormat分别传入作为参数时的差异
    
    (传入的width=720，传入的height=1280)   （RowPadding = RowStride - PixelStride * width）
    
            格式                    Plane数量     PixelStride    RowStride      RowPadding     ByteBuffer 
    
     PixelFormat.RGBA_8888             1             4              2880           0           (position = 0, limit = 3686400, capacity = 3686400)
     
     PixelFormat.RGBX_8888          ---------（手机系统不支持，无法测试）-------------
     
     PixelFormat.RGB_888               1             3              2175           15           (position = 0, limit = 2784000, capacity = 2784000)
     
     PixelFormat.RGB_565            ---------（手机系统不支持，无法测试  OpenGLRenderer: Display event receiver pipe was closed or an error occurred.  events=0x9）-------------
     
                                                  
                                              Plane[0]-->1     Plane[0]-->720      0             (position = 0, limit = 921600, capacity = 921600)
     ImageFormat.YUV_420_888           3      Plane[1]-->1     Plane[0]-->368     -352           (position = 0, limit = 235512, capacity = 235512)
                                              Plane[2]-->1     Plane[0]-->368     -352           (position = 0, limit = 235512, capacity = 235512)                             
                                                                                                              
                                                                                                              
                                                                                                                  
    实验过程中，配置的PixelFormat.RGBX_8888格式，会引起三星手机C7100系统崩溃重启，小米手机会提示
    java.lang.UnsupportedOperationException: The producer output buffer format 0x1 doesn't match the ImageReader's configured buffer format 0x2.
    
    
    总结：
    
    PixelStride表示一个像素所占字节数
    RowStride表示一行有多少个字节数
    RowPadding表示一行有多少个无效的字节数 （并不是像素有关的字节，而是为了字节对齐所使用）
    