测试markdown的显示效果

```ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
```

```JavaScript
var m;
var body = window.document.body;
for(var i; i < 10; i++){
	alert(i)
}
```

- [x] @mentions, #refs, [links](), **formatting**, and <del>tags</del> supported
- [x] list syntax required (any unordered or ordered list supported)
- [x] this is a complete item
- [ ] this is an incomplete item


玫瑰是红的  
Violets are blue

