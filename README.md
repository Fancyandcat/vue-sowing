## 这是一个基于vue的轮播图

+ 原理
 - 利用v-show 配合 *-enter-to等css3过渡属性实现
 - 使用函数截留防止快速点击问题
+ 方法
 - 当作一般组件放置于项目中
 - 使用compontent载入
+ props依赖说明
 属性名|类型|说明|默认
 -|-|-|-
 list|Array|需要轮播的数组|[]
 title|String|轮播图的title|''
 option|Object|轮播图的配置对象|{}
 -|isLoop|是否可以循环|true
 -|isAutoLoop|是否开启自动播放|true
 -|loopTime|若开启自动轮播的时间|3000