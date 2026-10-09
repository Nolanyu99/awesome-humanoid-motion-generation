# Awesome Humanoid Motion Generation for Loco-Manipulation

[English](README.md) | **简体中文**

> 收录范围：面向 humanoid loco-manipulation，能够**生成、规划、预测、重建全身运动，或显式提供中间全身运动表示**，再由跟踪器、控制器或策略学习模块执行的论文与项目。

本列表关注任务意图/感知与底层全身执行之间的“运动层”。它不试图收录所有直接输出关节动作的端到端 VLA、遥操作系统或普通控制器。

## 总览

| 工作 | 年月 | 类别 | Motion 表示或生成方法 | 输入 → 输出 | 执行/跟踪 | 真机 | 链接 |
|---|---:|---|---|---|---|:---:|---|
| Opt2Skill | 2024-09 预印本 / 2025 发表 | Motion Planning / Optimization | DDP 生成包含力矩参考的动力学可行全身轨迹 | 任务/接触约束 → 全身状态、控制和力矩轨迹 | RL 策略跟踪离线最优轨迹并输出关节位置目标 | ✅ Digit | [项目](https://opt2skill.github.io/) · [arXiv](https://arxiv.org/abs/2409.20514) · GitHub：TBD |
| Physically Consistent Humanoid Loco-Manipulation using Latent Diffusion Models | 2025-04 | Motion Planning / Optimization | LDM 生成交互图像，从中提取接触和机器人构型以初始化全身轨迹优化 | 场景图像 + 任务分解/提示 → 接触和构型 → 物理一致的全身轨迹 | 全身轨迹优化 | ❌ 仿真 | 项目：TBD · [arXiv](https://arxiv.org/abs/2504.16843) · GitHub：TBD |
| DreamControl | 2025-09 | Generative Motion Prior | 受引导的人体运动扩散先验生成参考轨迹 | 文本任务 + 时空约束 → 类人全身参考运动 | 使用生成参考训练任务级 RL 策略 | ✅ Unitree G1 | [项目](https://genrobo.github.io/DreamControl/) · [arXiv](https://arxiv.org/abs/2509.14353) · [GitHub](https://github.com/GenRobo/DreamControl) |
| VisualMimic | 2025-09 | Learned Motion Interface | 视觉策略生成 root、手、脚和头部的稀疏关键点 | 第一视角视觉 + 本体状态 → 关键点命令 | 通过 teacher-student 训练的通用关键点跟踪器 | ✅ Unitree G1 | [项目](https://visualmimic.github.io/) · [arXiv](https://arxiv.org/abs/2509.20322) · [GitHub](https://github.com/visualmimic/VisualMimic) |
| DreamControl-v2 | 2026-04 | Generative Motion Prior | 直接在统一机器人运动空间训练受引导扩散模型 | 文本/时空任务引导 → 机器人空间全身参考轨迹 | 下游 RL 策略从生成参考中学习 | ✅ Unitree G1 | 项目：TBD · [arXiv](https://arxiv.org/abs/2604.00202) · GitHub：TBD |
| REFINE-DP | 2026-03 | Learned Motion Interface | DiT diffusion planner 生成 base velocity 与手部位姿命令，并与控制器联合 RL 微调 | 机器人/任务状态与示范 → 高层运动命令 | 联合优化的 RL loco-manipulation controller 跟踪手和落脚命令 | ✅ Booster T1 | [项目](https://refine-dp.github.io/REFINE-DP/) · [arXiv](https://arxiv.org/abs/2603.13707) · GitHub：TBD |
| Humanoid-DART | 2026-06 | Generative Motion Prior | goal-conditioned 双分支扩散联合生成机器人/物体轨迹并迭代重标注 | 目标 + 少量示范/elite rollout → 机器人和物体参考轨迹 | RL tracker 执行；物理 rollout 筛选并重标注生成轨迹 | ✅ Unitree G1 | 项目：TBD · [arXiv](https://arxiv.org/abs/2606.26855) · GitHub：TBD |
| MotionDisco | 2026-06 | Motion Planning / Optimization | LLM 引导的接触程序进化搜索 + 顺序 kinodynamic trajectory optimization | 目标提示 + 可行性反馈 → 接触序列 → 动力学可行全身轨迹 | RL 策略跟踪发现的轨迹 | ✅ Humanoid 真机 | [项目](https://atarilab.github.io/motiondisco.io/) · [arXiv](https://arxiv.org/abs/2606.06139) · GitHub：TBD |
| HumanoidMimicGen | 2026-05 | Synthetic Motion / Data Generation | 将少量全身技能适配到新场景，并用全身规划生成无碰撞示范 | 少量源示范 + 新物体/场景状态 → 大规模 loco-manipulation 轨迹数据 | WBC 执行生成轨迹；视觉策略混合合成与真实数据训练 | ✅ Unitree G1 | [项目](https://humanoidmimicgen.github.io/) · [arXiv](https://arxiv.org/abs/2605.27724) · [GitHub](https://github.com/NVlabs/humanoidmimicgen) |
| GRAIL | 2026-06 | Synthetic Motion / Data Generation | 视频基础模型生成交互视频，再进行 metric 4D HOI 重建和机器人 retargeting | 3D 资产 + 已知场景 → 交互视频 → SMPL-X/物体 6-DoF → G1 轨迹/动作 | 通用跟踪策略与第一视角 RGB 策略 | ✅ Unitree G1 | [项目](https://research.nvidia.com/labs/dair/grail/) · [arXiv](https://arxiv.org/abs/2606.05160) · [GitHub](https://github.com/NVlabs/GRAIL) |
| SUGAR | 2026-05 | Synthetic Motion / Data Generation | 从人体视频提取运动/接触先验，经物理 RL 修正后生成命令 | 非结构化人体视频 → 人/物运动与接触 → 可执行机器人技能/命令 | command generator + command tracker 分层策略 | ✅ Humanoid 真机 | [项目](https://tianshuwu.github.io/sugar-humanoid/) · [arXiv](https://arxiv.org/abs/2605.20373) · [GitHub](https://github.com/tianshuwu/SUGAR) |
| MotionWAM | 2026-06 | Learned Motion Interface | 视频世界模型中间去噪特征条件化统一全身 motion latent/token | 第一视角 RGB → 覆盖行走、躯干、高度、脚和手的全身 motion token | 实时端到端 world-action policy | ✅ Unitree G1 | [项目](https://dit4dit.github.io/MotionWAM/) · [arXiv](https://arxiv.org/abs/2606.09215) · GitHub：TBD |
| Whole-Body UMI | 2026-09 | Learned Motion Interface | task-agnostic、末端轨迹条件化的实时全身 motion generator | UMI RGB → diffusion-policy EE 轨迹 → 全身运动 | WB-UMI motion generator + WBC 的异步分层系统 | ✅ Unitree G1 | [项目](https://wholebody-umi.github.io/) · [arXiv](https://arxiv.org/abs/2609.22829) · GitHub：TBD |
| λ₀ / HumanVerse-500 | 2026-09 | Learned Motion Interface | 使用共享的人/机器人表示进行全身人体运动中间训练，包含 G1-compatible motion latent | 第一视角视频 + 语言 + 人体/手部运动 → humanoid 全身动作 | 三阶段 VLA 预训练和 robot post-training | ✅ Unitree G1 | 项目：TBD · [arXiv](https://arxiv.org/abs/2610.00438) · GitHub：TBD |
| OCLO | 2026-10 | Motion Planning / Optimization | 解析可达性先验在线生成骨盆高度和躯干朝向，并用 policy-in-the-loop sampling 优化 | 两个末端目标 + 测量力 → 顺应性 EE 参考与在线姿态 | 冻结的下肢 RL 策略、差分 IK，可选 CEM 姿态优化 | ✅ Unitree G1 | [项目](https://oclo-humanoid.github.io/) · [arXiv](https://arxiv.org/abs/2610.05678) · GitHub：TBD |
| InterMimicGen | 2026-10 | Synthetic Motion / Data Generation | 接触保持 retargeting + 任务保持运动扰动 + 物理筛选 | 人–物 motion 数据 → 机器人参考 → 持续扩展的可执行运动集 | 通用物理 tracker 验证运动并迁移到真机 | ✅ Humanoid 真机 | [项目](https://sirui-xu.github.io/InterMimicGen/) · [arXiv](https://arxiv.org/abs/2610.06850) · GitHub：TBD |
| Workhorse | 2026-10 | Learned Motion Interface | flow-matching 视觉 planner 预测躯干、双腕、双脚目标轨迹块 | 第一视角 RGB + five-link 历史 → 1.16 秒 five-link target chunk | RL 全身 tracker 闭环执行短时目标 | ✅ Unitree G1 | [项目](https://hybridrobotics.github.io/workhorse/) · [arXiv](https://arxiv.org/abs/2610.09117) · GitHub/代码：TBD |
| Task and Motion Planning for Humanoid Loco-manipulation | 2025-08 | Motion Planning / Optimization | 用接触模式搜索统一任务、接触与全身运动规划 | 符号任务 + 机器人/物体/环境约束 → 动力学可行长时序计划 | 含全身动力学和执行器约束的优化 | TBD | 项目：TBD · [arXiv](https://arxiv.org/abs/2508.14099) · GitHub：TBD |
| Learning Differentiable Reachability Maps for Optimization-based Humanoid Motion Generation | 2025-08 | Motion Planning / Optimization | 学习可微 task-space reachability map 并作为连续轨迹优化约束 | 机器人运动学 + 任务/接触目标 → 落脚、多接触或 loco-manipulation 运动 | 连续优化 | TBD | 项目：TBD · [arXiv](https://arxiv.org/abs/2508.11275) · GitHub：TBD |
| OmniRetarget | 2025-09 | Synthetic Motion / Data Generation | interaction mesh retargeting 保持人–地形–物体几何与接触 | 人体运动 + 场景/物体配置 → 增广机器人轨迹 | 本体感知 RL 策略跟踪生成轨迹 | ✅ Unitree G1 | [项目](https://omniretarget.github.io/) · [arXiv](https://arxiv.org/abs/2509.26633) · GitHub：TBD |
| GenMimic | 2025-12 | Synthetic Motion / Data Generation | 将生成视频提升为 4D 人体运动并重定向为机器人关键点 | 生成人体视频 → 4D 运动 → humanoid 轨迹 | 物理感知 3D-keypoint RL tracker | ✅ Unitree G1 | [项目](https://genmimic.github.io/) · [arXiv](https://arxiv.org/abs/2512.05094) · GitHub：TBD |
| HuMI | 2026-02 | Learned Motion Interface | 便携 robot-free 设备采集丰富人体全身运动并用于分层策略学习 | 腕部图像 + 人体运动 + 夹爪状态 → 可执行 humanoid 全身技能 | 高层 diffusion policy + 底层全身控制器 | ✅ Humanoid 真机 | [项目](https://humanoid-manipulation-interface.github.io/) · [arXiv](https://arxiv.org/abs/2602.06643) · GitHub：TBD |
| BifrostUMI | 2026-05 | Learned Motion Interface | UMI 式夹爪和轻量 VR trackers 采集稀疏全身关键点 | 腕部 RGB + 骨盆/腕/脚关键点 + 夹爪状态 → 机器人全身参考 | 关键点策略、空间重定向和全身控制器 | ✅ Unitree G1 | [项目](https://baai-aether.github.io/BifrostUMI/) · [arXiv](https://arxiv.org/abs/2605.03452) · GitHub：TBD |
| Imagine2Real | 2026-05 | Synthetic Motion / Data Generation | 将视频先验转换为 base/手/物体的稀疏 4D point trajectories | 生成交互视频 → 稀疏 4D 关键点 → 机器人交互运动 | 在 BFM latent space 中搜索的关键点 tracker | ✅ Humanoid 真机 | 项目：TBD · [arXiv](https://arxiv.org/abs/2605.22272) · GitHub：TBD |
| WT-UMI | 2026-06 | Learned Motion Interface | force-supervised planner 预测末端位姿块与接触力轨迹 | 触觉图像 + 力 + EE 位姿 → 位姿和力参考 | 触觉 admittance controller | ✅ Humanoid 真机 | [项目](https://wt-umi.github.io/WTUMI/) · [arXiv](https://arxiv.org/abs/2606.13232) · GitHub：TBD |
| ReactiveBFM | 2026-06 | Learned Motion Interface | 自回归生成式 planner 生成闭环 spatial-reference chunks | 文本命令 + 当前物理状态 → 反应式全身参考轨迹 | 异步重规划 + 预训练 BFM tracker | ✅ Unitree G1 | [项目](https://xiao-chen.tech/) · [arXiv](https://arxiv.org/abs/2606.30362) · GitHub：TBD |
| TEXEDO | 2026-06 | Generative Motion Prior | 采样文本条件运动，并以可行性/语义 verifier 选择候选 | 文本提示 → 候选机器人运动 → 可执行参考 | 全身运动跟踪控制器 | ✅ Unitree G1 | 项目：TBD · [arXiv](https://arxiv.org/abs/2606.22998) · GitHub：TBD |
| VLK | 2026-06 | Synthetic Motion / Data Generation | 在重建 3D 场景中合成 vision-language-kinematics 监督 | 语言 + 第一视角图像 → 短时全身运动学轨迹 | 全身 tracker | ✅ Unitree G1 | [项目](https://vision-language-kinematics.github.io/) · [arXiv](https://arxiv.org/abs/2606.30645) · GitHub：TBD |
| GenHOI | 2026-06 | Synthetic Motion / Data Generation | 把生成交互视频转为接触约束和优化后的参考运动 | 文本 + 重建场景 → 视频/接触线索 → refined robot/object trajectory | 闭环跟踪控制器 | ✅ Humanoid 真机 | 项目：TBD · [arXiv](https://arxiv.org/abs/2606.12995) · GitHub：TBD |
| RoboReact | 2026-08 | Synthetic Motion / Data Generation | 从生成第一视角视频提取保持几何关系的 3D 交互关键帧 | 单张 RGB-D → 生成视频 → 重定向/修正的全身技能 | 在线物体中心重定位 + 全身控制器 | ✅ Humanoid 真机 | 项目：TBD · [arXiv](https://arxiv.org/abs/2608.03387) · GitHub：TBD |
| HIGenNTO | 2026-09 | Motion Planning / Optimization | 在接触、避碰和支撑约束下优化 text-motion model 的初始噪声 | 任务/场景程序 + 稀疏约束 → 全身交互参考 | tracking policy 与深度条件视觉运动策略 | ✅ Unitree G1 | 项目：TBD · [arXiv](https://arxiv.org/abs/2609.22611) · GitHub：TBD |
| KINO | 2026-09 | Learned Motion Interface | 用全身机器人/物体关键帧连接 VLM 规划和 RL 控制 | 语言 + 场景 + 反馈 → 场景重定向后的关键帧序列 | keyframe-conditioned 全身策略 | ✅ Unitree G1 | 项目：TBD · [arXiv](https://arxiv.org/abs/2609.18869) · GitHub：TBD |
| Sample, Simulate, Select | 2026-09 | Generative Motion Prior | 采样 text-to-motion 候选并通过 controller-in-the-loop 物理 rollout 排序 | 文本提示 → 重定向候选 → 最可执行全身参考 | SONIC tracking policy | ✅ Unitree G1 | 项目：TBD · [arXiv](https://arxiv.org/abs/2609.26420) · GitHub：TBD |
| WholeBodyWAM: Learning Whole-Body World Action Models with Scalable Motion Priors | 2026-09 | Generative Motion Prior | 在 4000+ 小时数据上预训练语言条件 Motion Expert，预测统一空间中的未来运动 | 语言 + 视觉/机器人上下文 → 未来全身运动先验 → embodiment action | Video、Motion、Action Experts 联合执行 | ✅ Humanoid 真机 | 项目：TBD · [arXiv](https://arxiv.org/abs/2609.18197) · GitHub：TBD |

## Generative Motion Prior

### DreamControl — 2025-09

- **表示/生成：** 使用任务与时空约束引导人体运动扩散模型，生成类人全身参考轨迹。
- **输入 → 输出：** 文本任务和稀疏时空目标 → 全身人体参考运动。
- **执行：** 在仿真中利用生成先验训练任务级 RL 策略，再迁移到真机。
- **真机：** Unitree G1。
- **链接：** [项目](https://genrobo.github.io/DreamControl/) · [arXiv](https://arxiv.org/abs/2509.14353) · [代码](https://github.com/GenRobo/DreamControl)

### DreamControl-v2 — 2026-04

- **表示/生成：** 在人类和机器人运动数据构成的 canonical robot-motion space 中训练 transformer diffusion model。
- **输入 → 输出：** 文本和时空约束 → 机器人空间全身参考轨迹。
- **执行：** 生成参考用于监督下游 RL skill policy。
- **真机：** Unitree G1。
- **链接：** 项目：TBD · [arXiv](https://arxiv.org/abs/2604.00202) · 代码：TBD

### Humanoid-DART — 2026-06

- **表示/生成：** goal-conditioned 双分支扩散模型联合生成机器人与物体轨迹；迭代保存成功轨迹并重标注 near-miss。
- **输入 → 输出：** 目标和少量示范/elite rollout → goal-conditioned 机器人与物体参考轨迹。
- **执行：** RL motion tracker 在物理环境中执行和评价候选运动，成功 rollout 继续扩展训练集。
- **真机：** Unitree G1。
- **链接：** 项目：TBD · [arXiv](https://arxiv.org/abs/2606.26855) · 代码：TBD

### TEXEDO — 2026-06

- **表示/生成：** 从预训练文本条件生成器采样运动，再用由 controller rollout 学得的动力学可行性 verifier 和语义 verifier 选择可执行参考。
- **输入 → 输出：** 文本提示 → 多个机器人运动候选 → 可行且任务一致的全身参考。
- **执行：** 全身控制器跟踪选中运动；论文报告了 G1 真机部署。
- **链接：** 项目：TBD · [arXiv](https://arxiv.org/abs/2606.22998) · 代码：TBD

### Sample, Simulate, Select — 2026-09

- **表示/生成：** 从冻结的 text-to-motion 模型采样，重定向到 G1，并利用部署 tracker 的完整物理 rollout 对候选排序。
- **输入 → 输出：** 文本提示 → 重定向候选 → 经 controller 验证的参考运动。
- **执行：** SONIC 跟踪选中运动；通过硬件筛选的 177 个片段全部在真实 G1 上执行。
- **链接：** 项目：TBD · [arXiv](https://arxiv.org/abs/2609.26420) · 代码：TBD

### WholeBodyWAM: Learning Whole-Body World Action Models with Scalable Motion Priors — 2026-09

- **表示/生成：** 在 UniMotion-4K 上预训练语言条件 Motion Expert，在规范化全身空间预测未来运动。
- **输入 → 输出：** 语言和多模态机器人上下文 → 未来全身运动 → embodiment-specific action。
- **执行：** robot post-training 中通过 asymmetric mixture-of-transformers attention 融合 Video、Motion 和 Action Experts。
- **范围说明：** 它不同于 2026-06 的 MotionWAM，也不同于另一篇 WBC-grounded WholeBodyWAM（arXiv:2609.16644）。
- **链接：** 项目：TBD · [arXiv](https://arxiv.org/abs/2609.18197) · 代码：TBD

## Motion Planning / Optimization

### Opt2Skill — 2024-09 预印本 / 2025 发表

- **表示/生成：** DDP 生成带状态、控制和力矩参考的动力学可行接触式全身轨迹。
- **输入 → 输出：** 任务和接触约束 → 全身最优轨迹。
- **执行：** RL 策略模仿离线轨迹并预测关节位置目标。
- **真机：** Digit。
- **链接：** [项目](https://opt2skill.github.io/) · [arXiv](https://arxiv.org/abs/2409.20514) · 代码：TBD

### Physically Consistent Humanoid Loco-Manipulation using Latent Diffusion Models — 2025-04

- **表示/生成：** 从 latent diffusion 生成的人–物交互图像中提取候选接触和机器人构型，再用于初始化全身轨迹优化。
- **输入 → 输出：** 场景图像和任务分解/提示 → 接触/构型 → 物理一致的全身运动。
- **执行：** 全身轨迹优化施加运动学与动力学约束。
- **真机：** 论文仅报告仿真。
- **链接：** 项目：TBD · [arXiv](https://arxiv.org/abs/2504.16843) · 代码：TBD

### Task and Motion Planning for Humanoid Loco-manipulation — 2025-08

- **表示/生成：** 将接触模式变化作为 symbolic action，统一 task、contact 和 whole-body motion planning。
- **输入 → 输出：** 任务描述与机器人/物体/环境约束 → 长时序、动力学可行的 loco-manipulation 计划。
- **执行：** 轨迹优化显式包含全身动力学、执行器限制和交互约束。
- **真机：** 此处尚未确认物理硬件实验。
- **链接：** 项目：TBD · [arXiv](https://arxiv.org/abs/2508.14099) · 代码：TBD

### Learning Differentiable Reachability Maps for Optimization-based Humanoid Motion Generation — 2025-08

- **表示/生成：** 学习标量 reachability field，为连续优化提供可微 task-space 约束。
- **输入 → 输出：** 运动学样本与任务/接触目标 → 落脚、多接触或 loco-manipulation 运动。
- **执行：** 连续优化生成运动学可达的 humanoid 轨迹。
- **真机：** 此处尚未确认物理硬件实验。
- **链接：** 项目：TBD · [arXiv](https://arxiv.org/abs/2508.11275) · 代码：TBD

### MotionDisco — 2026-06

- **表示/生成：** LLM 引导的进化搜索生成离散接触程序，顺序 kinodynamic optimizer 将其转化为完整运动。
- **输入 → 输出：** 语言目标和可行性反馈 → 接触计划 → 动力学可行全身轨迹。
- **执行：** RL policy 在真机上跟踪发现的参考轨迹。
- **真机：** Humanoid 真机。
- **链接：** [项目](https://atarilab.github.io/motiondisco.io/) · [arXiv](https://arxiv.org/abs/2606.06139) · 代码：TBD

### HIGenNTO — 2026-09

- **表示/生成：** 在稀疏时空、接触、碰撞和支撑约束下，优化预训练文本条件运动模型的初始噪声。
- **输入 → 输出：** 任务/场景程序和约束 → 真实且物理落地的人形–场景交互参考。
- **执行：** tracking policy 执行参考；生成数据还用于训练机载深度条件视觉运动策略。
- **真机：** Unitree G1。
- **链接：** 项目：TBD · [arXiv](https://arxiv.org/abs/2609.22611) · 代码：TBD

### OCLO — 2026-10

- **表示/生成：** 从稀疏末端命令出发，以解析可达性先验在线生成骨盆高度和躯干方向，并可用 policy-in-the-loop sampling 继续优化。
- **输入 → 输出：** 两个 EE 目标和交互力 → 顺应性 EE 参考与动态选择的全身姿态。
- **执行：** 冻结的下肢 RL policy 配合 differential IK；可选 CEM 通过冻结策略 rollout 评价候选姿态。
- **真机：** Unitree G1。
- **链接：** [项目](https://oclo-humanoid.github.io/) · [arXiv](https://arxiv.org/abs/2610.05678) · 代码：TBD

## Learned Motion Interface

### HuMI — 2026-02

- **表示/生成：** 便携 robot-free 硬件采集人体全身运动、腕部观测和操作状态，供分层策略学习。
- **输入 → 输出：** 人类示教观测和运动 → 可执行 humanoid 全身技能。
- **执行：** 高层 diffusion policy 向底层全身控制器提供全身目标。
- **真机：** 五项任务的真实 humanoid 实验。
- **链接：** [项目](https://humanoid-manipulation-interface.github.io/) · [arXiv](https://arxiv.org/abs/2602.06643) · 代码：TBD

### BifrostUMI — 2026-05

- **表示/生成：** UMI 式夹爪和轻量 VR 设备记录腕部 RGB、夹爪动作以及骨盆/腕/脚稀疏关键点。
- **输入 → 输出：** robot-free demonstration → 未来关键点 → 机器人原生全身参考。
- **执行：** spatial keypoint retargeting 和全身控制器执行参考。
- **真机：** Unitree G1，五个真实场景。
- **链接：** [项目](https://baai-aether.github.io/BifrostUMI/) · [arXiv](https://arxiv.org/abs/2605.03452) · 代码：TBD

### WT-UMI — 2026-06

- **表示/生成：** 可穿戴触觉接口记录触觉图像、接触力和末端位姿；force-supervised planner 预测位姿块和力轨迹。
- **输入 → 输出：** 多模态接触示教 → 末端位姿与力参考。
- **执行：** 预测力作为触觉 admittance controller 的参考。
- **真机：** 真实 humanoid 接触式操作；同时支持人体示教和 teleoperation。
- **链接：** [项目](https://wt-umi.github.io/WTUMI/) · [arXiv](https://arxiv.org/abs/2606.13232) · 代码：TBD

### ReactiveBFM — 2026-06

- **表示/生成：** 生成式 planner 从有误差的物理状态产生 spatial-reference chunks，并异步重规划。
- **输入 → 输出：** 文本命令和当前机器人状态 → 反应式全身参考轨迹。
- **执行：** 预训练 BFM tracker 跟踪经过时间集成的轨迹块。
- **真机：** Unitree G1。
- **链接：** [项目](https://xiao-chen.tech/) · [arXiv](https://arxiv.org/abs/2606.30362) · 代码：TBD

### VisualMimic — 2025-09

- **表示/生成：** 任务级 visuomotor policy 预测 root、双手、双脚和头部稀疏关键点。
- **输入 → 输出：** 第一视角图像和本体状态 → 全身关键点目标。
- **执行：** teacher-student 蒸馏训练的通用 keypoint tracker。
- **真机：** Unitree G1。
- **链接：** [项目](https://visualmimic.github.io/) · [arXiv](https://arxiv.org/abs/2509.20322) · [代码](https://github.com/visualmimic/VisualMimic)

### REFINE-DP — 2026-03

- **表示/生成：** transformer diffusion policy 规划 base velocity 和 hand pose command chunks，并通过 PPO 与控制器联合微调。
- **输入 → 输出：** 任务/机器人状态和示范历史 → 高层运动命令。
- **执行：** 下层 RL loco-manipulation controller 跟踪手部和落脚命令，并与 planner 共同适应。
- **真机：** Booster T1。
- **链接：** [项目](https://refine-dp.github.io/REFINE-DP/) · [arXiv](https://arxiv.org/abs/2603.13707) · 代码：TBD

### MotionWAM — 2026-06

- **表示/生成：** 统一 motion latent 解码为涵盖行走、躯干、高度、脚部交互和手部操作的 motion tokens。
- **输入 → 输出：** 单目第一视角视频，经视频世界模型中间去噪特征 → 全身 motion tokens。
- **执行：** 实时 world-action policy 在机器人上闭环运行。
- **真机：** Unitree G1。
- **链接：** [项目](https://dit4dit.github.io/MotionWAM/) · [arXiv](https://arxiv.org/abs/2606.09215) · 代码：TBD

### Whole-Body UMI — 2026-09

- **表示/生成：** 独立从 retargeted mocap 学习的 task-agnostic、EE-conditioned 全身 motion generator。
- **输入 → 输出：** 腕部相机观测 → diffusion-policy EE 轨迹 → 全身参考运动。
- **执行：** 异步分层系统结合 manipulation diffusion policy、WB-UMI generator、延迟补偿、状态反馈和 WBC。
- **真机：** Unitree G1。
- **链接：** [项目](https://wholebody-umi.github.io/) · [arXiv](https://arxiv.org/abs/2609.22829) · 代码：TBD

### KINO — 2026-09

- **表示/生成：** 每个中间关键帧定义全身机器人姿态，并在需要时同时定义物体姿态。
- **输入 → 输出：** 语言、场景观测和执行反馈 → 选择并按当前场景重定向的关键帧。
- **执行：** keyframe-conditioned 全身 RL policy 输出到达各目标的关节动作。
- **真机：** Unitree G1。
- **链接：** 项目：TBD · [arXiv](https://arxiv.org/abs/2609.18869) · 代码：TBD

### λ₀ / HumanVerse-500 — 2026-09

- **表示/生成：** 三阶段全身 VLA 使用共享的人/机器人表示；人体运动中间训练包含 G1-compatible motion latent 与灵巧手命令。
- **输入 → 输出：** 第一视角视频、语言以及身体/手部历史 → embodiment-specific humanoid 全身动作。
- **执行：** interaction pretraining → whole-body human-motion mid-training → 任务级 robot post-training。
- **真机：** Unitree G1。
- **范围说明：** 它是集成式 VLA，而不是独立部署的 motion generator + tracker，因此属于列表的边界条目。
- **链接：** 项目：TBD · [arXiv](https://arxiv.org/abs/2610.00438) · 代码/数据：已宣布发布，链接 TBD

### Workhorse — 2026-10

- **表示/生成：** flow-matching visual planner 预测 torso、双腕和双脚共 five-link target chunks。
- **输入 → 输出：** 4 张第一视角图像和约 1 秒 five-link 历史 → 1.16 秒 five-link 目标轨迹，以 5 Hz 重规划。
- **执行：** RL whole-body tracker 读取轨迹的短时窗口并控制全身；planner 和 tracker 使用同一批 robot-free human demonstrations 分别训练。
- **真机：** Unitree G1；展示了手脚协同箱子分类、接箱以及推倒/攀爬行李箱。
- **链接：** [项目](https://hybridrobotics.github.io/workhorse/) · [arXiv](https://arxiv.org/abs/2610.09117) · 代码：TBD

## Synthetic Motion / Data Generation

### OmniRetarget — 2025-09

- **表示/生成：** interaction mesh 在重定向时保持 agent、地形和操作物体之间的空间与接触关系。
- **输入 → 输出：** 人体运动与目标机器人/地形/物体配置 → 运动学可行、保持交互的轨迹。
- **执行：** 简单本体感知 RL policy 跟踪生成的长时序运动。
- **真机：** Unitree G1。
- **链接：** [项目](https://omniretarget.github.io/) · [arXiv](https://arxiv.org/abs/2509.26633) · 代码：TBD

### GenMimic — 2025-12

- **表示/生成：** 将生成视频提升为 4D 人体运动，重定向到 humanoid，并表示成 3D 关键点轨迹。
- **输入 → 输出：** 生成人体视频 → 4D 人体表示 → 机器人关键点参考。
- **执行：** 使用 keypoint-weighted reward 的物理感知 RL tracker 执行含噪生成运动。
- **真机：** Unitree G1。
- **链接：** [项目](https://genmimic.github.io/) · [arXiv](https://arxiv.org/abs/2512.05094) · 代码：TBD

### Imagine2Real — 2026-05

- **表示/生成：** 将机器人和物体运动表示为 base、双手和物体的稀疏 4D point trajectories。
- **输入 → 输出：** 生成 HOI 视频 → 稀疏交互关键点 → 机器人交互参考。
- **执行：** 关键点 tracker 在 behavior-foundation-model latent space 中搜索，以保留自然步态。
- **真机：** 在动捕环境中进行了 zero-shot 物理 humanoid 部署。
- **链接：** 项目：TBD · [arXiv](https://arxiv.org/abs/2605.22272) · 代码：TBD

### GenHOI — 2026-06

- **表示/生成：** 生成视频提供接触事件和物体中心几何约束；恢复的运动针对尺度、接触和新相对位姿继续优化。
- **输入 → 输出：** 语言与重建场景 → 交互视频/接触线索 → 优化的机器人–物体轨迹。
- **执行：** 闭环 tracking controller 执行修正后的参考。
- **真机：** 真实 humanoid 实验。
- **链接：** 项目：TBD · [arXiv](https://arxiv.org/abs/2606.12995) · 代码：TBD

### VLK — 2026-06

- **表示/生成：** 在重建场景中合成同步的第一视角图像、语言命令和机器人兼容运动学轨迹。
- **输入 → 输出：** 语言和第一视角观测 → 短时全身运动学轨迹。
- **执行：** 全身 tracker 将预测轨迹映射为物理动作。
- **真机：** Unitree G1。
- **链接：** [项目](https://vision-language-kinematics.github.io/) · [arXiv](https://arxiv.org/abs/2606.30645) · 代码：TBD

### RoboReact — 2026-08

- **表示/生成：** 从生成第一视角视频重建保持几何关系的 3D 交互关键帧，并重定向到 humanoid。
- **输入 → 输出：** 单个 RGB-D 观测 → 想象的操作视频 → 修正后的全身技能。
- **执行：** 在线物体中心重定位和 VLM 引导 refinement loop 向全身控制器提供目标。
- **真机：** 真实 humanoid 机器人。
- **链接：** 项目：TBD · [arXiv](https://arxiv.org/abs/2608.03387) · 代码：TBD

### HumanoidMimicGen — 2026-05

- **表示/生成：** 将少量源示范中的接触式全身技能适配到新物体状态，并与全身 locomotion/manipulation planning 交错组合。
- **输入 → 输出：** 源示范 + 新场景/物体配置 → 稳定、无碰撞的 state/action demonstrations。
- **执行：** WBC 执行生成轨迹，下游视觉策略使用生成和真实数据联合训练。
- **真机：** Unitree G1；生成数据提升了论文报告的真实策略表现。
- **链接：** [项目](https://humanoidmimicgen.github.io/) · [arXiv](https://arxiv.org/abs/2605.27724) · [代码](https://github.com/NVlabs/humanoidmimicgen)

### GRAIL — 2026-06

- **表示/生成：** 视频基础模型在已知 3D 场景中生成交互视频，再恢复 SMPL-X 人体、手部与物体 6-DoF 轨迹并 retarget 到 G1。
- **输入 → 输出：** 3D 物体和场景 → 合成交互视频 → metric 4D HOI → 机器人轨迹/动作。
- **执行：** 使用生成数据训练通用 manipulation/locomotion tracker 和第一视角 RGB policy。
- **真机：** Unitree G1。
- **链接：** [项目](https://research.nvidia.com/labs/dair/grail/) · [arXiv](https://arxiv.org/abs/2606.05160) · [代码](https://github.com/NVlabs/GRAIL)

### SUGAR — 2026-05

- **表示/生成：** 自动视频重建提取人/物运动和接触标签，privileged RL refiner 将不完美先验转为物理可执行技能。
- **输入 → 输出：** 非结构化人体视频 → kinematic interaction prior → refined robot skill 与短时命令。
- **执行：** 将技能蒸馏进 command generator + command tracker；推理时无需参考运动。
- **真机：** Humanoid 真机。
- **链接：** [项目](https://tianshuwu.github.io/sugar-humanoid/) · [arXiv](https://arxiv.org/abs/2605.20373) · [代码](https://github.com/tianshuwu/SUGAR)

### InterMimicGen — 2026-10

- **表示/生成：** 从异构人–物 motion 数据进行接触保持 retargeting，再迭代进行任务保持的空间和身体运动扰动。
- **输入 → 输出：** mocap 人–物交互 → humanoid reference → 持续扩大的物理可执行机器人运动集。
- **执行：** 通用 physics tracker 执行候选运动；通过任务检查的变体进入下一轮训练并继续生成。
- **真机：** 报告了真实 humanoid 迁移。
- **链接：** [项目](https://sirui-xu.github.io/InterMimicGen/) · [arXiv](https://arxiv.org/abs/2610.06850) · 代码：TBD

## 贡献

欢迎补充论文和项目。请参阅 [CONTRIBUTING.md](CONTRIBUTING.md)。新增条目至少应包含可验证的标题、年份、链接和所属类别；无法核实的链接或结论请标记为 `TBD`，不要猜测。

## 许可证

本仓库使用 [MIT License](LICENSE)。

