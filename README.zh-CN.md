# Pro Motion · 用 Claude Code 做"音频先行"的 MG 动画片

**给 Claude 一个产品(带上曲子，或者让它先帮你策划曲子),它会交给你一支有设计感、不是模板的发布片、品牌片或 TVC。片子用代码渲染，每个动作都卡在拍子上，几分钟出片。**

<table><tr>
<td align="center"><img src="docs/showcase-launch.gif" width="260"><br><sub><b>发布片</b>:产品界面、会表演的效果、5 种语言版本</sub></td>
<td align="center"><img src="docs/showcase-brand.gif" width="260"><br><sub><b>品牌片</b>:强烈风格化，桌面物件变乐器</sub></td>
<td align="center"><img src="docs/demo.gif" width="260"><br><sub><b>起步模板</b>:安装后自带的 8 秒演示</sub></td>
</tr></table>

<sub>以上为用这个 skill 给 <b>Tutti</b>(年轻人的照片玩法 App)做的片子。完整版在 <a href="https://github.com/jack-reson8/pro-motion/releases">最新 Release</a> 里。</sub>

现在大部分"AI 视频"要么是模板，要么是没法精确控制的生成片段。Pro Motion 走第三条路：先把音乐当成营销资产来策划，再让 Claude 把片子写成代码——画布上一个确定性的 `render(t)`。
这样做的好处是：
- 每个动作都能精确卡到十六分音符;
- 每个转场都是设计出来的;
- 任何一个镜头都能按时间点修改，几秒钟重新渲染。

它是从一个真实项目里沉淀出来的方法。那个项目改了大约十轮，被否掉的原因包括"像 PPT""素材丑""字浮在画面上""我说的是风格化，不是波普"。**起作用的是方法，所以这个 skill 沉淀的就是方法。**

## 它做什么

| 步骤 | Claude 做的事 |
|---|---|
| 1 · 需求 | 定下产品、受众、市场、音乐、画幅、片子类型;缺的信息给出合理默认值，不反复追问 |
| 1b · 音频先行 | 先想清楚传播机制(洞察、冲突、玩法、渠道),再出带听觉 logo 和记忆点的音频方案，写一条精确的 Suno/Udio 提示词，最后用 `audio_post.py` 精修:卡拍音效、结巴、循环、响度 |
| 2 · 音乐 | `analyze_music.py` 算出速度、节拍网格、重音、高低频打点，并画出带网格的频谱图。频谱图就是剪辑表 |
| 3 · 调研 | 找到官方视频，用 yt-dlp 拉 24 帧**联系表**(不用开浏览器),给每个参考写一份**技法卡** |
| 4 · 方向 | 用具体技法组合出**三个明显不同的风格帧**,你选定之后才开始做动画 |
| 5 · 叙事 | 一个**主角**贯穿每个场景，在每场里变形;每个功能在音乐节点上自己"表演";每个转场都有设计;文字本身就是动画;结尾长成 logo |
| 6 · 制作 | `motion-lib.js`:缓动和弹簧、挤压拉伸、每秒 12 帧的手绘线抖动、笔写文字、黏液融合、半调网点、条纹、3D 卡片环、贴纸描边、人像抠图(macOS Vision)、多语言切换 |
| 7 · 渲染 | `render-fast.js`:多个无头浏览器标签并行出帧，再一次合成音频。20 秒 60 帧约 30 秒出片;之后抽帧检查画面、检查音频长度 |
| 8 · 迭代 | 反馈按时间点来;审美层面的反馈退回调研和定方向，不在原片上打补丁 |

## 安装

**作为 Claude Code 插件(推荐)**

```bash
claude plugin marketplace add jack-reson8/pro-motion
claude plugin install pro-motion@pro-motion
```

**作为普通 skill**

```bash
git clone https://github.com/jack-reson8/pro-motion
cp -R pro-motion/plugins/pro-motion/skills/pro-motion ~/.claude/skills/
```

环境要求：
- Node 18 以上;
- Python 3,并装好 numpy、scipy、matplotlib;
- Chrome 或 Chromium(会自动查找，也可以用 `CHROME=/路径` 指定);
- 人像抠图需要 macOS 14 以上。

其余依赖由 `scripts/setup.sh` 按项目自动安装。

## 怎么用

直接说需求，并附上曲子：

> 用 `~/Downloads/track.mp3` 给我的修图 App 做一支 20 秒的发布片，竖屏，英文主版本，再出日文和韩文版。

> 用这首歌给我的品牌做一支强烈风格化的品牌片，先给我三个风格方向的风格帧。

> 8 到 10 秒那段像幻灯片，把那里的转场重新设计一下。

## 核心原则

1. **音乐就是剪辑表。** 先读频谱，再让每个动作落在节点上。
2. **看真实的帧，不凭记忆设计。**
3. **用风格帧选方向。** 不要选第一个想到的风格，也不要选最好写代码的风格。
4. **一个主角、会表演的效果、设计过的转场。** 少了任何一样都会像 PPT。
5. **文字是动画的一部分。** 不用下方字幕条、标签胶囊和框。
6. **素材丑，成片一定丑。** 用真实、当下、版权安全的素材;找不到就留白。
7. **几秒出片，查一张联系表，交付。** 之后按时间点迭代。

## 许可

代码和文档采用 MIT 协议。内置字体(Patrick Hand、Yomogi、Gaegu、站酷快乐体)采用 SIL OFL 1.1 协议。用 `ref_frames.sh` 下载的参考视频仅供个人学习，请勿再分发。
