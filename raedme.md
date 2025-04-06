## Kaggleのdocker imageをpullします。
```
$ docker pull gcr.io/kaggle-gpu-images/python:latest
```

## imageをrunしてコンテナを実行します。
```
$ docker run -itd --gpus all -p 8888:8888 -v "$(pwd)"/target:/home --name kaggle gcr.io/kaggle-gpu-images/python /bin/bash
```

## 5.3.1 Attach
### runによってコンテナが起動したことを確認します
```
# 起動しているコンテナを表示
$ docker ps
# CONTAINER ID   IMAGE                         COMMAND                   CREATED         STATUS         PORTS                    NAMES
# a5ec778a20dd   gcr.io/kaggle-images/python   "/entrypoint.sh /bin…"   3 minutes ago   Up 3 minutes   0.0.0.0:8888->8080/tcp   kaggle
```


## 次にVSCodeでCtrl + Shift + pでコマンドパレットを開き、開発コンテナ：実行中のコンテナにアタッチ.../Dev Containers: Attach to Running Container...を選択、先ほど起動したimageを選択します。



## ちなみに抜ける時は
```
$ exit
```

## 再度コンテナを起動するときは
```
$ docker start kaggle
```

## versionを確認したら、拡張機能からpythonとjupyterをインストールし、VSCode右上のKernelから確認したversionと同じpythonを選択します。
## これで準備は完了です！Ctrl + Shift + pからCreate: New Jupyter Notebookを選択し、簡単なコードを実行してみましょう。
## またGPUを使用している場合、以下のコードでGPUが認識されているか確かめることができます。
```
import torch

if torch.cuda.is_available():
    print("CUDA is available. GPU is recognized.")
    print("Number of GPUs available: ", torch.cuda.device_count())
    print("GPU Name: ", torch.cuda.get_device_name(0))
else:
    print("CUDA is not available. No GPU is recognized.")
```