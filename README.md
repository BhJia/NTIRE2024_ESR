# [NTIRE 2024 Challenge on Efficient Super-Resolution](https://cvlai.net/ntire/2024/) @ [CVPR 2024](https://cvpr.thecvf.com/)

<div align=center>
<img src="https://github.com/Amazingren/NTIRE2024_ESR/blob/main/figs/logo.png" width="400px"/> 
</div>


# IFADNet: Intermittent Feature Aggregation with Distillation for Efficient Super-resolution

Source code, pretrained model and fact sheet of our solution to NTIRE 2024 Efficient Super-resolution. More details can be found in the official repository of the challenge `https://github.com/Amazingren/NTIRE2024_ESR`




## The Environments

The evaluation environments adopted by us is recorded in the `requirements.txt`. After you built your own basic Python setup via either *virtual environment* or *anaconda*, please try to keep similar to it via:

```pip install -r requirements.txt```

or take it as a reference based on your original environments.



## The Validation datasets
After downloaded all the necessary validate dataset ([DIV2K_LSDIR_valid_LR](https://drive.google.com/file/d/1YUDrjUSMhhdx1s-O0I1qPa_HjW-S34Yj/view?usp=sharing) and [DIV2K_LSDIR_valid_HR](https://drive.google.com/file/d/1z1UtfewPatuPVTeAAzeTjhEGk4dg2i8v/view?usp=sharing)), please organize them as follows:

```
|NTIRE2024_ESR_Challenge/
|--DIV2K_LSDIR_valid_HR/
|    |--000001.png
|    |--000002.png
|    |--...
|    |--000100.png
|    |--0801.png
|    |--0802.png
|    |--...
|    |--0900.png
|--DIV2K_LSDIR_valid_LR/
|    |--000001x4.png
|    |--000002x4.png
|    |--...
|    |--000100x4.png
|    |--0801x4.png
|    |--0802x4.png
|    |--...
|    |--0900.png
|--NTIRE2024_ESR/
|    |--...
|    |--test_demo.py
|    |--...
|--results/
|--......
```

## How to test the model?

1. `https://github.com/BhJia/NTIRE2024_ESR`
2. Select the model you would like to test from [`run.sh`](./run.sh)
    ```bash
    CUDA_VISIBLE_DEVICES=0 python test_demo.py --data_dir [path to your data dir] --save_dir [path to your save dir] --model_id 33
    ```
    - Be sure the change the directories `--data_dir` and `--save_dir`.
3. More detailed example-command can be found in `run.sh` for your convenience.



## About Fact Sheet

All fact sheet files are in folder `Fact Sheet`.

`team33_Fact_Sheet.pdf` is the compiled pdf file of our fact sheet.  `team33_Fact_Sheet.zip` is the corresponding .tex source files. Important files in zip file are `EfficientSR_factsheet.tex` which is .tex source file. `egbib.bib` is the BibTex form reference file. `images.pdf` and `images1.pdf` are source images of the fact sheet. You can select zip file and upload it to a common LaTeX editor to check the details.




## License and Acknowledgement
This code repository is release under [MIT License](LICENSE). 