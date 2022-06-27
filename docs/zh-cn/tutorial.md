##  <b>详细教程</b> <!-- {docsify-ignore} -->

### 创建项目
每一个项目(Workspace)对应着一个完整的建模过程，包括模型试验，注册，部署以及监控。用户可以通过NTCore UI或者SDK创建项目。

#### 通过UI创建项目

在Workspaces页面点击右上角的"+"按钮。

<img src="./media/workspace-create.png" style="border:1px solid #F7F7F7; border-radius:5px;" />

成功创建项目后，将产生对应的ID。

<img src="./media/workspace-id.png" style="border:1px solid #F7F7F7; border-radius:5px;" />

#### 通过SDK创建项目
```
from ntcore import Client

client = Client(server="my_server_url")
workspace = client.create_workspace("my_workspace_name")
print(workspace['id'])
```

---
### 记录模型
NTCore提供模型版本控制功能，用户可以记录并审核实验过程中的模型的每个版本。以下是基于tensorflow的例子。
<details>
  <summary>展开查看具体的模型训练代码</summary>

    import tensorflow as tf

    mnist = tf.keras.datasets.mnist
    (x_train, y_train), (x_test, y_test) = mnist.load_data()
    x_train, x_test = x_train / 255.0, x_test / 255.0

    model = tf.keras.models.Sequential([
    tf.keras.layers.Flatten(input_shape=(28, 28)),
    tf.keras.layers.Dense(128, activation='relu'),
    tf.keras.layers.Dropout(0.2),
    tf.keras.layers.Dense(10)
    ])
    loss_fn = tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True)
    model.compile(optimizer='adam', loss=loss_fn, metrics=['accuracy'])
</details>

```
# 嵌入NTCore SDK对模型进行记录
from ntcore import Client

client = Client(server="my_server_url")
workspace_id = "my_workspace_id"
with client.start_run(workspace_id) as exper:
  model.fit(x_train, y_train, epochs=2, experiment=exper)
  model.evaluate(x_test,  y_test, verbose=2, experiment=exper)
  exper.save_model(model)
```

以下是所记录的模型版本。

<img src="./media/workspace-experiment.png" style="border:1px solid #F7F7F7; border-radius:5px;" />

---
### 注册模型
在得到比较满意的模型结果后，用户可以对将要部署的模型进行注册并进行测试，以确保模型部署的准确性，用户可以通过NTCore UI或者SDK注册模型。

#### 通过UI注册模型

选择合适的模型，点击对应的Register按钮。

<img src="./media/workspace-reg.png" style="border:1px solid #F7F7F7; border-radius:5px;" />

在Registry页面查看已注册的模型。

<img src="./media/workspace-reg-UI.png" style="border:1px solid #F7F7F7; border-radius:5px;" />

#### 通过SDK注册模型
```
from ntcore import Client

client = Client(server="my_server_url")
# my_model_version = 1
client.register_experiment("my_workspace_id", my_model_version)
```

---
### 模型部署
测试无误后，用户可以通过UI或者SDK快速发布成果，NTCore将自动生成模型API。用户可以通过NTCore UI或者SDK部署模型。

#### 通过UI部署模型
点击已注册模型(Registry)页面中的"Deploy"按钮，开始部署模型。Deployments页面展示了模型部署的历史。

<img src="./media/workspace-deploy.png" style="border:1px solid #F7F7F7; border-radius:5px;" />

#### 通过SDK部署模型

```
from ntcore import Client

client = Client(server="my_server_url")
client.deploy_model("my_workspace_id")
```

#### 使用模型API
用户可以使用CURL，requests (Python)，Jersey (Java)和Postman等工具调用模型API。以下的例子使用了Python requests调用上面例子中的tensorflow模型。
```
import requests

url = 'http://{server_url}/s/{workspace_id}/predict'.format(server_url="my_server_url", workspace_id="my_workspace_id")
requests.post(url, json={"instances": [1] * 784})
```

#### 追踪模型API

点击页面左边Deployments选项，追踪所有成功部署的模型。

<img src="./media/workspace-model-track.png" style="border:1px solid #F7F7F7; border-radius:5px;" />
