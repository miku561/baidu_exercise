function Observer(data){
  this.data = data;
  this.scan(data);
}

let p = Observer.prototype;
p.scan = function(obj){
  let val;
  for(let key in obj){
    if(obj.hasOwnProperty(key)){
      val = obj[key];
      if(typeof(val) === "object"){
        new Observer(val);
      }
      this.watch(key, val);
    }
  }
}

p.watch = function(key, val){
  Object.defineProperty(this.data, key, {
    enumerable: true,
    configurable: true,
    get: function(){
      console.log('你成功访问了' + key);
      return val;
    },
    set: function(newVal){
      if (newVal === val) return;
      console.log('您更改了' + key);
      console.log('新的' + key + '=' + newVal);
      val = newVal;
    }
  })
}

let app1 = new Observer({
  name: "youngwind",
  age: 25
});

let app2 = new Observer({
  university: "bupt",
  major: "computer"
});
