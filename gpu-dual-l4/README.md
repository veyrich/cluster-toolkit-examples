# Basic GPU deployment

Blueprint to deploy a very simple GPU cluster with a single partition providing a single compute node equipped with two Nvidia L4 GPUs. To test this example make sure to run sbatch / srun with the --gres option as follows:

`srun --gres=gpu nvidia-smi`

Or

`srun --gres=gpu:2 nvidia-smi`

(see below for example output)


Note that according to the [documentation](https://github.com/GoogleCloudPlatform/cluster-toolkit/blob/main/examples/README.md):

"Users only need to provide machine type for standard ["a2", "a3" and "g2"] machine families, while the other settings like type, count , gpu_driver_installation_config will default to machine family specific values."

So one can omit specifying parameters like the below for basic use cases (but see the above doc link for more advanced configuration parameters)

    guest_accelerator:
          - type: nvidia-l4
            count: 2

The resulting gres.conf (/usr/local/etc/slurm/gres.conf) will look similar to the following:

    # Warning:
    # This file is managed by a script. Manual modifications will be overwritten.
    NodeName=g2-nsg2spot-0 Name=gpu File=/dev/nvidia[0-1]

Example output:

Single GPU

    srun --gres=gpu nvidia-smi
    Thu Apr 24 00:16:20 2025       
    +-----------------------------------------------------------------------------------------+
    | NVIDIA-SMI 550.90.12              Driver Version: 550.90.12      CUDA Version: 12.4     |
    |-----------------------------------------+------------------------+----------------------+
    | GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
    |                                         |                        |               MIG M. |
    |=========================================+========================+======================|
    |   0  NVIDIA L4                      On  |   00000000:00:03.0 Off |                    0 |
    | N/A   58C    P8             13W /   72W |       1MiB /  23034MiB |      0%      Default |
    |                                         |                        |                  N/A |
    +-----------------------------------------+------------------------+----------------------+
                                                                                             
    +-----------------------------------------------------------------------------------------+
    | Processes:                                                                              |
    |  GPU   GI   CI        PID   Type   Process name                              GPU Memory |
    |        ID   ID                                                               Usage      |
    |=========================================================================================|
    |  No running processes found                                                             |
    +-----------------------------------------------------------------------------------------+

Dual GPU

    srun --gres=gpu:2 nvidia-smi
    Thu Apr 24 00:16:54 2025       
    +-----------------------------------------------------------------------------------------+
    | NVIDIA-SMI 550.90.12              Driver Version: 550.90.12      CUDA Version: 12.4     |
    |-----------------------------------------+------------------------+----------------------+
    | GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
    |                                         |                        |               MIG M. |
    |=========================================+========================+======================|
    |   0  NVIDIA L4                      On  |   00000000:00:03.0 Off |                    0 |
    | N/A   57C    P0             24W /   72W |       1MiB /  23034MiB |      0%      Default |
    |                                         |                        |                  N/A |
    +-----------------------------------------+------------------------+----------------------+
    |   1  NVIDIA L4                      On  |   00000000:00:04.0 Off |                    0 |
    | N/A   60C    P8             14W /   72W |       1MiB /  23034MiB |      0%      Default |
    |                                         |                        |                  N/A |
    +-----------------------------------------+------------------------+----------------------+
                                                                                         
    +-----------------------------------------------------------------------------------------+
    | Processes:                                                                              |
    |  GPU   GI   CI        PID   Type   Process name                              GPU Memory |
    |        ID   ID                                                               Usage      |
    |=========================================================================================|
    |  No running processes found                                                             |
    +-----------------------------------------------------------------------------------------+
