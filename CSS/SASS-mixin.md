**mixin을 사용한 여러줄 말줄임**

```sass
@mixin line_clamp($font-size, $line-height, $lines) {
    display: block; // For non-webkit browsers
    display: -webkit-box;
    font-size: $font-size;
    line-height: $line-height;
    max-height: $line-height * $lines;
    overflow: hidden;
    text-overflow: ellipsis;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: $lines;
}
```
