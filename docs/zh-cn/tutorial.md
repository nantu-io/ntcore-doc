## <b>详细教程</b> <!-- {docsify-ignore} -->

### 工作台 Workspace

每一个 Workspace 对应着一个完整的端到端建模过程，包括数据上传，数据处理，模型实验到模型部署。

用户需要完成以下安装：

```
git clone https://github.com/nantu-io/ntcore.git
pip3 install ntcore
```

创建 Workspace 的时候，用户可以选择对应的开发类型。

<img src="./media/workspace-create.png" style="border:1px solid #F7F7F7; border-radius:5px;" />

<b>Python 例子:</b>

```
from ntcore import client

client = Client(server = 'http://localhost:8000/')

name = "test"
client.create_workspace(name)
```

成功创建 Workspace 后，将产生对应的 ID。

<img src="./media/workspace-id.png" style="border:1px solid #F7F7F7; border-radius:5px;" />

进入 Workspace 后，可以看到模型实验，模型注册以及模型部署开发流程。

<img src="./media/workspace-development.png" style="border:1px solid #F7F7F7; border-radius:5px;" />

<b>Python 例子:</b>

```
from ntcore import client

workspace_id = "C8W60XEPH7DA3AAH3S41PJZ3OV" # any available workspace ids in your console
client.get_workspace(workspace_id)
```

#### 删除 Workspace

用户可删除特定 Workspace

<b>Python 例子:</b>

```
from ntcore import client

workspace_id = "C8W60XEPH7DA3AAH3S41PJZ3OV" # any available workspace ids in your console
client.delete_workspace(workpsace_id)
```

#### 列举所有 Workspace

用户可列举所有 Workspace

<b>Python 例子:</b>

```
from ntcore import client

client.list_workspaces()
```

---

### 模型 Experiment

NTCore 提供模型版本控制功能，用户调整参数，并通过平台记录与对比不同模型结果。

用户需要完成以下安装：

```
git clone https://github.com/nantu-io/ntcore.git
pip3 install ntcore
```

比如在运用 SKlearn 进行模型开发实验时，只需在代码中加入以下方法便可记录模型结果。

<b>Python 例子:</b>

```
from ntcore import client
#指向URL
client.set_endpoint('http://localhost:8000')
#指向 workspace ID
client.autolog('C8W60XEPH7DA3AAH3S41PJZ3OV')
```

模型结果将会在界面上得到展示。以下是对所有模型实验结果进行记录。

<img src="./media/workspace-experiment.png" style="border:1px solid #F7F7F7; border-radius:5px;" />

#### 删除实验

删除特定 Workspace 里所有实验

<b>Python 例子:</b>

```
from ntcore import client

workspace_id = "C82NM5766JT36WL4764YDVH9SU" # 任何现有的workspace id
version = 1 # 任意现有的版本
client.delete_experiment(workspace_id, version)
```

---

### 模型 Registry

在得到比较满意的模型结果后，用户可以对将要部署的模型进行注册并进行测试，以确保模型部署的准确性。

用户需要完成以下安装：

```
git clone https://github.com/nantu-io/ntcore.git
pip3 install ntcore
```

#### 注册模型。

<img src="./media/workspace-reg.png" style="border:1px solid #F7F7F7; border-radius:5px;" />

<b>Python 例子:</b>

```
from ntcore import client

workspace_id = "C82NM5766JT36WL4764YDVH9SU" # 任何现有的workspace id
version = 1 # 任意现有的版本
client.register_experiment(workspace_id, version)
```

#### 已注册模型展示。

<img src="./media/workspace-reg-UI.png" style="border:1px solid #F7F7F7; border-radius:5px;" />

<b>Python 例子:</b>

```
from ntcore import client

workspace_id = "C82NM5766JT36WL4764YDVH9SU" # 任何现有的workspace id
client.get_registry(workspace_id)
```

#### 取消模型注册

<b>Python 例子:</b>

```
from ntcore import client

workspace_id = "C82NM5766JT36WL4764YDVH9SU" # 任何现有的workspace id
client.unregister_experiment(workspace_id)
```

---

### 模型 Deployment

测试无误后，用户可以通过 UI 一键发布成果。NTCore 将自动生成模型 API。

用户需要完成以下安装：

```
git clone https://github.com/nantu-io/ntcore.git
pip3 install ntcore
```

<img src="./media/workspace-deploy.png" style="border:1px solid #F7F7F7; border-radius:5px;" />

用户可以用 curl 或 Python 对模型 API 进行使用。

<b>Curl 例子:</b>

```
curl -H "Content-Type: application/json" -X POST --data '{"data": [[1,1 ...]]}' http://localhost:8000/s/C82NM5766JT36WL4764YDVH9SU/predict
```

<b>Python 例子:</b>

```
from ntcore import client

workspace_id = "C82NM5766JT36WL4764YDVH9SU" # 任何现有的workspace id
version = 1 # 任意现有的版本
client.deploy_model(workspace_id, version)
```

#### 对于所有成功 deploy 的模型进行汇总和追踪。

<img src="./media/workspace-model-track.png" style="border:1px solid #F7F7F7; border-radius:5px;" />

<b>Python 例子:</b>

```
from ntcore import client

client.list_active_deployments()
```
