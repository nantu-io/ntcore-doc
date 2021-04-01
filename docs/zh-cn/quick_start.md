# 快速开始
## 环境设置

1. **下载并安装Docker**

   NT-Core平台依赖docker来启动一个本地的服务。请根据这里的<em>[docker官方指南](https://docs.docker.com/get-started/#download-and-install-docker)</em>来下载并安装docker引擎。

2. **下载github代码**

  通过git的clone命令下载NT-Core代码仓的代码
  ```
  git clone https://github.com/dsp-columbus/ntcore.git
  ```

3. **打包前端网页资源**

  进入到刚下载好的NT-Core代码仓中的webapp文件夹下面，打包前端所需资源
  ```
  cd webapp/
  npm install .
  npm run build
  ```

4. **启动NT-Core服务器**

  ?> 在这步之前请确保您的8180端口没有被占用
  ```
    cd ../
    npm run dev
  ```

5. **登录NT-Core平台首页**
  在浏览器中打开<em>http://localhost:8180/dsp/console/home</em>。如果您能看到如下所示的平台首页, 那代表环境已经搭建成功，您可以开始使用平台了。
  <img src="./media/nt-platform-home.png"  />


## 创建您的第一个模型