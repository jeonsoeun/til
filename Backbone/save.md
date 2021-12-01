# Backbone - save()

Backbone model.save()를 하게되면 model에 적힌 url로 post request를 보낸다. 그러면 body값은 어떻게 설정하느냐?

Model이 아래와 같이 있을 때, defaults에 들어가는 값이 request의 body값으로 들어간다! 

```js
...
export var Model = Backbone.Model.extend({
  url: 'some/link',

  defaults: function () {
    return  {
      type: 1
    }
  },
  
  initialize: function() {}, // 얘는 그럼 뭔역할이람?

  parse: function(res) {
    // 결과 파싱.
    return _.extend({}, res.result, history) // 이제 underscore(_) 안쓰는데 extend의 역할이 뭘까?
  }
})

...
```
그럼 저 defaults에 있는 변수는 어떻게 바꾸느냐? `Model.set('type', 2)` 이렇게 바꿔주면 된다.