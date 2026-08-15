# 课程视频解析 — 交接说明(HANDOFF)

> 本文件记录"面向绩效编程"课程视频解析任务的当前状态,供新会话继续使用。

## 一、已完成:45 个视频全部转写并生成 md

课程根目录:`/Users/xuzhenxin/Downloads/已加速- 面向绩效编程，程序员的向上管理法/`

- 共 9 章、45 个视频,已全部生成对应的 md 笔记,存放在**各章节目录内、与视频同目录**。
- 各章 md 数量:
  - 第1章(课程介绍与学习指南):4
  - 第2章(你还没听说过面向绩效编程?):3
  - 第3章(用绩效点石成金的秘密):6
  - 第4章(你的绩效为什么不好?):6
  - 第5章(勇攀高峰:改善你的绩效):6
  - 第6章(可进可退:为你的绩效上保险):4
  - 第7章(更好的发展,需要突破你的瓶颈):10
  - 第8章(渡人渡己,提高团队绩效):4
  - 第9章(课程谢幕):2

## 二、md 文档是怎么来的

1. **本地 Whisper 语音转写**(small 模型,中文),无需 API key
2. **zhconv 简体转换**(原始转写是繁体/夹杂)
3. **人工校对同音误识**:如 计校/技校/迹效→绩效、掌心→涨薪、吹子/崔子/Trace→TRIZ、步道师→布道师、平静→瓶颈、破装校园→破窗效应 等
4. **结构化整理**:按视频讲解顺序整理成小标题/列表/表格

## 三、重要局限(已知问题)

- **画面帧无法读取**:本环境 Read 工具对所有图片返回 `[Unsupported Image]`(已用 64×64 PNG 验证,是环境/配置限制,换模型也无效)。
- 因此 **md 只覆盖了口头讲授内容**,漏掉了"只出现在 PPT 上但没被念出来"的图表/代码/板书。
- 校对是"尽力而为",个别拿不准处保留了转写原样或加了注释。

## 四、临时文件位置(可复用/可清理)

- 第2-9章转写原始 json(41 个):
  `/tmp/course_watch/batch/第X章.../第X章.../xxx.json`
  同目录还有 `.txt`(带时间戳繁体)和 `_s.txt`(简体)。
- 第1章转写 + 提取脚本 + 测试帧:
  `/tmp/course_watch/`(含 `1-2..._s.txt`、`extract.py`、`to_simplified.py`、`batch_transcribe.sh`、`test_frame.jpg/png` 等)
- 视频帧(当时每30秒抽一帧,未用上):
  `/tmp/course_watch/frames_1_2/` … `/tmp/course_watch/frames_1_5/`

## 五、如果新会话要"带画面重新整理"

前提:新会话的 Read 能读图片(换到 Claude Desktop / claude.ai/code 网页版等支持图像的环境)。

步骤参考:
1. 用 ffmpeg 从视频抽帧(如每 20-30 秒一帧,或按 PPT 切换抽):
   ```bash
   ffmpeg -y -i "视频.mp4" -vf "fps=1/20,scale=1024:-2" -q:v 4 out/f_%03d.jpg
   ```
2. Read 每一帧,结合已有 `_s.txt` 简体转写文本对照
3. 补充/修正 md 里 PPT 相关图表、代码、列表内容

## 六、工具/依赖清单

- `whisper`(pipx 装的 openai-whisper):`/Users/xuzhenxin/.local/bin/whisper`
  - 模型缓存:`~/.cache/whisper/`(small.pt、tiny.pt)
  - 转写命令示例:
    ```bash
    whisper "视频.mp4" --model small --language zh --task transcribe --output_format json --output_dir 输出目录 --verbose False
    ```
- `zhconv`(已装到系统 python3,用于繁→简)
- `ffmpeg` / `ffprobe`(系统已装)

## 七、清理建议

若不再需要临时文件,可删除:
```bash
rm -rf /tmp/course_watch
```
（md 文档不受影响,已独立存放在课程目录里）
