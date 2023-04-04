# 使用 Python 的 Conda 环境，您只需要知道

> 原文：<https://blog.devgenius.io/using-conda-environments-for-python-all-you-need-to-know-2eb36e224d1c?source=collection_archive---------4----------------------->

## 动手为您的 python 项目创建和管理 conda 环境和 env 变量。

![](img/5cb7cefca90cdd5b3adfde2ee3587e02.png)

克里斯蒂安·阿尔索夫在 [Unsplash](https://unsplash.com/photos/wqxCjls3Hyo) 上的照片

> 本教程的所有代码都可以在[这里](https://github.com/armand-sauzay/best-practices/tree/main/tutorials/conda_environment)找到。

Python 代码很棒，但是能够复制代码就更棒了！这就是为什么所有 python 项目都应该提供一些定义包和版本的东西，以便能够运行完全相同的代码:**一个环境**。

> 那么问题来了:我应该用什么来创造我的环境？

有很多种可能。首先，我相信你们都用过以下内容:

```
pip install <package>
```

但是我们如何跟踪我们安装的软件包及其版本呢？这就是 virtualenv(本地 Python 环境管理器)可以派上用场的地方。但是当您有不同 python 版本的项目时会发生什么呢？你需要使用 pyenv(本地 python 版本管理器)来确保你使用了正确的 Python 版本。然后 virtualenv 来确保您使用正确的软件包版本…

如果有一个工具可以做到这一切，那就太好了，不是吗？这个工具就是康达。简而言之，在大多数情况下，conda 涵盖了您需要的所有东西，在幕后做了一些魔术，所以您不必担心环境，并且有一些很大的附加好处，比如允许您定义环境变量。

没有进一步的到期，让我们了解这个神奇的工具更多！

在本教程中，我们将涵盖以下内容:

1.  基本 conda 命令:创建、列表、激活和停用
2.  environment.yaml 及其使用
3.  通过 conda 定义环境变量

# 1.基本 conda 命令:创建、列表、激活和停用

如果你还没有安装康达，可以从安装`[miniconda](https://docs.conda.io/en/latest/miniconda.html)`开始。如果你不确定，打开一个终端并运行`conda list`
一旦安装完成，运行`conda list`以确保你已经安装了 conda。

* *以下命令是 bash 命令

a.创造你的环境

```
conda create --name conda-tutorial python=3.8
```

→这里我们创建一个名为`conda-tutorial`的环境，安装了 python 3.8。→ *注意:如果询问是否继续，键入 y 并输入*

b.列出现有环境

```
conda env list
```

→您应该能够看到新创建的环境

c.激活您的环境

```
conda activate conda-tutorial
```

→您现在处于 conda 虚拟环境中。所以您执行的 python 代码应该找到它的 python 版本和当前通过这个环境安装的包。

→要对此进行检查，您可以键入 terminal: `which python`。

→您也可以通过键入`conda install <package>`来安装软件包

→我们将在后面看到如何使用 environment.yaml 来避免手动安装

d.停用您的环境

```
conda deactivate
```

e.最后，如果您愿意，可以删除您的环境

```
conda env remove --name <your_environment_name>
```

→请注意，如果您希望删除当前环境，需要先将其停用

不错！您现在可以创建一个运行代码的环境，这将极大地有助于再现性！您还可以为 [conda 备忘单](https://docs.conda.io/projects/conda/en/4.6.0/_downloads/52a95608c49671267e40c689e0bc00ca/conda-cheatsheet.pdf)添加书签，以备将来使用

但是，说实话，运行大量的`conda install <package>`对于可重复性来说并不是很好。让我们看看如何使用 environment.yaml 文件。

# 2.environment.yaml 及其使用

让我们创建一个安装了 pandas 的环境文件。为此，我们需要创建两个文件:

1.  一个名为`conda_tutorial_pandas.py`的简单 python 文件，它导入 pandas 并打印其版本。

```
### conda_tutorial_pandas.py fileimport pandas as pdif __name__ == "__main__":    
    print(pd.__version__)
```

2.一个名为`environment_pandas.yaml`的文件，并在其中粘贴以下内容:

```
### environment_pandas.yaml filename: conda-tutorialchannels:
  - conda-forge
  - defaultsdependencies:
    - python=3.8
    - pandas=1.4.2
```

→这个文件将用 python 3.8 和 pandas 1.4.2 创建一个名为`conda-tutorial`的环境。

如果没有名为 conda-tutorial 的环境，可以运行

```
conda env create --file environment_pandas.yaml
```

如果您有一个名为 conda-tutorial 的环境，您可以运行

```
conda env update --file environment_pandas.yaml
```

现在，您可以激活环境`conda-tutorial`并从终端运行`python conda_tutorial_pandas.py`。它应该打印出来:`1.4.2.`

现在让我们更进一步，让我们创建环境变量，这样您就不必担心凭据被泄露等问题。

# 3.环境变量

a.创建将激活和停用环境变量的 shell 脚本。

```
cd $CONDA_PREFIX 
mkdir -p ./etc/conda/activate.d 
mkdir -p ./etc/conda/deactivate.d 
touch ./etc/conda/activate.d/env_vars.sh 
touch ./etc/conda/deactivate.d/env_vars.sh
```

b.添加以下内容，以便在激活环境时导出环境变量

```
nano $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
```

*   然后粘贴以下内容:

```
export MY_KEY='secret-key-value' 
export MY_FILE=/path/to/my/file/
```

*   然后单击 ctrl+o，再单击 ctrl+x，如果不想使用 nano，可以随意手动创建这个文件及其内容。

c.添加以下内容，以便在停用环境时取消设置环境变量

```
nano $CONDA_PREFIX/etc/conda/deactivate.d/env_vars.sh
```

*   然后粘贴以下内容

```
unset MY_KEY 
unset MY_FILE
```

*   然后点击 ctrl+o，再点击 ctrl+x，如果不想用 nano，可以手动创建这个文件及其内容。

现在让我们创建一个名为`conda_tutorial_environment_variables.py`的 python 文件，它将打印出这些环境变量

```
### conda_tutorial_environment_variables.py fileimport osif __name__ == “__main__”: 
    print(os.getenv(“MY_KEY”)) 
    print(os.getenv(“MY_FILE”))
```

现在，激活您的 conda 环境`conda activate <my_env>`，并从终端运行:

```
python conda_tutorial_environment_variables.py
```

呜哇！您现在知道了如何创建和管理 conda 环境，以及如何在您的环境中创建环境变量，因此您不必担心在代码中硬编码凭证！！

希望你喜欢这篇文章！如果您有任何问题或建议，请不要犹豫，在评论中，或者随时通过 [LinkedIn](https://www.linkedin.com/in/armand-sauzay-80a70b160/) 、 [GitHub](https://github.com/armand-sauzay) 或 [Twitter](https://twitter.com/armandsauzay) 联系我，或者查看我写的关于 DS/ML 最佳实践的一些[其他教程](https://medium.com/@armand-sauzay/list/devops-for-data-science-b9776aadb7c9)。