## 节流

``节流``原理:    
如果在定时器执行时间内再次触发，不予理睬;直到定时器**完成**，才能开始下一个任务;    

```javascript
  function throttle(fn, interval){
    let flag = true;
    return function(...args) {
      let context = this;
      if(!flag) return;
      flag = false;
      setTimeout({
        fn.apply(context, args);
        flag = true;
      }, interval);
    }
  }
```

## 防抖

``防抖``原理:    
每次事件触发重新建立定时器，删除之前的定时器。    

```javascript
  function debounce(fn, delay){
    let timer = null;
    return function(...args) {
      let context = this;
      if(timer) clearTimeout(timer);
      timer = setTimeout(()=>{
        fn.apply(context, args);
      }, delay);
    }
  }
```

