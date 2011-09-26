## NEVER USE INLINE STILE!

An haml template like:

```haml
#content.padding{:style => "background:url(/images/middle.png) no-repeat top center #fff;"}
```

must be converted in css:

```css
#content { background:url(/images/middle.png) no-repeat top center #fff;padding:5px } 
```

## NEVER USE ID UNLESS IT IS NEEDED BY JS

```haml
#content
  My Content
```

must be:

```haml
.content
  My Content
```

## NAMESPACE WITH CSS CLASSES

```haml
.login-fields=login_here
.login-warnings=warnings_here
```

must be:

```haml
.login
  .fields=login_here
  .warnings=warnings_here
```

## NEVER ABUSE OF CONCATENATED CLASSES

```haml
.content.right.aright.padding-top.padding-bottom
```

must be rewritten in css:

``` css
.content{float:right;text-align:right;padding:5px 0px 5px 0px}
```

## COMPRESS ALWAYS CSS

```css
.theme-default .slider-wrapper{background:url(/images/slider.png) no-repeat;width:722px;height:337px;padding-top:18px;position:relative;margin:0 auto;}
.theme-default .slider{position:relative;width:568px;height:268px;margin-left:77px;background:url(/images/loading.gif) no-repeat 50% 50%;}
.theme-default .slider img{position:absolute;top:0;left:0;display:none;width:568px;height:268px;}
.theme-default .slider a{border:0;display:block;}
```

## USE STYLUS

We recommend to use [stylus](http://learnboost.github.com/stylus/)
