# 容器

## 流体容器(fluid container)

width:auto padding : auto  15px

## 固定容器(container)

padding : auto  15px

视口阈值                                                width
小于768px(xs 移动手机)                         auto

小于992px                                            750px（720px + 槽宽）
大于等于768px(sm 平板)

小于1200px                                          970px（940px + 槽宽）
大于等于992px(md 中屏PC)

大于等于1200px(lg 大屏PC)                 1170px（1140px + 槽宽）

# 栅格系统源码分析（grid）

## 容器源码分析

```
//固定容器和流体容器的公共样式
//margin: auto
//padding: auto 15px
//清除浮动

.container-fixed(@gutter: @grid-gutter-width) { //@grid-gutter-width 槽宽 30px
  margin-right: auto;
  margin-left: auto;
  padding-left:  floor((@gutter / 2));  //15px
  padding-right: ceil((@gutter / 2));   //15px
  &:extend(.clearfix all);  //清除浮动
}

//固定容器
//注意媒体查询顺序不可变，应该从小到大
.container {
  .container-fixed();
  
  //固定容器的媒体查询
  //@screen-sm-min 768px
  //@screen-md-min 992px
  //@screen-lg-min 1200px
  
  @media (min-width: @screen-sm-min) {
    width: @container-sm; //@container-sm = 720px + @grid-gutter-width = 750px
  }
  @media (min-width: @screen-md-min) {
    width: @container-md; //@container-md = 940px + @grid-gutter-width = 970px
  }
  @media (min-width: @screen-lg-min) {
    width: @container-lg; //@container-lg = 1140px + @grid-gutter-width = 970px
  }
}

//流体容器
.container-fluid {
  .container-fixed();
}
```

## 行列源码分析

```
//行

.row {
  .make-row();
}
//margin: auto -15px
//清除浮动
.make-row(@gutter: @grid-gutter-width) {
  margin-left:  ceil((@gutter / -2));
  margin-right: floor((@gutter / -2));
  &:extend(.clearfix all);
}

//列

//列的第一步
//position: relative;
//min-height: 1px;
//padding: auto 15px
.make-grid-columns();
.make-grid-columns() {
  .col(@index) {
    @item: ~".col-xs-@{index}, .col-sm-@{index}, .col-md-@{index}, .col-lg-@{index}";
    .col((@index + 1), @item);
    //.col((2), ".col-xs-1, .col-sm-1, .col-md-1, .col-lg-1");
  }
  .col(@index, @list) when (@index =< @grid-columns) { //@grid-columns 栅格列数默认12
    @item: ~".col-xs-@{index}, .col-sm-@{index}, .col-md-@{index}, .col-lg-@{index}";
    .col((@index + 1), ~"@{list}, @{item}");
    //.col(3, ".col-xs-1, .col-sm-1, .col-md-1, .col-lg-1, .col-xs-2, .col-sm-2, .col-md-2, .col-lg-2")
    //.........
    //.col(13, str)
    str:.col-xs-1, .col-sm-1, .col-md-1, .col-lg-1,
        .col-xs-2, .col-sm-2, .col-md-2, .col-lg-2,
        .col-xs-3, .col-sm-3, .col-md-3, .col-lg-3,
        .col-xs-4, .col-sm-4, .col-md-4, .col-lg-4,
                                .
                                .
                                .
        .col-xs-12, .col-sm-12, .col-md-12, .col-lg-12,
  }
  //str传入变为选择器给所有列加样式
  .col(@index, @list) when (@index > @grid-columns) {
    @{list} {
      position: relative;
      min-height: 1px;
      padding-left:  ceil((@grid-gutter-width / 2));
      padding-right: floor((@grid-gutter-width / 2));
    }
  }
  .col(1);
}

//列的第二步（注意移动优先）
//列浮动
//列排序
//列偏移

.make-grid(xs);
.make-grid(@class) {
  //2.1
  //.col-xs-1,........., .col-xs-12 {float:left}
  .float-grid-columns(@class);
  //2.2
  //.col-xs-12 {width: 12/12;}, .col-xs-11 {width: 11/12;} ......., .col-xs-1 {width: 1/12;}
  .loop-grid-columns(@grid-columns, @class, width);
  //2.3（列排序）
  //.col-xs-push-12 {left: 12/12;}, .col-xs-push-11 {left: 11/12;} ......., .col-xs-push-1 {left: 1/12;}.col-xs-push-0 {left: auto}
  //.col-xs-pull-12 {right: 12/12;}, .col-xs-pull-11 {right: 11/12;} ......., .col-xs-pull-1 {right: 1/12;}.col-xs-pull-0 {right: auto}
  .loop-grid-columns(@grid-columns, @class, pull);
  .loop-grid-columns(@grid-columns, @class, push);
  //2.4（列偏移）
  //.col-xs-offset-12 {margin-left: 12/12}........ .col-xs-offset-0 {margin-left: 0/12}
  .loop-grid-columns(@grid-columns, @class, offset);
}
.float-grid-columns(@class) {
  .col(@index) {
    @item: ~".col-@{class}-@{index}";
    .col((@index + 1), @item);
  }
  .col(@index, @list) when (@index =< @grid-columns) {
    @item: ~".col-@{class}-@{index}";
    .col((@index + 1), ~"@{list}, @{item}");
  }
  //.col(13, ".col-xs-1, .col-xs-2,......., col-xs-12")
  .col(@index, @list) when (@index > @grid-columns) {
    @{list} {
      float: left;
    }
  }
  .col(1); 
}

.loop-grid-columns(@index, @class, @type) when (@index >= 0) {
  .calc-grid-column(@index, @class, @type);
  .loop-grid-columns((@index - 1), @class, @type);
}
.calc-grid-column(@index, @class, @type) when (@type = width) and (@index > 0) {
  .col-@{class}-@{index} {
    width: percentage((@index / @grid-columns));
  }
}
.calc-grid-column(@index, @class, @type) when (@type = push) and (@index > 0) {
  .col-@{class}-push-@{index} {
    left: percentage((@index / @grid-columns));
  }
}
.calc-grid-column(@index, @class, @type) when (@type = push) and (@index = 0) {
  .col-@{class}-push-0 {
    left: auto;
  }
}
.calc-grid-column(@index, @class, @type) when (@type = pull) and (@index > 0) {
  .col-@{class}-pull-@{index} {
    right: percentage((@index / @grid-columns));
  }
}
.calc-grid-column(@index, @class, @type) when (@type = pull) and (@index = 0) {
  .col-@{class}-pull-0 {
    right: auto;
  }
}
.calc-grid-column(@index, @class, @type) when (@type = offset) {
  .col-@{class}-offset-@{index} {
    margin-left: percentage((@index / @grid-columns));
  }
}







@media (min-width: @screen-sm-min) {
  .make-grid(sm);
}

@media (min-width: @screen-md-min) {
  .make-grid(md);
}

@media (min-width: @screen-lg-min) {
  .make-grid(lg);
}


```

# 栅格盒模型设计的精妙之处

为了维护槽宽的规则，列两边必须有15px的padding
而如果列里再套行就会导致槽宽增加，所以行两边具有-15px的margin
又因为行两边具有了-15的margin，为了套住行，container容器两边加上了15px的padding

# 响应式工具（responsive.less）

.visible-xs,.visible-sm,.visible-md,.visible-lg
先display:none
当满足相应的媒体查询时显示

.hidden-xs,.hidden-sm,.hidden-md,.hidden-lg则相反

# Bootstrap定制化

新建一个Less文件
```
//引入bootstrap.less
@import "bootstrap.less";
//修改变量
@grid-gutter-width:200px;
```