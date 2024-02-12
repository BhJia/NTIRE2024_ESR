# [NTIRE 2024 Challenge on Efficient Super-Resolution](https://cvlai.net/ntire/2024/) @ [CVPR 2024](https://cvpr.thecvf.com/)

<div align=center>
<img src="https://github.com/Amazingren/NTIRE2024_ESR/blob/main/figs/logo.png" width="400px"/> 
</div>

## About the Challenge

Jointly with NTIRE workshop we have a challenge on Efficient Super-Resolution, that is, the task of super-resolving (increasing the resolution) an input image with a magnification factor x4 based on a set of prior examples of low and corresponding high resolution images. The challenge has three tracks.

The aim is to devise a network that reduces one or several aspects such as runtime, parameters, and FLOPs of RLFN (https://arxiv.org/pdf/2205.07514.pdf), the winner of NTIRE2022 Efficient Super-Resolution Challenge, while at least maintaining a threshold PSNR on the LSDIR_DIV2K_valid (26.90 dB) datasets and LSDIR_DIV2K_test datasets (27.00 dB).

Note that for the final ranking and challenge winners we are weighing more the teams/participants improving in more than one aspect (runtime, parameters, FLOPs) over the provided reference solution.

To ensure fairness in the evaluation process, it is imperative to adhere to the following guidelines:

- **Avoid Training with Specific Image Sets:**
    Refrain from training your model using the validation LR images, validation HR images, or testing LR images. The test datasets will not be disclosed, making PSNR performance on the test datasets a crucial factor in the final evaluation.

- **PSNR Threshold and Ranking Eligibility:**
    Methods with a PSNR below the specified threshold (i.e., 26.90 dB on LSDIR_DIV2K_valid and, 27.00 dB on LSDIR_DIV2K_test) will not be considered for the subsequent ranking process. It is essential to meet the minimum PSNR requirement to be eligible for further evaluation and ranking.
## The Environments
The evaluation environments adopted by us is recorded in the `requirements.txt`. After you built your own basic Python setup via either *virtual environment* or *anaconda*, please try to keep similar to it via:

```pip install -r requirements.txt```

or take it as a reference based on your original environments.

## The Validation datasets
After downloaded all the necessary validate dataset ([LSDIR_DIV2K_valid_LR](https://drive.google.com/file/d/17bYWToyxHOTsjvSkWxLNkoUs_PYVuUc9/view?usp=sharing) and [LSDIR_DIV2K_valid_HR](https://drive.google.com/file/d/1qgjV2y47TxR6TriaGfqKj6FTSkWYdEZ8/view?usp=drive_link)), please organize them as follows:

```
|NTIRE2024_ESR_Challenge/
|--LSDIR_DIV2K_valid_HR/
|    |--000001.png
|    |--000002.png
|    |--...
|    |--000100.png
|    |--0801.png
|    |--0802.png
|    |--...
|    |--0900.png
|--LSDIR_DIV2K_valid_LR/
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

## How to test the baseline model?

1. `git clone https://github.com/Amazingren/NTIRE2024_ESR.git`
2. Select the model you would like to test from [`run.sh`](./run.sh)
    ```bash
    CUDA_VISIBLE_DEVICES=0 python test_demo.py --data_dir [path to your data dir] --save_dir [path to your save dir] --model_id -1
    ```
    - Be sure the change the directories `--data_dir` and `--save_dir`.
3. More detailed example-command can be found in `run.sh` for your convenience.

As a reference, we provide the Performances of RLFN (baseline method) below:
- Average PSNR on LSDIR_DIV2K validation data: 26.96 dB
- Average PSNR on LSDIR_DIV2K test data: 27.07 dB
- Number of parameters: 0.317M
- Runtime: 16.18 ms (LSDIR_DIV2K_valid data), 10.89 ms (LSDIR_DIV2K_test data)
- FLOPs on an LR image of size 256×256: 19.70 G

    Please note that the results reported above are the average of 5 runs, and each run is conducted on the same device (i.e., NVIDIA GeForce RTX 3090 GPU).



## How to add your model to this baseline?

1. Register your team in the [Google Spreadsheet](https://docs.google.com/spreadsheets/d/1ZFlte0uR4bNl6UVJxShESkui1n3ejzXAvUX_e1qyhSc/edit?usp=sharing) and get your team ID.
2. Put your the code of your model in `./models/[Your_Team_ID]_[Your_Model_Name].py`
   - Please add **only one** file in the folder `./models`. **Please do not add other submodules**.
   - Please zero pad [Your_Team_ID] into two digits: e.g. 00, 01, 02 
3. Put the pretrained model in `./model_zoo/[Your_Team_ID]_[Your_Model_Name].[pth or pt or ckpt]`
   - Please zero pad [Your_Team_ID] into two digits: e.g. 00, 01, 02  
4. Add your model to the model loader `./test_demo/select_model` as follows:
    ```python
        elif model_id == [Your_Team_ID]:
            # define your model and load the checkpoint
    ```
   - Note: Please set the correct data_range, either 255.0 or 1.0
5. Send us the command to download your code, e.g, 
   - `git clone [Your repository link]`
   - We will do the following steps to add your code and model checkpoint to the repository.
   

## How to calculate the number of parameters, FLOPs, and activations

```python
    from utils.model_summary import get_model_flops, get_model_activation
    from models.team00_RLFN import RLFN_Prune
    from fvcore.nn import FlopCountAnalysis

    model = RLFN_Prune()
    
    input_dim = (3, 256, 256)  # set the input dimension
    activations, num_conv = get_model_activation(model, input_dim)
    activations = activations / 10 ** 6
    print("{:>16s} : {:<.4f} [M]".format("#Activations", activations))
    print("{:>16s} : {:<d}".format("#Conv2d", num_conv))

    # The FLOPs calculation in previous NTIRE_ESR Challenge
    # flops = get_model_flops(model, input_dim, False)
    # flops = flops / 10 ** 9
    # print("{:>16s} : {:<.4f} [G]".format("FLOPs", flops))

    # fvcore is used in NTIRE2024_ESR for FLOPs calculation
    input_fake = torch.rand(1, 3, 256, 256).to(device)
    flops = FlopCountAnalysis(model, input_fake).total()
    flops = flops/10**9
    print("{:>16s} : {:<.4f} [G]".format("FLOPs", flops))

    num_parameters = sum(map(lambda x: x.numel(), model.parameters()))
    num_parameters = num_parameters / 10 ** 6
    print("{:>16s} : {:<.4f} [M]".format("#Params", num_parameters))
```

## How the Ranking Strategy Works?
After the organizers receive all the submitted codes/checkpoints/results, there are three steps are adopted:

- Step1: The organizers will execute each model five times to reevaluate all submitted methods on the same device, specifically the NVIDIA GeForce RTX 3090. The average results of these five runs will be documented for each metric.
- Step2: To ensure PSNR consistency with the baseline method RLFN, PSNR checks will be conducted for all submitted methods. Any method with a PSNR below 26.50 dB on the LSDIR_DIV2K_valid dataset or less than 26.90 on the LSDIR_DIV2K_test datasets will be excluded from the comparison list for the remaining rankings. 
- Step3: For the rest, a comparison score will be calculated as:

    *Score = 0.7 \* Score_Runtime + 0.15 \* Score_FLOPs + 0.15 \* Score_Params*
 
    and **The Lower The Better**. 

Note that in the Step3, *Score_Runtime = exp(2 * Runtime / Runtime_RLFN)*, *Score_FLOPs = exp(2 * FLOPs / FLOPs_RLFN)*, and *Score_Params = exp(2 * Params / Params_RLFN)*. 

The reference results of the baseline method RLFN (i.e., Runtime_RLFN, FLOPs_RLFN, Params_RLFN, and PSNR performance) can be found in the corresponding NTIRE2024_ESR website.


## Organizers
- Yawei Li (yawei.li@vision.ee.ethz.ch)
- Bin Ren (bin.ren@unitn.it)
- Nancy Mehta (nancy.mehta@uni-wuerzburg.de)
- Radu Timofte (Radu.Timofte@uni-wuerzburg.de) 

If you have any question, feel free to reach out the contact persons and direct managers of the NTIRE challenge.



## License and Acknowledgement
This code repository is release under [MIT License](LICENSE). 