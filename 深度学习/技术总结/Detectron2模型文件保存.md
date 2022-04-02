# Detectron2 模型文件保存

- [Detectron2 模型文件保存](#detectron2-模型文件保存)
  - [Pytorch 保存/加载模型](#pytorch-保存加载模型)
    - [保存/加载 static_dict（模型参数）](#保存加载-static_dict模型参数)
    - [保存/加载完整模型](#保存加载完整模型)
  - [Detectron2 使用模型](#detectron2-使用模型)
  - [参考资料](#参考资料)

## Pytorch 保存/加载模型

Pytorch 有两种保存/加载模型的方式：

1. 保存/加载 static_dict（模型参数）
2. 保存/加载完整模型

> PS：两种方式保存的模型一般都是 `.pth` 文件，难以区分。
>
> 通常通过判断 `torch.load(PATH)` 类型进行区分：
> - static_dict 方式为 `dict`
> - 完整模型为 `torch.nn.Module` 的子类。

### 保存/加载 static_dict（模型参数）

- 保存

    ```python
    torch.save(model.state_dict(), PATH)
    ```

- 加载

    ```python
    model = TheModelClass(*args, **kwargs)  # returns a torch.nn.Module
    state_dict = torch.load(PATH)  # returns a dict
    model.load_state_dict(state_dict)
    model.eval()
    ```

### 保存/加载完整模型

- 保存

    ```python
    torch.save(model, PATH)
    ```

- 加载

    ```python
    # 模型类必须在此之前被定义
    model = torch.load(PATH)  # returns a torch.nn.Module
    model.eval()
    ```

## Detectron2 使用模型

Detectron2 的模型（及其子模型）使用 `CfgNode` 对象作为参数通过 `build_model`、`build_backbone`、`build_roi_heads` 等函数构建：

```python
from detectron2.modeling import build_model
model = build_model(cfg)  # returns a torch.nn.Module
```

`build_model` 只构建模型结构并用随机参数填充，之后需要将现有检查点加载到模型：

```python
from detectron2.checkpoint import DetectionCheckpointer
DetectionCheckpointer(model).load(cfg.MODEL.WEIGHTS)

checkpointer = DetectionCheckpointer(model, save_dir="output")
checkpointer.save("model_999")  # save to output/model_999.pth
```

`DetectionCheckpointer` 其父类 `Checkpointer` 的 `save` 方法代码如下：

```python
    def save(self, name: str, **kwargs: Any) -> None:
        """
        Dump model and checkpointables to a file.

        Args:
            name (str): name of the file.
            kwargs (dict): extra arbitrary data to save.
        """
        if not self.save_dir or not self.save_to_disk:
            return

        data = {}
        data["model"] = self.model.state_dict()
        for key, obj in self.checkpointables.items():
            data[key] = obj.state_dict()
        data.update(kwargs)

        basename = "{}.pth".format(name)
        save_file = os.path.join(self.save_dir, basename)
        assert os.path.basename(save_file) == basename, basename
        self.logger.info("Saving checkpoint to {}".format(save_file))
        with self.path_manager.open(save_file, "wb") as f:
            torch.save(data, f)
        self.tag_last_checkpoint(basename)
```

可知 `checkpoint` 保存的 `.pth` 文件为 static_dict（模型参数）。

保存完整模型需要如下操作：

```python
torch.save(model, "output/model_999.pth")
```

## 参考资料

- [Pytorch: Saving and loading models](https://pytorch.org/tutorials/beginner/saving_loading_models.html)
- [Detectron2: Use Models](https://detectron2.readthedocs.io/en/latest/tutorials/models.html)
