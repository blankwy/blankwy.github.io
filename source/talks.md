<head>
  <!-- ... -->
  <script src="//cdn.jsdelivr.net/gh/Uyoahz26/daodao@main/dist/qexo-dao.min.js"></script>
  <!-- ... -->
</head>
<body>
  <!-- ... -->
  <div id="qexoDaoDao"></div>
  <script>
    qexoDaodao?.init({
      el: "#qexoDaoDao",
      avatar: "https://q1.qlogo.cn/g?b=qq&nk=2035650646&s=640",
      name: "Binarydev",
      limit: 10,
      useLoadingImg: false,
      baseURL: "https://hexo-admin.binarydev.top/",
      title: "说说",
    }).then(function (){
      console.log("絮絮叨叨加载完成");
    })
  </script>
</body>
