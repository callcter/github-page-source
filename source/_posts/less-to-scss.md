---
title: 将 LESS 转化为 SCSS
tags:
  - CSS
---


#### 色彩函数

```css
fade(@color, @alpha) // less
rgba($color, $alpha) // scss

mix(@color1, @color2, @weight) // less
mix($color1, $color2, $weight) // scss

tint(@color, @weight) // less
mix(#fff, $color, $weight) // scss

shade(@color, @weight) // less
mix(#000, $color, $weight) // scss
```