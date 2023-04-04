# 主动学习。

> 原文：<https://blog.devgenius.io/mlflow-with-active-learning-b804c49e978b?source=collection_archive---------6----------------------->

## 重要提示。

本文的目的不是太深入地解释什么是 ml flow/主动学习，而是了解如何实际使用它，因此本文期望人们对**ml flow/主动学习**有所了解。

## MLflow

[MLflow](https://www.mlflow.org/) 是一个管理端到端机器学习生命周期的开源平台。它有以下主要组件:

*   **跟踪**:允许您跟踪实验，记录和比较参数和结果。
*   **Models** :允许您从各种 ML 库中管理和部署模型到各种模型服务和推理平台。
*   **项目**:允许您将 ML 代码打包成可重用、可复制的形式，以便与其他数据科学家共享或转移到生产中。
*   **Model****Registry**:允许您集中管理模型的整个生命周期阶段转换的模型存储:从阶段转换到生产，具有版本控制和注释的能力。
*   **模型** **服务**:允许您托管 MLflow 模型作为 REST 端点。

## 主动学习。

主动学习是机器学习的一种特殊情况，在这种情况下，学习算法可以交互式地询问用户(或一些其他信息源)以用期望的输出来标记新的数据点。有些情况下，未标记的数据很丰富，但手动标记的成本很高。在这种情况下，学习算法可以主动向用户/教师查询标签。这种类型的迭代监督学习被称为主动学习。由于学习者选择例子，学习一个概念的例子的数量通常比正常的监督学习所需的数量少得多。在本文中，我将介绍一个简单的使用 MLflow 进行主动学习的例子。

## 实施细节。

在这个实验中，我使用主动学习结合 HuggingFace 模型训练**情感分析任务**，使用 MLFlow 进行**实验跟踪**、**模型**、**代码** **版本化**。我还通过创建不同的文件，如 **MLproject** 、 **conda.yaml** ，使用 MLFlow 展示了**代码可再现性**。

**数据集详细信息。**

1.  我使用了取自 Kaggle 的情感分析 IMDB 电影评论数据集，该数据集可从[https://www . ka ggle . com/code/Lakshmi 25 npathi/perspective-Analysis-of-IMDB-Movie-reviews](https://www.kaggle.com/code/lakshmi25npathi/sentiment-analysis-of-imdb-movie-reviews)下载

**预处理。**

```
from configs import CONFIG
import pandas as pd
import re 
import os 
import mlflow
from tqdm import tqdm 
import pickle
import sys
import string 
from sklearn.model_selection import StratifiedKFold

class prepDataset:
    def __init__(self) -> None:
        self.df = pd.read_csv(CONFIG.TRAIN_FILE)
        self.index2word = {idx: word for idx, word in enumerate(set(self.df["sentiment"].values))}
        self.word2index = {word: idx for idx, word in self.index2word.items()}
        self.df["label"] = self.df["sentiment"].apply(lambda x: self.word2index[x])
        arg1, arg2 = sys.argv[1], sys.argv[2]
        self.dumpData(self.index2word,arg1.split("src/")[-1])
        self.dumpData(self.word2index, arg2.split("src/")[-1])
        self.threshold = 256
        self.df["preprocessedREVIEW"]  = self.df["review"].apply(self.preprocessing)

        self.stratkFold = StratifiedKFold(n_splits=4)
        self.splitData()

    def splitData(self):
        X = self.df["preprocessedREVIEW"]
        Y = self.df["label"]
        for idx, (train_idx, test_idx) in enumerate(self.stratkFold.split(X,Y)):
            # let's create folder
            print(f"--------------------------------------- FOLD_{idx} ---------------------------------------")
            path = os.path.join(CONFIG.MULTIFOLD, f"fold_{idx}")
            if not os.path.isdir(path):
                os.mkdir(path)
            # let's save the train and test data inside the corresponding fold 
            train = self.df.iloc[train_idx]
            test  = self.df.iloc[test_idx]
            train.to_csv(os.path.join(path, "train.csv"), index=False)
            test.to_csv(os.path.join(path, "test.csv"), index=False)

    @staticmethod
    def dumpData(dictionary, filename):
        if not os.path.isdir(CONFIG.WORDINDEX):
            os.mkdir(CONFIG.WORDINDEX)
        path = os.path.join(CONFIG.WORDINDEX, filename)
        pickle.dump(dictionary, open(path, "wb"))

    @staticmethod
    def loadData(filename):
        path = os.path.join(CONFIG.WORDINDEX, filename)
        return pickle.load(open(path, "rb"))

    def preprocessing(self,sentence):
        sentence = sentence.translate(str.maketrans("","", string.punctuation)).lower()
        #let's remove if their any links 
        sentence = re.sub(r"https?://\s+", "", sentence)
        sentence = re.sub(r"\b\d+\b",  "", sentence)
        sentence = re.sub(r" +"," ",sentence)
        return sentence

prepDataset()
```

**prepDataset** 类负责预处理 IMDB 数据集，去除所有的*链接*、*数字、*、*多个空格、标点*。它还创建了 *4 个折叠 CV 数据集*，该数据集存储在 4 个不同的折叠中，所有折叠用于训练模型，并考虑表现最佳的折叠。这个类包含两个静态方法，负责保存 pickle 文件，这些文件存储了与标签的键值对相关的信息。

**主动学习类。**

```
import random
import torch
import pandas as pd
class ACLearning:
    def __init__(self, dataset) -> None:
        self.unlabelledDataset = dataset
        self.labelledDataset = None

    def randomSample(self, k = 200):
        labelledIndexes= random.sample(range(len(self.unlabelledDataset)), k=200)
        unlabelledIndexes = range(len(self.unlabelledDataset))
        # let's remove the labelled Indexes from unlabelled Indexes
        unlabelledIndexes = list(filter(lambda x: x not in labelledIndexes, unlabelledIndexes))

        self.labelledDataset = self.unlabelledDataset.iloc[labelledIndexes]
        self.unlabelledDataset= self.unlabelledDataset.iloc[unlabelledIndexes]

        self.labelledDataset.reset_index(inplace=True, drop=True)
        self.unlabelledDataset.reset_index(inplace=True, drop=True)

        # self.labelledDataset = torch.utils.data.Subset(self.unlabelledDataset, labelledIndexes)
        # self.unlabelledDataset= torch.utils.data.Subset(self.unlabelledDataset, unlabelledIndexes)
        # now we have the labelled dataset we can substract the labelled datset from unlabelled one

    def getlabelledDataset(self, labelledIndexes):
        unlabelledIndexes = range(len(self.unlabelledDataset))
        unlabelledIndexes = list(filter(lambda x: x not in labelledIndexes, unlabelledIndexes))

        self.labelledDataset = pd.concat([self.labelledDataset, self.unlabelledDataset.iloc[labelledIndexes]])
        self.unlabelledDataset = self.unlabelledDataset.iloc[unlabelledIndexes]

        self.labelledDataset.reset_index(inplace=True, drop=True)
        self.unlabelledDataset.reset_index(inplace=True, drop=True)

        # self.labelledDataset = torch.utils.data.ConcatDataset([self.labelledDataset, torch.utils.data.Subset(self.unlabelledDataset, labelledIndexes)])
        # self.unlabelledDataset= torch.utils.data.Subset(self.unlabelledDataset, unlabelledIndexes)

    @property
    def get_unlabelled_dataset(self):
        return len(self.unlabelledDataset)

    @property
    def get_labelled_dataset(self):
        return 0 if self.labelledDataset is None else len(self.labelledDataset) 
```

**ACLearning** 类是主动学习的大脑，这个类背后的整个思想是管理**已标记和未标记的数据集。**在这一类中最重要的两种方法是:

1.  **randomSample :** 它负责最初创建一个带标签的数据集并将其从一个无标签的数据集中移除，它需要传递一个参数 **K** ，该参数告诉**随机算法**要从无标签的数据集中抽取多少个样本。
2.  **getlabelledDataset:** 这个负责把**标签索引**作为实参。标记为**的索引**是使用主动学习的抽样策略计算的，我们将在本文后面看到。已标记的**索引**随后用于移动与未标记数据集中的索引相对应的数据点，并将其与已标记数据集中的数据点相结合。

**主动学习策略**

在这种情况下，我只展示了一种策略，即**熵采样、**但是可以使用不同的策略，如多样性采样、不确定性采样或两者一起使用。我们可以简单地在下面显示的类中添加另一个策略。

```
import torch
import itertools
from tqdm import tqdm
from customDataset import dataset
class strategies:
    def __init__(self) -> None:
        pass

    def predict_proba(self, dataloader, rows):
        self.model.eval()
        batchIDs = []
        data = torch.ones([rows, 2])
        start = 0
        counter =0 
        with torch.no_grad():
          for  batch_idx, element in tqdm(dataloader,position=0, leave=True):
              out = self.model(**element)
              pred = torch.softmax(out.logits, dim=-1)
              end = start + element["input_ids"].shape[0]
              data[start:end] = pred
              start = end
              batchIDs.append(batch_idx.tolist())
              counter +=1
        return data, batchIDs

    def entropySampling(self, ac_learning, model, accelerator, tokenizer):
        self.model =model
        unlabelledDataset = dataset(ac_learning.unlabelledDataset["preprocessedREVIEW"], ac_learning.unlabelledDataset["label"], tokenizer)
        dataloader = torch.utils.data.DataLoader(unlabelledDataset, batch_size=32,shuffle=False)
        self.model, dataloader = accelerator.prepare(self.model, dataloader)

        probs, batchIDs =  self.predict_proba(dataloader, len(unlabelledDataset))
        log_prob = torch.log(probs)
        batchIDs = list(itertools.chain(*batchIDs))
        return (-probs * log_prob).sum(1), batchIDs

    def entropySamplignDropout(self):
        pass
```

在这个类中， **entropySampling** 方法接受输入(ac_learning 是 **ACLearning** 类的一个对象，**模型**在带标签的数据集上训练，**加速器**是 huggingFace 库，而 Tokenizer 我们将在接下来的 Tokenizer 中深入讨论)。 **entropySampling** 方法负责返回所有未标记数据集的概率及其批量 IDX。我们只根据概率从高到低挑选排名前 k 的指数。一旦顶部 k 索引被选择，它被传递到上面讨论的 **getlabelledDataset** 方法。

## **流量测井信息**

MLflow 是一个 MLops 框架，有助于代码版本控制、实验跟踪、再现性、模型注册等。MLflow 最棒的地方在于它易于使用，并且可以很容易地与代码集成。

MLflow 提供了记录信息的不同方式:

1.  **mlflow.log_params** →这用于记录超参数。如果我们使用随机搜索、网格搜索或贝叶斯搜索，我们可以使用它将参数记录到 MLflow UI 服务器中，进行比较，以选择最佳参数。
2.  **mlflow.log_metrics** →在 mlflow 中，我们可以记录不同的指标，例如(f1_score、recall_score、precision_score 等)。
3.  **mlflow.log_artifacts** →记录工件对于代码、数据和模型的版本控制很重要。MLflow 提供了记录这些信息的方法。我们也可以使用 **mlflow.log_artifact** ，只要我们需要传递文件而不是文件夹。
4.  **mlflow.sklearn.model** →一旦我们训练了模型，我们需要将模型信息记录到 mlflow 中，如果我们正在使用 sklearn 模型，我们可以使用 MLflow 提供的方式简单地记录。
5.  **ml flow . py torch . autolog**→这是用于在我们使用 PyTorch lightning ( `pl.LightningModule`)的情况下进行自动记录。
6.  **ml flow . pytorch . log _ model**→用于记录 py torch 的型号(nn。模块)。
7.  **ml fow . py torch . load _ model**→用于从不同的 URI (S3、azure、file 等)加载模型。

我们使用 **MLflow** 记录的所有信息都可以在 **MLflow UI 中查看。**如果我们使用的是本地机器，我们可以通过简单的命令来访问它:

> mlflow ui

如果您使用 **google colab** 访问 **MLflow 的 **UI** 界面，**您必须使用 **ngrok** 来托管本地 URL。使用下面的代码可以做到这一点:

```
from pyngrok import ngrok
import mlflow

get_ipython().system_raw("mlflow ui --port 5000 &")
# Terminate open tunnels if exist
ngrok.kill()

# Setting the authtoken (optional)
# Get your authtoken from https://dashboard.ngrok.com/auth
auth = getpass('Authentication Token:')
ngrok.set_auth_token(auth)

# Open an HTTPs tunnel on port 5000 for http://localhost:5000
ngrok_tunnel = ngrok.connect(addr="5000", proto="http", bind_tls=True)
print("MLflow Tracking UI:", ngrok_tunnel.public_url)
```

传递身份验证令牌。

**使用 MLflow 进行模型注册。**

MLflow 模型注册组件是一个集中式模型存储、一组 API 和 UI，用于协作管理 MLflow 模型的整个生命周期。它提供了模型沿袭(MLflow 实验和运行产生了模型)、模型版本化、阶段转换(例如从阶段转换到生产)和注释。我们可以从 MLFlow 提供的用户界面或代码本身创建模型注册表，如 ml flow[https://www.mlflow.org/docs/latest/model-registry.html](https://www.mlflow.org/docs/latest/model-registry.html)中所述。

**ml 项目描述。**

如果我们想要执行代码再现性，此文件非常重要。一旦创建了此 MLProject 文件和 Conda.yml 文件，我们就可以运行远程服务器中的代码，甚至无需下载代码，我们只需更改参数并执行 CLI **MLflow run 命令**。

```
name: text-classification
conda_env: conda.yaml
entry_points:
  preprocessing_classification:
    parameters:
      index2word: {type: str, default="index2word.pickle"}
      word2index: {type: str, default="word2index.pickle"}
    command: "python preprocessing.py {index2word} {word2index}"
  training_classification:
    parameters:
      grad_accumulation: {type: int, default: 1}
      budget: {type: int, default: 200}
      trainFile: {type: str, default: "dataset/"}
      testFile: {type: str, default: "dataset/"}
      batch: {type: int, default: 4}
      lr: {type: float, default:1e-5}
```

1.  **名称**:定义文件的名称，可以是任何名称。
2.  **conda_env** :需要指定 conda.yaml 文件，该文件包含我们需要安装的所有库的相关信息。
3.  **entry _ points***:entry point 是入口点，例如如果我们想要执行多个文件，如 *modeltraining* 、*预处理*、*度量、*等。我们需要有多个入口点，通过这些入口点 **mflow 运行**命令，以了解它需要为特定文件运行什么命令。*
4.  ***参数** :包含我们传递给训练/预处理/度量文件等的参数。*
5.  ***命令**:该命令按照我们指定的参数执行文件。*

***Conda.yaml 文件描述***

*该文件负责存储与所有依赖项相关的信息。*

```
*name: text_classification
channels:
  - conda-forge
dependencies:
  - python=3.7
  - pip
  - pip:
    - mlflow==1.30.0
    - scipy==1.7.3
    - scikit-learn==1.0.2
    - torch==1.12.1+cu113
    - sentencepiece!=0.1.92
    - transformers==4.16.2
    - datasets>=1.8.0
    - seqeval==1.2.2
    - accelerate*
```

1.  *conda.yaml 文件由文件的**名**组成。*
2.  *然后我们指定**通道**和**依赖关系**，它们基本上定义了 python、pip 和不同重要包的版本。*
3.  *最后一个是所有文件的 **pip 安装**，在运行时安装。*

## ***模型/标记器信息。***

*我正在使用 HuggingFace 模型，tokenizer 作为 **roberta-base** 到训练语义分析任务。HuggingFace 模型上没有添加新层。我已经使用 HuggingFace 的 Auto 类来加载模型和记号赋予器，它们是:*

1.  *自动 Tokenizer。*
2.  *AutoModelForSequenceClassification。*

## ***使用 MLflow &主动学习进行培训。***

```
*from configs import CONFIG
import os 
import pandas as pd
from tqdm import tqdm
from customDataset import dataset
from ActiveLearning import ACLearning
import torch
import pprint
import mlflow 
import numpy as np
import math
from strategies import strategies
from accelerate import Accelerator
from torch.utils.data import DataLoader
from sklearn.metrics import f1_score, precision_score, recall_score
from transformers import AutoModelForSequenceClassification, AdamW, get_scheduler, AutoTokenizer
import argparse
def metric(pred, gt):
    return f1_score(pred, gt, average="weighted", zero_division=1), precision_score(pred, gt,  average="weighted", zero_division=1), recall_score(pred, gt,  average="weighted", zero_division=1)

def main():
  with mlflow.start_run():
    parser = argparse.ArgumentParser(
        description="Make text classification dataset")
    parser.add_argument("--grad_accumulation", help="output_dir")
    parser.add_argument("--budget", help="Enter budget")
    parser.add_argument("--trainFile", help="Train path")
    parser.add_argument("--testFile", help="evaluation path")
    parser.add_argument("--batch", help="Enter Batch Size.")
    parser.add_argument("--lr", help="learning rate")

    args = parser.parse_args()

    mlflow.log_params(vars(args))
    # training on all folds
    strat = strategies()
    fold= args.trainFile.split("datasets/")[-1].split("/")[0]
    mlflow.log_artifact(args.trainFile)
    mlflow.log_artifact(args.testFile)
    gradient_accumulation_steps = int(args.grad_accumulation)
    accelerator =  Accelerator(gradient_accumulation_steps=gradient_accumulation_steps)
    # folder_path = os.path.join(CONFIG.MULTIFOLD, f"fold_{i}")
    # trainFile = os.path.join(folder_path, "train.csv")
    # testFile  = os.path.join(folder_path, "test.csv")

    trainData = pd.read_csv(args.trainFile)[["preprocessedREVIEW","label"]]
    testData  = pd.read_csv(args.testFile)[["preprocessedREVIEW", "label"]]
    model = AutoModelForSequenceClassification.from_pretrained("roberta-base", num_labels =len(trainData["label"].unique().tolist()) )
    tokenizer = AutoTokenizer.from_pretrained("roberta-base")

    ac_learning = ACLearning(trainData)
    ac_learning.randomSample()

    # train and test dataset 
    traindataset = dataset(ac_learning.labelledDataset["preprocessedREVIEW"], ac_learning.labelledDataset["label"],tokenizer)
    testdataset  = dataset(testData["preprocessedREVIEW"], testData["label"], tokenizer)

    trainloader = DataLoader(traindataset, batch_size=int(args.batch), shuffle=True)
    testloader = DataLoader(testdataset, batch_size=int(args.batch), shuffle=False)

    #optimizer 

    no_decay = ["bias", "LayerNorm.weight"]
    optimizer_grouped_parameters = [
        {
            "params": [p for n, p in model.named_parameters() if not any(nd in n for nd in no_decay)],
            "weight_decay": 0.0,
        },
        {
            "params": [p for n, p in model.named_parameters() if any(nd in n for nd in no_decay)],
            "weight_decay": 0.0,
        },
    ]
    num_train_epochs = 100
    #num_update_steps_per_epoch = math.ceil(len(trainloader) / gradient_accumulation_steps)
    optimizer = torch.optim.AdamW(optimizer_grouped_parameters, lr=float(args.lr))
    max_train_steps = num_train_epochs * len(trainloader)
    lr_scheduler = get_scheduler(
        name="linear",
        optimizer=optimizer,
        num_warmup_steps=10,
        num_training_steps=max_train_steps,
    )
    trainloader, testloader, model, optimizer, lr_scheduler = accelerator.prepare(trainloader, testloader, model, optimizer, lr_scheduler)

    for epc in range(num_train_epochs):
        print(f"############## Labelled Dataset: {ac_learning.get_labelled_dataset} && Unlabelled Dataset: {ac_learning.get_unlabelled_dataset} ##############")
        model.train()
        for data in tqdm(trainloader,position=0, leave=True):
            with accelerator.accumulate(model):
                optimizer.zero_grad()
                batch_idx, inputs = data
                output = model(**inputs)
                loss = output.loss
                accelerator.backward(loss)
                optimizer.step()
                lr_scheduler.step()

        # we can perform testing here

        model.eval()
        f1_scores = []
        precisions = []
        recalls = []
        for data in tqdm(testloader,position=0, leave=True):
            batch_idx, inputs = data
            output = model(**inputs)
            logits = output.logits
            probab = torch.softmax(logits, dim=-1)
            pred = torch.argmax(probab, -1).tolist()
            gt   = inputs["labels"].tolist()
            f1Score, precision, recall = metric(pred, gt)
            f1_scores.append(f1Score)
            precisions.append(precision)
            recalls.append(recall)
        scores = {
            "f1_score": np.mean(np.array(f1Score)),
            "precision": np.mean(np.array(precisions)),
            "recall": np.mean(np.array(recalls))
        }
        print("\n\n")
        pprint.pprint(scores)
        print("\n\n")
        mlflow.log_metrics(scores) # logging the metrics 
        # perform the ActiveLearning pipeline 

        probabilities, batchIds = strat.entropySampling(ac_learning, model, accelerator, tokenizer)
        assert len(probabilities.tolist()) == len(batchIds)
        probIndex = list(zip( probabilities.tolist(),batchIds))
        ot = sorted(probIndex, key = lambda x: x[0], reverse=True)
        indexes = [ind for score, ind in ot]
        # now use these indxes to extract next sample from unlabelled dataset
        ac_learning.getlabelledDataset(indexes[:int(args.budget)])
        traindataset = dataset(ac_learning.labelledDataset["preprocessedREVIEW"], ac_learning.labelledDataset["label"], tokenizer)
        trainloader = DataLoader(traindataset, batch_size=int(args.batch), shuffle=True) # create train loader again
        trainloader = accelerator.prepare(trainloader)
          # logging the model 
    mlflow.pytorch.log_model(model, f"model-{fold}")
  mlflow.end_run()
if __name__ == "__main__":
    main()*
```

*这是**主**方法，结合了**主动学习、MLflow** 以及 HuggingFace 模型。如果我们仔细观察代码，我们会发现 **MLflow** 用于记录模型、度量、工件等信息。在每个时期使用主动学习来将更多未标记的数据添加到已标记的数据集中，并重新训练模型。为了运行上述**训练&预处理**文件，我们使用 MLflow **CLI** 命令行指令。*

*下面单元格中的命令用于运行预处理/模型训练文件。*

1.  *它需要传递**实验**名称，该名称将在 **MLflow UI 服务器中创建一个新实验。***
2.  *我们传递我们在上面描述的 **MLproject** 文件中定义的入口点名称。*
3.  ***— env-manager** 需要通过我们代码所在的本地/conda/虚拟环境。*
4.  ***-P** 表示参数，确保参数与 **MLproject** 文件中定义的参数相同，并且顺序相同。*

```
*# This one is for running the Preprocessing file.
%%shell 
mlflow run --experiment-name prepData -b local \
--entry-point preprocessing_classification --env-manager local --run-name preprocessText \
-P index2word="index2word.pickle" \
-P word2index="word2index.pickle" \
src

%%shell
for i in {0..4}
do
  mlflow run --experiment-name TrainingModel -b local \
  --entry-point training_classification --env-manager local --run-name fold_$i \
  -P grad_accumulation=1 \
  -P budget=200 \
  -P trainFile="datasets/fold_$i/train.csv" \
  -P testFile="datasets/fold_$i/test.csv" \
  -P batch=4 \
  -P lr=1e-5 \
  src
done*
```

**访问 github 获取完整代码:*[https://github.com/Anurich/Active-Learning-With-MLflow](https://github.com/Anurich/Active-Learning-With-MLflow)*

# *结论。*

*在本文中，主要讨论如何将主动学习与 MLflow 结合起来，以及如何使用 MLflow 来记录信息。我们还研究了使用 MLflow 记录信息的不同方式，以及主动学习如何帮助我们从未标记的数据集中选择最佳样本，并将其与标记的数据集相结合。我们还讨论了如何创建 MLProject 和 Conda.yaml 文件来实现代码再现性。*