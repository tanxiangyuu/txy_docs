# pytorch learning

## 1. nn模块

### 1. nn.functional()与 nn.mudule()

1. nn.functional中的函数与nn.Module()的区别是：
   - nn.Module实现的层（layer）是一个特殊的类，都是由class Layer(nn.Module)定义，会自动提取可学习的参数
   - nn.functional中的函数更像是纯函数,由def functional(input）定义

> 如果模型有可学习的参数时，最好使用nn.Module；否则既可以使用nn.functional也可以使用nn.Module，二者在性能上没有太大差异，具体的使用方式取决于个人喜好。由于激活函数（ReLu、sigmoid、Tanh）、池化（MaxPool）等层没有可学习的参数，可以使用对应的functional函数，而卷积、全连接等有可学习参数的网络建议使用nn.Module。
>
> 注意：
>
> 虽然dropout没有可学习参数，但建议还是使用nn.Dropout而不是nn.functional.dropout，因为dropout在训练和测试两个阶段的行为有所差别，使用nn.Module对象能够通过model.eval操作加以区分。