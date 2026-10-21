# 数据来源、哈希与脱敏记录

## 1. 上游数据集

- 仓库：`Dxsssu/SecAlertBench`，锁定提交 `42a84889fda912ca432c994924a1ccd4b9df6274`
- 源文件 SHA-256：`33f95305d1c42f8e615e4f94066119570859dee7eb086dff7c2273536c932ea3`
- 规模：8 322 条脱敏告警（Attack 2 496 / Non-Attack 5 826）；作者声明来自三家企业 SOC，未经独立取证
- **许可状态：该仓库 `license` 字段为 null（无许可证）** → 本仓库不逐条再分发其数据，只发布聚合指标

## 2. 被测模型工件

| 工件 | 标识（SHA-256 / 摘要） |
|---|---|
| `qwen3.6:35b-a3b-mtp-q4_K_M` | `2f2015ccff10e15cb16889a4771479e48eab740e607924e76fd27ba71f9e11bb` |
| `qwen3.8:27b` | `da40f4e2edac9287190fa1fc207c751533ffc47da910304e9d16caea29de5eb4` |
| `Laya-Q8_0.gguf`（449,397,600 B） | `c06528c5746d3bb8baa72a27938be95abbfd0b226f8471e8a9e365ed0bb066d2` |
| `Clef-Flash-Q4_K_M.gguf`（6,486,448,288 B） | `fd3e90605e8103307dca37cb5a8cdb036267e2fe3cb2d908d80a8ceb9ec0638c` |
| `Clef-Q4_K_M.gguf`（19,232,219,200 B） | `bd9b8ae24c5752a2bb5e2733d5168340d5f04457e02b8bf0d1bc9285ec3d4f61` |

运行时：Ollama 0.34.4（生成式模型）；llama.cpp v0.6.0 的 `/v1/systemone`（决策模型）。
注意：Ollama 无法加载 `modern-bert` 架构，报 `check_tensor_dims: tensor 'blk.28.ffn_up.weight' has wrong shape`，
因此决策模型必须走 llama.cpp 独立实例。

## 3. 本仓库数据文件的 SHA-256

| `results/decision_models.json` | `c98fd128e4df1a0ff53f4beace95a901ae0697f9b2b90af7227aa30f90bcaaee` |
| `results/derived_analysis.json` | `c699d60539749679fbb582007c6e01e961441e26afeed7b416556139ab6180d0` |
| `results/progress_last.json` | `27704c242358ed1bd99d326a58d30db75536cf1e9f59a26369eec46756d259b3` |
| `results/round1_round2.json` | `5d8e4e073175f5f76faebc6ef8b15c871c1f31c7579ec7acd1dfdc93a01f956d` |
| `results/round3.json` | `934d61467f839f9601588af02b6ee15e2a0f98ab2eb47de874ae9c6cde2dca6d` |
| `results/statuses.json` | `4f323f72f5b1d1f4bf847b7c308c342b986f139815da74d772aaf794c2559ed7` |

生成方式：`round3.json`、`decision_models.json`、`statuses.json`、`progress_last.json` 为评测流水线
发布产物中的聚合字段直接导出；`round1_round2.json` 与 `derived_analysis.json` 由作者侧从已发布报告
逐项转录，以及对开发/验证集与决策模型原始响应的独立复算（口径差异见 `docs/PAPER.md` §5–§6）。

## 4. 脱敏记录

| 处理 | 说明 |
|---|---|
| 移除公网主机地址 | 原报告中出现的管理站点公网 IP 已全部删除，改为"管理站点"表述 |
| 移除绝对路径与主机名 | 站点目录、设备绝对路径、设备主机名均不出现在本仓库 |
| 不发布逐条判定 | 上游无许可证，且逐条表含数据集自身字段（告警类别、规则名称） |
| 不发布原始告警载荷 | 脱敏数据的二次分发权不在我们手上 |
| 保留端口号 | 仅为本机回环端口编号，不含主机信息，用于说明运行时隔离方式 |

## 5. 可复核性限制

评测设备已于 2026-10-12 归还下线，原始 attempt 明细无法重放。本仓库数字来自两条路径：
(a) 流水线自带的已发布聚合产物；(b) 设备在线期间作者侧的独立复算。两者均已分别标注，
读者不应把 (b) 当作可重放的一手证据。
