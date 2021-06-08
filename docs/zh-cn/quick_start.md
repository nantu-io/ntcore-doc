##  <b>快速开始</b> <!-- {docsify-ignore} -->
### 环境设置

1. **下载并安装Docker**

   NTCore平台依赖docker来启动一个本地的服务。请根据这里的<em>[docker官方指南](https://docs.docker.com/get-started/#download-and-install-docker)</em>来下载并安装docker引擎。请确保Docker安装正确。
   ```
   docker-compose --version
   ```

2. **下载NTcore github代码**

  通过git的clone命令下载NTCore代码仓的代码
  ```
  git clone https://github.com/nantutech/ntcore.git
  ```

3. **启动NTCore服务**

  转到ntcore路径并启动(请确保docker服务已经启动)
  ```
  cd ntcore/
  docker-compose up
  ```

4. **登录NT-Core平台首页**
  在浏览器中打开<em>http://localhost:8000/dsp/console/home</em>。如果出现如下所示的平台首页, 那代表环境已经搭建成功。
  <img src="./media/workspace-home.png" style="border:1px solid #F7F7F7; border-radius:5px;" />

---
### 建立第一个模型
以下例子使用Sklearn建立了一个简单的Decision Tree模型，并运用NTCore记录模型结果。

安装：
```
git clone https://github.com/nantutech/mlflow.git mlflow-nantu
pip3 install ntcore
```

建模与模型版本控制：
```
from sklearn.datasets import load_iris
from sklearn.tree import DecisionTreeClassifier
import ntcore
# enable autologging
ntcore.sklearn.autolog()

iris = load_iris()
X = iris.data[:, 2:] # petal length and width
y = iris.target

tree_clf = DecisionTreeClassifier(max_depth=2, random_state=42)
# train a model

with mlflow.start_run() as run:
    tree_clf.fit(X, y)
```
