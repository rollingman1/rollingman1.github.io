---
layout: post
title:  "Pytorch Lightning Overview"
subtitle: "파이토치 라이트닝 개요 - 공식 문서 핵심만 정리"
date:   2023-01-13 15:14:00+0900
categories: [mlops]
tags: ["Pytorch Lightning"]
---

## 1. Define Module

[https://pytorch-lightning.readthedocs.io/en/stable/starter/introduction.html#define-a-lightningmodule](https://pytorch-lightning.readthedocs.io/en/stable/starter/introduction.html#define-a-lightningmodule)

```python
import os
from torch import optim, nn, utils, Tensor
from torchvision.datasets import MNIST
from torchvision.transforms import ToTensor
import pytorch_lightning as pl

# define any number of nn.Modules (or use your current ones)
encoder = nn.Sequential(nn.Linear(28 * 28, 64), nn.ReLU(), nn.Linear(64, 3))
decoder = nn.Sequential(nn.Linear(3, 64), nn.ReLU(), nn.Linear(64, 28 * 28))

# define the LightningModule
class LitAutoEncoder(pl.LightningModule):
    def __init__(self, encoder, decoder):
        super().__init__()
        self.encoder = encoder
        self.decoder = decoder

    def training_step(self, batch, batch_idx):
        # training_step defines the train loop.
        # it is independent of forward
        x, y = batch
        x = x.view(x.size(0), -1)
        z = self.encoder(x)
        x_hat = self.decoder(z)
        loss = nn.functional.mse_loss(x_hat, x)
        # Logging to TensorBoard by default
        self.log("train_loss", loss)
        return loss

    def configure_optimizers(self):
        optimizer = optim.Adam(self.parameters(), lr=1e-3)
        return optimizer

# init the autoencoder
autoencoder = LitAutoEncoder(encoder, decoder)
```

## 2. Train

```python
# train the model (hint: here are some helpful Trainer arguments for rapid idea iteration)
trainer = pl.Trainer(limit_train_batches=100, max_epochs=1)
trainer.fit(model=autoencoder, train_dataloaders=train_loader)
```

The Lightning [Trainer](https://pytorch-lightning.readthedocs.io/en/stable/common/trainer.html) automates [40+ tricks](https://pytorch-lightning.readthedocs.io/en/stable/common/trainer.html#trainer-flags) including:

- Epoch and batch iteration
- `optimizer.step()`, `loss.backward()`, `optimizer.zero_grad()` calls
- Calling of `model.eval()`, enabling/disabling grads during evaluation
- [Checkpoint Saving and Loading](https://pytorch-lightning.readthedocs.io/en/stable/common/checkpointing.html)
- Tensorboard (see [loggers](https://pytorch-lightning.readthedocs.io/en/stable/visualize/loggers.html) options)
- [Multi-GPU](https://pytorch-lightning.readthedocs.io/en/stable/accelerators/gpu.html) support
- [TPU](https://pytorch-lightning.readthedocs.io/en/stable/accelerators/tpu.html)
- [16-bit precision AMP](https://pytorch-lightning.readthedocs.io/en/stable/guides/speed.html#speed-amp) support

- **Loop**

    [https://pytorch-lightning.readthedocs.io/en/stable/extensions/loops.html#built-in-loops](https://pytorch-lightning.readthedocs.io/en/stable/extensions/loops.html#built-in-loops)

## 3. Test

[https://pytorch-lightning.readthedocs.io/en/stable/common/evaluation_basic.html#define-the-test-loop](https://pytorch-lightning.readthedocs.io/en/stable/common/evaluation_basic.html#define-the-test-loop)

- **`Trainer.test()`** 실행

- model 객체에 `test_step()` 함수 정의(overriding)하면 **`Trainer.test()`** 실행 시 Batch 별로 `model.test_step()` 실행됨

```python
from torch.utils.data import DataLoader

# initialize the Trainer
trainer = Trainer()

# test the model
trainer.test(model, dataloaders=DataLoader(test_set))
```

## 4. Validation

[https://pytorch-lightning.readthedocs.io/en/stable/common/evaluation_basic.html#define-the-validation-loop](https://pytorch-lightning.readthedocs.io/en/stable/common/evaluation_basic.html#define-the-validation-loop)

**`Trainer.fit()`** 실행할 때 `val_dataloaders` 파라미터에 valid 데이터 셋이 포함된 DataLoader를 넘겨주면 된다.

```python
from torch.utils.data import DataLoader

train_loader = DataLoader(train_set)
valid_loader = DataLoader(valid_set)

# train with both splits
trainer = Trainer()
trainer.fit(model, train_loader, valid_loader)
```

## 5. Checkpointing

[https://pytorch-lightning.readthedocs.io/en/stable/common/checkpointing_basic.html#checkpointing-basic](https://pytorch-lightning.readthedocs.io/en/stable/common/checkpointing_basic.html#checkpointing-basic)

### 5.1. Contents of a checkpoint

A Lightning checkpoint contains a dump of the model’s entire internal state. Unlike plain PyTorch, Lightning **saves *everything* you need to restore a model** even in the most complex distributed training environments.

Inside a Lightning checkpoint you’ll find:

- 16-bit scaling factor (if using 16-bit precision training)
- Current epoch
- Global step
- LightningModule’s state_dict
- State of all optimizers
- State of all learning rate schedulers
- State of all callbacks (for stateful callbacks)
- State of datamodule (for stateful datamodules)
- The hyperparameters used for that model if passed in as hparams (Argparse.Namespace)
- The hyperparameters used for that datamodule if passed in as hparams (Argparse.Namespace)
- State of Loops (if using Fault-Tolerant training)

### 5.2. Save

```python
# saves checkpoints to 'some/path/' at every epoch end
trainer = Trainer(default_root_dir="some/path/")
```

### 5.3. Load

```python
model = MyLightningModule.load_from_checkpoint("/path/to/checkpoint.ckpt")

# disable randomness, dropout, etc...
model.eval()

# predict with the model
y_hat = model(x)
```

### 5.4. Hyper Parameter

The LightningModule allows you to automatically save all the hyperparameters passed to *init* simply by calling *self.save_hyperparameters()*.

```python
class MyLightningModule(LightningModule):
    def __init__(self, learning_rate, another_parameter, *args, **kwargs):
        super().__init__()
        self.save_hyperparameters()
```

The hyperparameters are saved to the “hyper_parameters” key in the checkpoint

```python
checkpoint = torch.load(checkpoint, map_location=lambda storage, loc: storage)
print(checkpoint["hyper_parameters"])
# {"learning_rate": the_value, "another_parameter": the_other_value}
```

The LightningModule also has access to the Hyperparameters

```python
model = MyLightningModule.load_from_checkpoint("/path/to/checkpoint.ckpt")
print(model.learning_rate)
```

### 5.5. Resume

```python
model = LitModel()
trainer = Trainer()

# automatically restores model, epoch, step, LR schedulers, apex, etc...
trainer.fit(model, ckpt_path="some/path/to/my_checkpoint.ckpt")
```

## 6. Early Stopping

[https://pytorch-lightning.readthedocs.io/en/stable/common/early_stopping.html#early-stopping](https://pytorch-lightning.readthedocs.io/en/stable/common/early_stopping.html#early-stopping)

2-ways

1. overriding `[on_train_batch_start()](https://pytorch-lightning.readthedocs.io/en/stable/api/pytorch_lightning.core.hooks.ModelHooks.html#pytorch_lightning.core.hooks.ModelHooks.on_train_batch_start)` to return `-1` when some condition is met.
2. callback (EarlyStopping Module)

```python
from pytorch_lightning.callbacks.early_stopping import EarlyStopping

class LitModel(LightningModule):
    def validation_step(self, batch, batch_idx):
        loss = ...
        self.log("val_loss", loss)

model = LitModel()
trainer = Trainer(callbacks=[EarlyStopping(monitor="val_loss", mode="min")])
trainer.fit(model)
```

The `[EarlyStopping](https://pytorch-lightning.readthedocs.io/en/stable/api/pytorch_lightning.callbacks.EarlyStopping.html#pytorch_lightning.callbacks.EarlyStopping)` callback runs at the end of every validation epoch by default.

```python
class MyEarlyStopping(EarlyStopping):
    def on_validation_end(self, trainer, pl_module):
        # override this to disable early stopping at the end of val loop
        pass

    def on_train_end(self, trainer, pl_module):
        # instead, do it at the end of training loop
        self._run_early_stopping_check(trainer)
```

## 7. Tutorial & Examples

[https://pytorch-lightning.readthedocs.io/en/stable/tutorials.html](https://pytorch-lightning.readthedocs.io/en/stable/tutorials.html)
