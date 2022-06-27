##  <b>快速开始</b> <!-- {docsify-ignore} -->
### NTCore SDK
用户可通过NTCore SDK记录和部署模型。运行以下命令安装SDK。
```
pip3 install ntcore
```

### 本地开发环境（可选）
如用户需要对NTCore代码进行修改，可根据以下步骤配置开发环境。
1. **下载并安装Docker**

  请根据这里的<em>[docker官方指南](https://docs.docker.com/get-started/#download-and-install-docker)</em>下载并安装docker引擎。运行以下命令确保Docker安装正确。
  ```
  docker-compose --version
  ```

2. **下载NTCore github代码**

  通过git的clone命令下载NTCore代码仓的代码
  ```
  git clone https://github.com/nantu-io/ntcore.git
  ```

3. **启动NTCore服务**

  转到ntcore路径并启动NTCore(请确保docker服务已经启动)
  ```
  cd ntcore/
  docker-compose up
  ```

4. **登录NTCore平台首页**
  在浏览器中打开<em>http://localhost:8000/dsp/console/home</em>。如果出现如下所示的平台首页, 那代表环境已经搭建成功。
  <img src="./media/workspace-home.png" style="border:1px solid #F7F7F7; border-radius:5px;" />
