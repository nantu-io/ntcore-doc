##  <b>详细教程</b> <!-- {docsify-ignore} -->

### 创建Workspace
每一个Workspace对应着一个完整的端到端建模过程，包括数据上传，数据处理，模型实验到模型部署。

创建Workspace的时候，用户可以选择对应的开发类型。

<img src="./media/workspace-create.png" style="border:1px solid #F7F7F7; border-radius:5px;" />

成功创建Workspace后，可以看到完整的模型开发流程。

<img src="./media/workspace-development.png" style="border:1px solid #F7F7F7; border-radius:5px;" />

---
### 开关Instance
开启Instance将为用户提供相应的模型开发工具。用户可以自定义所有需要的算力，并安装模型开发所需软件包。

<img src="./media/workspace-instance.png" style="border:1px solid #F7F7F7; border-radius:5px;" />

在打开Jupyter后，将自动生成和Workspace相对应的folder。

<img src="./media/workspace-jupyter.png" style="border:1px solid #F7F7F7; border-radius:5px;" />

---
### 模型Experiment
NTCore提供模型版本控制功能，用户调整参数，并通过平台记录与对比不同模型结果。

用户需要完成以下安装：

```
git clone https://github.com/nantutech/mlflow.git mlflow-nantu
pip3 install ntcore
```

比如在运用SKlearn进行模型开发实验时，只需在代码中加入以下方程便可记录模型结果。

```
ntcore.sklearn.autolog()
```

模型结果将会在界面上得到展示。以下是不同模型实验得出的结果。

<img src="./media/workspace-experiment.png" style="border:1px solid #F7F7F7; border-radius:5px;" />

---
### 模型Deploy
当用户获得比较满意的模型结果后，可以通过UI一键发布成果。NTCore将自动生成模型API。

<img src="./media/workspace-deploy.png" style="border:1px solid #F7F7F7; border-radius:5px;" />

Deploy成功后，可以在UI查询状态。

<img src="./media/workspace-deploy-status.png" style="border:1px solid #F7F7F7; border-radius:5px;" />

用户可以用curl或Python对模型API进行使用。

<b>Curl例子:</b>
```
curl -H "Content-Type: application/json" -X POST --data '{"data": [[1,1 ...]]}' http://localhost:8000/s/C82NM5766JT36WL4764YDVH9SU/predict
```

<b>Python例子:</b>
```
import requests
requests.post('http://localhost:8000/s/C82NM5766JT36WL4764YDVH9SU/predict', data={"data": [[1,1 ...]]})
```

对于所有成功deploy的模型进行汇总和追踪。

<img src="./media/workspace-model-track.png" style="border:1px solid #F7F7F7; border-radius:5px;" />