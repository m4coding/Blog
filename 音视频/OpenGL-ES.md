# OpenGL ES

OpenGL ES 即为OpenGl for Embedded System，是一个OpenGL的子集，专用于嵌入式设备
    
### OpenGL ES基本概念

    在OpenGL ES的世界里，实际上只能绘制点、直线以及三角形，若想绘制其他图像只能靠这三个基本元素组成
    例如想要绘制一个正方形，那么可以通过两个三角形组合绘制而成

    OpenGL以管道的方式传送数据并绘制在屏幕上显示。顺序如下；
    
    读取顶点数据 --> 执行顶点着色器，确定最终位置 
        --》 组装图元 --》 光栅化图元 --》 执行片段着色器 
            --》 写入缓冲区 （FrameBuffer） --》 显示在屏幕上

    1、顶点着色器：Vertex Shader
    
        生成每个顶点的最终位置  （针对于每个顶点，执行一次）
    
    2、片段着色器：Fragment Shader
        
        对点、直线、或三角形每个片段生成最终颜色   （针对于每个片段，执行一次）
        
    代码使用流程：
    
    1、加载顶点数据  
    
        ByteBuffer.allocateDirect。。
        
    2、从res目录raw中加载Shader代码  （android平台）
        String vertexShaderStr = readShaderCodeFromResource(mActivity, R.raw.vertex_shader); //顶点着色器代码
        String fragmentShaderStr = readShaderCodeFromResource(mActivity, R.raw.fragment_shader); //片段着色器代码
        
    3、创建Shader，并验证
     
        //当返回的id为0时，表示发生了异常
        final int vertexShaderId = glCreateShader(GL_VERTEX_SHADER); //创建Vertex Shader，保存id以备后用
        final int fragmentShaderId = glCreateShader(GL_FRAGMENT_SHADER); //创建Fragment Shader，保存id以备后用
        
        //将要加载的shader代码和GL中所引用的着色器对象关联起来
        glShaderSource(vertexShaderId, vertexShaderStr);
        glShaderSource(fragmentShaderId, fragmentShaderStr);
        
        //编译所关联的着色器代码
        glCompileShader(vertexShaderId);
        glCompileShader(fragmentShaderId);
        
        //获取编译状态
        final int[] compileStatus = new int[1];
        glGetShaderiv(fragmentShaderId, GL_COMPILE_STATUS,
                compileStatus, 0);

    4、创建Programe，并链接着色器对象
    
        // 创建Program
        final int programId = glCreateProgram();
        //关联Shader
        glAttachShader(programId, vertexShaderId);
        glAttachShader(programId, fragmentShaderId);
        //最后链接Shader
        glLinkProgram(programId);\
        
        //验证program
        glValidateProgram(programId);
        final int[] validateStatus = new int[1];
        glGetProgramiv(programId, GL_VALIDATE_STATUS, validateStatus, 0); //validateStatus[0] != 0为true表示创建的Program是正确的
        Log.v(TAG, "result of validate program is " + validateStatus[0] + " log:" + glGetProgramInfoLog(programId));
        
        // 启用这个Program
        glUseProgram(programId);
    
    5、找到shader中的变量，并赋值
    
         // 找到需要赋值的变量  shader代码中定义的变量
        int uMatrixLocation = glGetUniformLocation(programId, "u_Matrix");
        int aPositionLocation = glGetAttribLocation(programId, "a_Position");
        int uColorLocation = glGetUniformLocation(programId, "u_Color");
        
        //赋值，填充数据
        glUniform4f(uColorLocation, 1, 0, 0, 1); //红色  绿色  蓝色  透明度
        mVertexBuffer.position(0);
        glVertexAttribPointer(aPositionLocation, 2, GL_FLOAT, false, 0, mVertexBuffer);
        glEnableVertexAttribArray(aPositionLocation); //使能
        
    6、绘制到屏幕
    
        //第一个参数表示的是要绘制的形状，第二个参数表示的是从顶点数组哪个位置开始读取，第三个参数表示的是读入多少个顶点
        glDrawArrays(GL_TRIANGLE_FAN, 0, 6);
        
### OpenGL ES坐标认知
    
    在OpenGL的坐标里，范围为[-1,1]
                
    [-1,1]------------[1,1]
       |                 |
       |                 |
 -------------[0,0]------------- 
       |                 |
       |                 |
    [-1,-1]-----------[1,-1]
    
    中间为原点
    
    
    
### GLSL  -- OpenGL Shader Language


    类似于C语言
    
    如下为一个Fragment Shader代码
    
    precision mediump float; //指定精度
    
    uniform vec4 u_Color;  //颜色矢量  存储着红色、绿色、蓝色、透明度
    
    void main() //执行入口
    {
        gl_FragColor = u_Color; //输出
    }