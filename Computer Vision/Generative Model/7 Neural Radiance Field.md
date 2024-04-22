# Neural Radiance Field

最后我们把 NeRF/3DGS/CryoDRGN 也融入到这个生成式模型的框架之中。

NeRF 的输入是相机位姿，以及场景的“范围”。然后用固定的采样方式从相机位姿获取其对应的 5D 点，然后把 5D 点映射到 256D 最后到 4D 表达，最后再用固定的累积方式重建图片。

3DGS 的输入是相机位姿，以及场景的“范围”。然后用固定的 MVP 变换方式从获取 62D 场景中的 Gaussian 球，最后再用固定的累积方式重建图片。

CryoDGRN 的输入是图片，得到一个 Latent。Decoder 是一个 2D->?D->1D 的频域 NeRF。

| $q(z\mid x;\theta)$ | $p(z)$  | $p(x\mid z)$ |           |
| :-----------------: | :-----: | :----------: | :-------: |
|        Known        |  Known  |    Known     |   3DGS    |
|        Known        |  Known  |   Unknown    | Diffusion |
|        Known        | Unknown |    Known     |     -     |
|        Known        | Unknown |   Unknown    |     -     |
|       Unknown       |  Known  |    Known     |    EM     |
|       Unknown       |  Known  |   Unknown    |    VAE    |
|       Unknown       | Unknown |    Known     |           |
|       Unknown       | Unknown |   Unknown    |    AE     |