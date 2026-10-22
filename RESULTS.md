# Reported results

These summaries reflect the current manuscript. Code and pretrained models will be released upon publication.

## Reduced-resolution reconstruction

Evaluation follows the Wald protocol with sensor-matched degradation, a spatial ratio of four, and 100 test scenes per sensor. PSNR is pooled over all evaluated pixels and bands; it is not the arithmetic mean of scene-wise PSNR. Higher PSNR, SSIM, Q4/Q8, Qave, and SCC are better; lower SAM and ERGAS are better.

| Sensor | PSNR (dB) | SSIM | Q4/Q8 | Qave | SAM (degrees) | ERGAS | SCC |
|---|---:|---:|---:|---:|---:|---:|---:|
| WorldView-4 | 37.83 | 0.9742 | 0.8768 (Q4) | 0.8855 | 1.3843 | 1.3410 | 0.9767 |
| WorldView-2 | 36.31 | 0.9690 | 0.7277 (Q8) | 0.7174 | 2.9385 | 2.8329 | 0.9786 |

FCU-Net leads all seven listed metrics on WorldView-4 and five on WorldView-2; PanNet has the highest Q8 and ADWM the highest Qave on WorldView-2. The WorldView-4 PSNR improvement over BPDUN, the next-best PSNR method, is 1.55 dB.

## Sensor-specific training

Each sensor uses a separately trained model. The ranks below compare PSNR with the sensor-specific baseline sets reported in the manuscript.

| Sensor | Test scenes | FCU-Net PSNR (dB) | Highest baseline PSNR (dB) | Baseline | FCU-Net rank |
|---|---:|---:|---:|---|---:|
| WorldView-4 | 100 | 37.83 | 36.28 | BPDUN | 1 |
| WorldView-2 | 100 | 36.31 | 35.85 | PanFormer / ADWM | 1 |
| IKONOS | 40 | 38.91 | 38.51 | FusionNet | 1 |
| QuickBird | 100 | 47.86 | 47.96 | FusionMamba | 2 |
| Gaofen-1 | 82 | 42.36 | 41.63 | FusionMamba | 1 |
| WorldView-3 | 32 | 35.11 | 34.51 | PanNet | 1 |

## Full-resolution observation consistency

There is no high-resolution MS ground truth at full resolution. QNR, spectral distortion D_lambda, and spatial distortion D_s measure consistency with the available observations. Higher QNR and lower distortion values are better. All three quantities are averaged separately over scenes.

| Sensor | Setting | QNR | D_lambda | D_s |
|---|---|---:|---:|---:|
| WorldView-4 | FCU-Net, supervised | 0.8966 | 0.0556 | 0.0540 |
| WorldView-4 | FCU-Net +cal | 0.9103 | 0.0339 | 0.0582 |
| WorldView-2 | FCU-Net, supervised | 0.8888 | 0.0598 | 0.0552 |
| WorldView-2 | FCU-Net +cal | 0.9151 | 0.0421 | 0.0462 |

The table reports all 40 scenes per sensor. The optional +cal setting fine-tunes only the final injection module using scenes 0-19; all other model parameters remain frozen. On the held-out scenes 20-39, mean QNR increases from 0.8979 to 0.9137 on WorldView-4 and from 0.8779 to 0.9033 on WorldView-2. Calibration can smooth fine texture, so its observation-consistency results should be interpreted separately from reference-based reconstruction accuracy.
