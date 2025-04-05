# NVIDIA System I/O Analysis

## Design principles
- AI workloads is latency and bandwidth sensitive
- switching (both scale-up and scale-out) becomes more important
- copper vs optics is power consumption-driven


## GPU & System comparison

### CPU comparison

| Specification                  | H100          | B100                                 | B200                                 | B300                                 | Rubin         | Rubin Ultra    |
|----------------------|---------------|--------------------------------------|--------------------------------------|--------------------------------------|---------------|---------------|
| **Architecture**     | Hopper        | Blackwell                            | Blackwell                            | Blackwell Ultra                      | Rubin         | Rubin Ultra    |
| **Process Node**     | TSMC 4N       | TSMC 4NP                             | TSMC 4NP                             | TSMC 4NP                             | Not specified | Not specified |
| **Chip Packaging**   | Monolithic    | Dual GB100 dies with NV-HBI          | 2 GB100 dies with NV-HBI             | 2 GB100 dies with NV-HBI             | 2 Rubin dies with NV-HBI | 4 Rubin dies with NV-HBI  |
| **FP4 Tensor**       | Not supported | 7 PFLOPS (dense) <br> 14 PFLOPS (sparse) | 10 PFLOPS (dense) <br> 20 PFLOPS (sparse) | 15 PFLOPS (dense) <br> 30 PFLOPS (sparse) | 50 PFLOPS | 100 PFLOPS |
| **INT8 Tensor**      | 0.4 P-OPS     | 3.5 P-OPS (dense) <br> 7 P-OPS (sparse) | 5 P-OPS (dense) <br> 9 P-OPS (sparse) | 7.5 P-OPS (dense), 15 P-OPS (sparse) | Not specified | Not specified |
| **FP16 Tensor**      | 0.2 PFLOPS    | 1.85 PFLOPS (dense) <br> 3.5 PFLOPS (sparse) | 2.5 PFLOPS (dense) <br> 4.5 PFLOPS (sparse) | 3.75 PFLOPS (dense) <br> 7.5 PFLOPS (sparse) | Not specified | Not specified |
| **FP64**             | 34 TFLOPS     | 30 TFLOPS                            | 40 TFLOPS                            | 68 TFLOPS                            | Not specified | Not specified |
| **Memory**           | 80 GB HBM3    | 192 GB HBM3e                         | 192 GB HBM3e                          | 288 GB HBM3e                        | 288 GB HBM4 | 1 TB HBM4e |
| **Memory Bandwidth** | 3.2 TB/s      | Up to 8 TB/s （4Tb/s /die)           | Up to 8 TB/s  （4Tb/s /die)           | Up to 8 TB/s    （4Tb/s /die)       | 8 TB/s /die | 8 TB/s /die |
| **Power (TDP)**      | 700W          | 700W                                 | 1,000W                                | Not specified                       | Not specified | Not specified |


Note:
- Chip CoWoS Packaging: Each GPU utilizes a dual-die design with two GB100 dies connected via NVIDIA's NV-High Bandwidth Interface (NV-HBI), providing a 10 TB/s interconnect.
- TDP depends on configurations [[ref](https://www.tomshardware.com/pc-components/gpus/nvidias-next-gen-ai-gpu-revealed-blackwell-b200-gpu-delivers-up-to-20-petaflops-of-compute-and-massive-improvements-over-hopper-h100?utm_source=chatgpt.com)]
  - HGX B200 Configuration: Each B200 GPU is rated at a TDP of 1,000W. 
  - GB200 Superchip Configuration: When integrated into the GB200 Superchip, a single B200 GPU can have a configurable TDP of up to 1,200W, with the entire superchip (comprising two B200 GPUs and one Grace CPU) reaching up to 2,700W.
  - Rubin HBM bandwidth is sourced from: [[ref](https://www.theregister.com/2025/03/29/nvidia_moores_law/)]

### CPU rack system comparison

| Specification                  | H100 NVL8 | Blackwell NVL72 | Blackwell Ultra NVL72 | Rubin NVL144 | Rubin Ultra NVL576 |
|--------------------------------|-----------|------------|------------|--------------|--------------|
| **Number of Trays**            | N/A       | 36         | 36         | 36           | 36           |
| **GPU**                        | H100      | B100/B200  | B300       | Rubin        | Rubin Ultra  |
| **Number of GPUs**             | 8         | 72 (144 dies) | 72 (144 dies) | 144 dies | 576 dies   |
| **CPU**                        | Grace     | Grace      | Grace      | Vera         | Vera         |
| **Number of CPUs**             | N/A       | 36         | 36         | 36?          | 36?          |
| **CPU-GPU Bandwidth**          |  TB/s  |  TB/s |  TB/s | 1.8 TB/s | 1.8 TB/s |
| **Memory Capacity per GPU**    | 94 GB HBM3 | 192 GB HBM3e | 288 GB HBM3e | 520 GB HBM4 | 1 TB HBM4e |
| **Total Memory Capacity**      | 752 GB    | 13.824 TB | 20.736 TB | 75 TB | 365 TB (inconsistent with 1TB*144) |
| **Memory Bandwidth per GPU**   | 3.9 TB/s  | 8 TB/s | 8 TB/s | 13 TB/s | ? TB/s |
| **Total Memory Bandwidth**     | 31.2 TB/s | 576 TB/s | 576 TB/s | ? TB/s | 4500 TB/s |
| **Scale-Up Interconnect**      | NVLink 4  | NVLink 5 | NVLink 5 | NVLink 6 | NVLink 7 |
| **Scale-Up Interconnect BW per GPU** | 900 GB/s (18x4x100Gb/s) | 1.8 TB/s (18x4x200Gb/s) | 1.8 TB/s | 3.6 TB/s (18x4x400Gb/s?) | ? TB/s (?) |
| **Total Scale-Up Bandwidth**  | 7.2 TB/s | 129.6 TB/s | 129.6 TB/s | 260 TB/s | 1.5 PB/s |
| **Server NIC**                 |     |   | ConnectX-8 (800Gb/s) | ConnectX-9 (1.6Tb/s) | ConnectX-9 (1.6T/s) |
| **Scale-Out Interconnect**     | Not specified | InfiniBand/Ethernet | InfiniBand/Ethernet | InfiniBand/Ethernet | InfiniBand/Ethernet |
| **Total Scale-Out Bandwidth**  |  TB/s |  TB/s | 14.4 TB/s （144x800Gb/s) | 28.8 TB/s (144x1.6Tb/s) | 115.2 TB/s (576x1.6Tb/s?) |
| **Total Rank System Power**    | Not specified | ~72 kW | Not specified | Not specified | 600 kW |
| **FP4 Dense Inference**        | PLFOPS |  PFLOPS | 1.1 EFLOPS | 3.6 EFLOPS | 15 EFLOPS |
| **FP8 Training**               | PLFOPS |  PFLOPS | 0.36 EFLOPS | 1.2 EFLOPS | 5 EFLOPS |

Note:
- System power consumption varies from different sources:
  - B200 NVL72 System: Each DGX B200 unit, housing eight B200 GPUs, consumes approximately 14.3 kW. In a standard data center setup, this translates to around 60 kW per rack. [AMAX ENGINEERING](https://www.amax.com/comparing-nvidia-blackwell-configurations/?utm_source=chatgpt.com)
  - GB200 NVL72 System: The GB200 NVL72 rack, comprising 72 GB200 GPUs, has a total power consumption of approximately 120 kW.[CONTINUUM LABS](https://training.continuumlabs.ai/infrastructure/servers-and-chips/nvidia-gb200-nvl72?utm_source=chatgpt.com)
- Since Rubin, the # of GPUs are counted as the # of dies inside a GPU package. This causes confusions for comparison. For example, B300 NVL72 has 72 B300 GPUs with 144 GPU dies (2 dies per GPU packages) while Rubin NVL144 has 144 CPU dies in 72 GPU packages and Rubin Ultra NVL576 has 576 GPU dies in 144 GPU packages. 
- Memory bandwidth numbers for Rubin systems shown in GTC 2025 Keynote are not consistent for per GPU and total
- Memory capacity of Rubin Ultra shown in GTC 2025 Keynote are not consistent for per GPU and total
- NVLink 6 bandwidth and ConnextX-9 bandwidth are adopted from source [guru3d](https://www.guru3d.com/story/nvidia-confirms-nextgeneration-architecture-is-based-on-vera-rubin-gpu/)


## GB200 NVL72 system


## NVIDIA CPO

### Quantum-X switch
GTC 2025 Keynote
- NVidia will ship Quantum-X (InfiniBand) switch later in 2025, and then a Specturm-X (Ethernet) switch in the second half of 2026.
- Up to 512 fiber ports on the Spectrum-X switch.
- 2 optical transceivers per GPU now, 6 optical transceivers per GPU for larger scale with additional layer of switching
- each transceiver (800G by assumption) costs $1000 and 6W. A 1M GPU datacenter will dedicate 6MW, 10 Rubin Ultra racks for optics
- 

![Image](https://github.com/user-attachments/assets/60a30901-0052-4e77-aa3c-196377bcd09d)

#### Connectivity comparison
![Image](https://github.com/user-attachments/assets/5c34ec4a-951b-42de-817a-ff58e0f8c059)


## Information
### GTC 2025


## Reference
- https://www.nextplatform.com/2025/03/18/nvidia-weaves-silicon-photonics-into-infiniband-and-ethernet/
- https://arstechnica.com/ai/2025/03/nvidia-announces-rubin-ultra-and-feynman-ai-chips-for-2027-and-2028/
- https://www.tomshardware.com/pc-components/gpus/nvidia-announces-rubin-gpus-in-2026-rubin-ultra-in-2027-feynam-after
- https://www.servethehome.com/nvidia-gtc-2025-keynote-live-coverage/
- https://www.theverge.com/news/631835/nvidia-blackwell-ultra-ai-chip-gb300
- https://mp.weixin.qq.com/s/Fm6hFvHygsx0IcQ27G73Xw

