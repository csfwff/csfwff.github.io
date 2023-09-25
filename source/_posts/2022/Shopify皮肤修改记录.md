---
title: Shopify皮肤修改记录
date: 2022-08-03 13:23:22
tags: [CSS,前端,Shopify]
categories: 
- 菜鸡修炼手册
permalink: /articles/2022/08/03/1659504202231.html
top_img: https://tmx.fishpi.cn/img/flowers-7233987_1920.jpg
cover: https://tmx.fishpi.cn/img/flowers-7233987_1920.jpg
---
![1628345033618.jpg](https://tmx.fishpi.cn/img/flowers-7233987_1920.jpg)

## 顶部广告图修改
皮肤自定义里**Header Default**，编辑**Top Message Text**，内容如下`{表示说明}`，改的时候去掉大括号
``` html
<a href="{换成点击跳转地址}" style="display:block">		 
<picture>
  <source media="(max-width: 500px)" srcset="{换成手机端图片地址}">
  <source media="(min-width: 501px)" srcset="{换成PC端图片地址}">
  <img src="{换成PC端图片地址}">
</picture>
</a>
```

## logo和nav居中修改
`theme-styles-responsive.scss.liquid` 6712行附近
``` scss
  .site-nav {
        margin: 0;
        display:flex;  /*新增加*/
        justify-content:center; /*新增加*/

        .icon-dropdown,
        .menu-mb-title {
            display: none;
        }
```
`header.liquid` 24行，`container`增加`max-width`
``` html
<div class="header-bottom" data-sticky-mb>
    <div class="container" style="max-width:1350px;"> <!-- 这里加max-width -->
       <div class="wrapper-header-bt">
```

## newsletter手机端修改
`theme-styles-responsive.scss.liquid` 1352行附近
``` scss
    .wrapper-newsletter {
        .modal-overlay {
            width: 400px;
            @include calc(max-width, '100% - 40px');
        }

        .halo-modal-body {
            .column-left {
                display: none;
            }

             .column-right-img{
                display:block;

                img{   /*新增这个img*/
                  width:100%
                }
            }

            .column-right {
                width: 100%;
                /*padding: 35px 20px 25px;   padding改为0 */
                padding:0;
            }

            .title {
                font-size: $font_size + 6;
                display:none;   /*新增这个*/
            }
        }
    }
```

## newsletter增加手机端图片
`settings_schema.json`1097行附近增加选项
```json
    {
        "type": "image_picker",
        "id": "newsletter_mobile",
        "label": "Upload a new newsletter image for mobile"
    }
```
`newsletter.liquid`33行附近
```liquid
    {% if settings.popup_newsletter_title != blank %}
        <h2 class="title">
        {% render 'multilang' with settings.popup_newsletter_title %}
        </h2>
        {% endif %}
        <!--以下新增 现有就修改-->
        <div class="column-right-img">
            {%- assign img_url = settings.newsletter_mobile | img_url: '360x180' -%}
                      
            {% if settings.newsletter != blank %}
                <img data-srcset="{{ img_url }}" class="lazyload" data-sizes="auto">
            {% else %}
                <div class="not_img">
                          360 x 180px
                </div>   
            {% endif %}
        </div>
        <!--以上新增 -->
```

## bestseller产品增加边框
`theme-styles-scss.liquid` 2357行附近
```scss
.product-item {
    .product-top {
        position: relative;
        text-align: center;
        padding: 10px;  /*新增这个*/
        border: 1px solid #cccccc;  /*新增这个*/
    }

    .product-grid-image {
        position: relative;
        display: block;
    }
```
2306行附近
```scss
    .product-image {
        .product-grid-image {
            min-height: unset;/*改为unset 原先216px*/
        }

        img {
             min-height: unset;/*改为unset 原先216px*/
        }
    }
```

`theme-styles-responsive.scss.liquid` 4834行附近
```scss
 .product-image {
            margin-bottom: 0px;/*改为0 原先13px*/
    }
```
4805行附近
```scss
    &.abs-center {
                @include transform(none);
                top: inherit;
                bottom: -1px;;
                display:none; /*新增这个*/
            }
```

## 去除货币选择
自带的货币选择logo的图片无法访问，导致加载半天然后加载失败
直接隐藏货币选择

在`header.liquid`或带01 02的里
搜索`new-currency-picker`，然后注释掉render那一行
理论上关闭多语言多货币多支付方式就好了

