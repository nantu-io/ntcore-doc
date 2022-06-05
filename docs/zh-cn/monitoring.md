# <b>模型监控</b> <!-- {docsify-ignore} -->

## 日志监控
通过SDK用户可以把实时日志发送到NTCore，并通过NTCore Dashboard搜索来帮助排错。
```
monitor = Monitor({workspace_id}, server="http://{hostname}:{port}")
monitor.log("This is a demo")
```

## 指标监控
通过SDK用户可以把实时指标发送到NTCore，并通过NTCore Dashboard来监测异常状况。
```
monitor = Monitor({workspace_id}, server="http://{hostname}:{port}")
monitor.add_metric(metric_name, metric_value)
```
### API指标
目前NTCore Dashboard支持以下几种指标。我们将会支持用户自定义的指标。
- API健康状态：
    ```
    monitor.add_metric("Success", 1.0)
    monitor.add_metric("Error", 1.0)
    ```
- API运算时间：
    ```
    monitor.add_metric("Latency", 150.0) // milliseconds
    ```
- API访问量。NTCore会根据上述健康状态指标计算出访问量，无需用户额外调用`add_metric`。

### 系统指标
目前NTCore Dashboard支持以下几种系统指标。我们将会支持更多的系统指标，如GPU使用率等。
- CPU使用率
- 内存使用率
- 网络传输量

在上述SDK基础上，我们提供了`SystemMetricsPublisherDaemon`以方便自动收集系统指标，用户可根据如下方式使用。
```
monitor = Monitor({workspace_id}, server="http://{hostname}:{port}")
system_metrics_daemon = SystemMetricsPublisherDaemon(monitor)
system_metrics_daemon.start()
```

## 完整例子
用户可以参考以下的Flask例子使用Monitoring SDK。
```
from ntcore.monitor import Monitor, SystemMetricsPublisherDaemon
from flask import Flask

monitor = Monitor({workspace_id}, server="http://{hostname}:{port}")
system_metrics_daemon = SystemMetricsPublisherDaemon(monitor)
system_metrics_daemon.start()

app = Flask(__name__)

@app.route("/predict")
def predict():
    start_time = round(time.time() * 1000)
    try:
        /* 此处补充用户的预测逻辑 */
        monitor.add_metric("Success", 1.0)
    except Exception as e:
        monitor.log("[Error] Unable to generate prediction: {0}".format(str(e)))
        monitor.add_metric("Error", 1.0)
    finally:
        monitor.add_metric("Latency", round(time.time() * 1000) - start_time)
```