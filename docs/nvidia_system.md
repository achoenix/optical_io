![image](https://github.com/user-attachments/assets/c0925387-cde2-4a46-aef7-3a067a28196f)# NVIDIA System I/O Analysis

## Design principles
- AI workloads is latency and bandwidth sensitive
- switching (both scale-up and scale-out) becomes more important
- copper vs optics is power consumption-driven

#### Connectivity comparison
![Image](https://github.com/user-attachments/assets/5c34ec4a-951b-42de-817a-ff58e0f8c059)

## GPU & System comparison

### CPU comparison

| Specification                  | B300                                 | B200                                 | B100                                 |
|--------------------------------|--------------------------------------|--------------------------------------|--------------------------------------|
| **Architecture**               | Blackwell Ultra                      | Blackwell                            | Blackwell                            |
| **Semiconductor Node**         | TSMC 4NP                             | TSMC 4NP                             | TSMC 4NP                             |
| **Chip Packaging**             | Dual GB100 dies with NV-HBI interconnect | Dual GB100 dies with NV-HBI interconnect | Dual GB100 dies with NV-HBI interconnect |
| **FP4 Tensor Performance**     | 15 PFLOPS (dense), 30 PFLOPS (sparse) | 10 PFLOPS (dense), 20 PFLOPS (sparse) | 7 PFLOPS (dense), 14 PFLOPS (sparse) |
| **INT8 Tensor Performance**    | 7.5 petaOPS (dense), 15 petaOPS (sparse) | 5 petaOPS (dense), 10 petaOPS (sparse) | 3.5 petaOPS (dense), 7 petaOPS (sparse) |
| **FP16 Tensor Performance**    | 3.75 PFLOPS (dense), 7.5 PFLOPS (sparse) | 2.5 PFLOPS (dense), 5 PFLOPS (sparse) | 1.85 PFLOPS (dense), 3.5 PFLOPS (sparse) |
| **FP64 Tensor Performance**    | 68 TFLOPS                            | 45 TFLOPS                            | 30 TFLOPS                               |
| **Memory**                     | 288 GB HBM3e                          | 192 GB HBM3e                          | 192 GB HBM3e                          |
| **Memory Bandwidth**           | Up to 8 TB/s                          | Up to 8 TB/s                          | Up to 8 TB/s                          |
| **Power Consumption (TDP)**    | Not specified                         | 1,000W                                | 700W                                  |

Note:
- Chip CoWoS Packaging: Each GPU utilizes a dual-die design with two GB100 dies connected via NVIDIA's NV-High Bandwidth Interface (NV-HBI), providing a 10 TB/s interconnect.
- TDP depends on configurations [[ref](https://www.tomshardware.com/pc-components/gpus/nvidias-next-gen-ai-gpu-revealed-blackwell-b200-gpu-delivers-up-to-20-petaflops-of-compute-and-massive-improvements-over-hopper-h100?utm_source=chatgpt.com)]
  - HGX B200 Configuration: Each B200 GPU is rated at a TDP of 1,000W. 
  - GB200 Superchip Configuration: When integrated into the GB200 Superchip, a single B200 GPU can have a configurable TDP of up to 1,200W, with the entire superchip (comprising two B200 GPUs and one Grace CPU) reaching up to 2,700W.

### CPU rack system comparison

| Specification                  | H100 NVL8 | B100 NVL72 | B200 NVL72 | B300 NVL72 | Rubin NVL144 | Rubin NVL576 |
|--------------------------------|-----------|------------|------------|------------|--------------|--------------|
| **Number of Trays**            | Not specified | Not specified | Not specified | Not specified | Not specified | Not specified |
| **Number of GPUs**             | 8         | 72         | 72         | 72         | 144          | 576          |
| **Number of CPUs**             | Not specified | 36 | 36 | 36 | Not specified | Not specified |
| **Memory Capacity per GPU**    | 94 GB HBM3 | Up to 192 GB HBM3e | Up to 192 GB HBM3e | 288 GB HBM3e | Not specified | Not specified |
| **Total Memory Capacity**      | 752 GB    | Up to 13.824 TB | Up to 13.824 TB | 20.736 TB | Not specified | Not specified |
| **Memory Bandwidth per GPU**   | 3.9 TB/s  | Up to 8 TB/s | Up to 8 TB/s | Up to 8 TB/s | Not specified | Not specified |
| **Total Memory Bandwidth**     | 31.2 TB/s | Up to 576 TB/s | Up to 576 TB/s | 576 TB/s | Not specified | Not specified |
| **Scale-Up Interconnect**      | NVLink 4  | NVLink 5 | NVLink 5 | NVLink 5 | NVLink 6? | NVLink 6? |
| **Scale-Up Interconnect BW per GPU** | 900 GB/s | 1.8 TB/s | 1.8 TB/s | 1.8 TB/s | Not specified | Not specified |
| **Total Scale-Up Interconnect BW** | 7.2 TB/s | 129.6 TB/s | 129.6 TB/s | 129.6 TB/s | Not specified | Not specified |
| **Scale-Out Interconnect**     | Not specified | InfiniBand/Ethernet | InfiniBand/Ethernet | InfiniBand/Ethernet | InfiniBand/Ethernet | InfiniBand/Ethernet |
| **Total Rank System Power**    | Not specified | ~50 kW | ~72 kW | Not specified | Not specified | Not specified |

Note:
- System power consumption varies from different sources:
  - B200 NVL72 System: Each DGX B200 unit, housing eight B200 GPUs, consumes approximately 14.3 kW. In a standard data center setup, this translates to around 60 kW per rack. [AMAX ENGINEERING](https://www.amax.com/comparing-nvidia-blackwell-configurations/?utm_source=chatgpt.com)
  - GB200 NVL72 System: The GB200 NVL72 rack, comprising 72 GB200 GPUs, has a total power consumption of approximately 120 kW.[CONTINUUM LABS](https://training.continuumlabs.ai/infrastructure/servers-and-chips/nvidia-gb200-nvl72?utm_source=chatgpt.com)



## GB100 NVL8 system

## GB200 NVL72 system


## Information
### GTC 2025


## Reference
- https://www.nextplatform.com/2025/03/18/nvidia-weaves-silicon-photonics-into-infiniband-and-ethernet/
- https://arstechnica.com/ai/2025/03/nvidia-announces-rubin-ultra-and-feynman-ai-chips-for-2027-and-2028/
- https://www.tomshardware.com/pc-components/gpus/nvidia-announces-rubin-gpus-in-2026-rubin-ultra-in-2027-feynam-after
- https://www.servethehome.com/nvidia-gtc-2025-keynote-live-coverage/
- https://www.theverge.com/news/631835/nvidia-blackwell-ultra-ai-chip-gb300
- https://mp.weixin.qq.com/s/Fm6hFvHygsx0IcQ27G73Xw

