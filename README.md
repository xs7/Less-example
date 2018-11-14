# Less实践
> from IMOOC案例
## Tips
- 标注工具PxCook，对设计稿中元素的大小、颜色等进行标注
- 使用Emmet插件快捷的写html结构（VSCode默认已启用）
- BEM命名规范

## VScode中Less自动编译
在.vscode文件夹下settings.json文件中设置

```
{    
    "less.compile": {
        "out": "${workspaceRoot}\\css\\"
    }
}
```
即将less文件自动编译为同名css文件，放在根目录的CSS文件目录下，目录结构如下
- Project
 - .vscode
    - settings.json
 - css
    - index.css(编译文件)
 - less
    - index.less

每次保存修改后的Less文件都会自动编译为CSS

## 实现
### 布局相关样式
布局相关样式 定义了大致的页面布局，页面中元素分布的高度、颜色等（header\banner.....\ft-size\color）
- #bg-color(@name) 背景色定义
- #font-size(@name) 字体大小定义
- #color(@name) 字体颜色定义
- #layout(@name) 布局样式编写（header banner）

```Less
//将整体的结构定义放在一起
#layout(@name) {
    & when(@name=header) {
        height: 60px;
        #bg-color(black)
    }
    & when(@name=banner) {
        height: 560px;
        background-color: #6a6593;
    }
}

//在具体结构样式中调用定义好的#layout()
.header {
    #layout(header);
}
.banner{
    #layout(banner)
}
```

### 功能样式(radius\square...)
- #radius(@radius:50%) 圆角
- #center-h(@width) 水平horizontal对齐
- #bg-img(@src) 背景图片
- #square(@w:10px) 设置相同宽高正方形
- #line-height(@h) 高度和行高

```Less
#radius(@radius: 50%) {
    border-radius: @radius;
}
#center-h(@width) {
    left: 50%;
    margin-left: @width*-.5;
}
#bg-img(@src) {
    background-image: url("../imgs/@{src}");
}
#square(@w: 10px) {
    width: @w;
    height: @w;
}
#line-height(@h) {
    height: @h;
    line-height: @h;
}
```

### 通用样式(.fl\.fr\.clearfix)
- .fl() 左浮
- .fr() 右浮
- .clearfix()清除浮动

```Less
.clearfix(){
    &:after{
        content:'';
        display: block;
        font-size: 0;
        line-height: 0px;
        clear: both;
        zoom: 1;
    }
}
```

- cover-background()背景图设置为覆盖模式

### 模块具体样式（.header>.nav>.nav__item）
定义重复的小模块的具体的样式，比如课程卡片列表中每个卡片的宽高、阴影、装饰
- #course-box(@name)课程模块样式定义

```Less
#course-box(@name){
    & when(@name=recommend){
        ......
    }
    & when(@name=stardard){
        ......
    }
}
```
- #decorate-bottom() 底部装饰样式

### 根据模块结构写具体CSS细节
小模块中的具体细节，比如课程卡片中子元素的具体样式
```Less
.recommend {
    #layout(recommend);
    .recommend-course {
        #course-box(recommend);
        &__cover {
            width: 100%;
            height: 172px;
            overflow: hidden;
            img {
                width: 100%;
            }
        }
```

