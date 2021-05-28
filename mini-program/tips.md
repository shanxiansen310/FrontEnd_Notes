1.组件属性中如果是false, true需要加上{{}}

```html
<video id="daxueVideo" src="https://image-list-1258374833.cos.ap-chengdu.myqcloud.com/D50EC79580B2788ED11780E489D2CE28.mp4" autoplay="{{false}}" muted="{{false}}" duration="12" title="抄底">
</video>
```

如果直接写autoplay那么就是按语意自动播放, 如果是muted那么就是静音

```html
<video id="daxueVideo" autoplay muted duration="12" title="抄底">
</video>
```

即 autoplay 等价于  autoplay={{true}} ,  muted  等价于  muted={{false}}  

<video id="daxueVideo" src="https://image-list-1258374833.cos.ap-chengdu.myqcloud.com/D50EC79580B2788ED11780E489D2CE28.mp4" autoplay="{{false}}" muted="{{false}}" duration="12" title="抄底">
  </video>



<video id="daxueVideo" src="https://image-list-1258374833.cos.ap-chengdu.myqcloud.com/D50EC79580B2788ED11780E489D2CE28.mp4" autoplay="{{false}}" muted="{{false}}" duration="12" title="抄底">
  </video>

