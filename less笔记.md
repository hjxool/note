## 设置变量

- 可以多处调用

  ```less
  @color1: #fff; // 必须有;号
  .bg1{
      background: @color1;
  }
  .font{
      color: @color1;
  }