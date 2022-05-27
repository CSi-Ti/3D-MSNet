# 3D-MSNet: A point cloud based deep learning model for untargeted feature detection and quantification in profile LC-HRMS data

If you meet any problem in running or training 3D-MSNet, feel free to contact ***wangruimin@westlake.edu.cn***


## Highlights
- **Novelty:** 3D-MSNet achieves lossless high-dimensional feature detection and quantification, by considering LC-MS feature detection problem as a 3D point cloud instance segmentation problem.
- **Accuracy:** 3D-MSNet achieved the best accuracy in terms of feature detection and quantification on the test datasets. 
- **Applicability:** 3D-MSNet can be widely applied to various MS systems and acquisition methods. 
- **Efficiency:** 3D-MSNet spends reasonable analysis time by accelerating with GPU, which is similar to traditional methods and about 5 times faster than other deep learning methods. 
- **Potentiality:** 3D-MSNet can obtain better accuracy on bigger annotated training datasets.


## Environment
##### Recommended
Intel(R)_Core(TM)_i9-10900K CPU, 128GB memory, GeForce RTX 3090 GPU

Ubuntu 16.04 + CUDA 11.1 + cuDNN 8.0.5

Anaconda 4.9.2 + Python 3.6.13 + PyTorch 1.9

## Setup
1. Prepare the deep-learning environment based on your system and hardware, 
   including GPU driver, CUDA, cuDNN, Anaconda, Python, and PyTorch.
   
2. Install the dependencies. Here we use ROOT_PATH to represent the root path of 3D-MSNet.
    
    ```cd ROOT_PATH```
   
    ```pip install -r requirements.txt```
        
3. Compile CUDA code. This will take a few minutes.
   
    ```cd cuda```
   
    ```python setup.py install```


## Datasets
The 3DMS dataset and all the benchmark datasets (mzML format) can be freely downloaded at [Zenodo](https://zenodo.org/record/6582912).

Raw MS files of the metabolomics datasets can be downloaded at [Google Drive](https://drive.google.com/drive/folders/1PRDIvihGFgkmErp2fWe41UR2Qs2VY_5G).

Raw MS files of the proteomics datasets can be downloaded at ProteomeXchange (dataset [PXD001091](http://proteomecentral.proteomexchange.org/cgi/GetDataset?ID=PXD001091)).

Targeted annotation results, evaluation results and evaluation methods can be downloaded at [Zenodo](https://zenodo.org/record/6582912).

## Run 3D-MSNet
### Demos
Our demos can help you reproduce the evaluation results.

Place the benchmark datasets as follows.
```
3D-MSNet-master
├── dataset
│   ├── TripleTOF_6600
│   │   ├── mzml
│   │   │   ├── *.mzML

│   ├── QE_HF
│   │   ├── mzml
│   │   │   ├── *.mzML

│   ├── Orbitrap_XL
│   │   ├── mzml
│   │   │   ├── *.mzML
```
Then run scripts in folder DEMO. For example:

```cd ROOT_PATH```

Prepare point clouds: ```python DEMO/TripleTOF_6600_untarget/0_pc_extraction.py```

Extract features: ```python DEMO/TripleTOF_6600_untarget/1_peak_detection.py```

The result files are saved in the dataset folder.

### Customized running

Refer to DEMO for parameter setting of different LC-MS platforms.

```cd ROOT_PATH```

Prepare point clouds:

```python workflow/predict/point_cloud_extractor.py --data_dir=PATH_TO_MZML --output_dir=POINT_CLOUD_PATH --window_mz_width=0.8 --window_rt_width=6 --min_intensity=128 --from_mz=0 --to_mz=2000 --from_rt=0 --to_rt=300 --expansion_mz_width=0.1 --expansion_rt_width=1```

Extract features:

```python workflow/predict/main_eval.py --data_dir=POINT_CLOUD_PATH --mass_analyzer=orbitrap --mz_resolution=60000 --resolution_mz=400 --rt_fwhm=0.1 --target_id=None```

Run ```python workflow/predict/point_cloud_extractor.py -h``` and ```python workflow/predict/main_eval.py -h``` to learn parameter details.

## Train 
We provided a pretrained model in ```experiment``` folder.

If you want to train the model on your self-annotated data, prepare your .csv files refer to the 3DMS dataset.
Each MS signal should be annotated an instance label.

Place the training dataset as follows.
```
3D-MSNet-master
├── dataset
│   ├── your_training_dataset
│   │   ├── dataset_anno
│   │   │   ├── [id_mz_rt].csv
```

Then change the training parameters at ```config/msnet_default.yaml```

```cd ROOT_PATH```

Split training set and validation set:

```python workflow/train/dataset_generator.py```

Start training:

```python workflow/train/main_train.py```

Trained models are saved in ```experiment``` folder.

## Citation

Cite our paper at:
```
@article{}
```

## License

3D-MSNet is an open-source tool, using [***Mulan Permissive Software License，Version 2 (Mulan PSL v2)***](http://license.coscl.org.cn/MulanPSL2)

