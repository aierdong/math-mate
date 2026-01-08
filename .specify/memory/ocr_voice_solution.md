# OCR 和语音识别技术方案

本文档详细说明 Math Mate 项目中使用的 OCR（光学字符识别）和语音识别技术方案。

## 概述

Math Mate 支持多模态输入，包括：
- **语音输入**：用户可以通过语音方式输入数学问题（口语化数学表达）
- **图像输入**：用户可以通过拍照上传数学题目图片

## 语音识别方案

### 方案选择策略

项目将使用两种大模型：**Gemini** 和 **QWen**。语音识别方案根据场景灵活选用，**优先使用阿里云方案**（便于日志记录，不依赖大模型的多模态输入能力）。

### 1. Gemini 语音识别

**适用场景**：当使用 Gemini 大模型时

**技术特点**：
- Gemini 本身支持语音输入，无需额外处理
- 直接将音频作为输入即可，由 Gemini 模型内部处理

**使用方式**：
- 将音频数据直接传递给 Gemini API
- 无需单独的语音识别服务

### 2. 阿里云 gummy-chat-v1 语音识别

**适用场景**：优先使用方案，适用于所有场景

**模型信息**：
- 模型名称：`gummy-chat-v1`
- 一句话识别/翻译模型，识别出一句话后结束任务
- 默认进行标点符号预测和逆文本正则化（INT，Inverse Text Normalization）
- 支持定制热词

**调用方式**：

#### 方式一：OpenAPI 兼容方式
- **适用场景**：上传录音文件
- **特点**：适合处理已录制的音频文件
- **文档**：[Gummy一句话识别、翻译Python SDK](https://help.aliyun.com/zh/model-studio/sentence-python-sdk)

#### 方式二：WebSocket 流式方式
- **适用场景**：用户使用麦克风实时输入
- **特点**：支持流式识别，实时返回结果
- **文档**：[Gummy一句话识别、翻译WebSocket API](https://help.aliyun.com/zh/model-studio/sentence-websocket-api?spm=a2c4g.11186623.help-menu-2400256.d_2_7_3_2.41e7589aJ57js7)

**技术限制**：
- 音频时长不能超过一分钟，否则将报错断连
- 一句话的结束通过静音时长判断（默认700ms，可通过 `max_end_silence` 参数设置）
- 如果语音时长超过一分钟，则认为这一分钟内的语音是一句话

**优势**：
- 不依赖大模型的多模态输入能力
- 便于记录日志和追踪
- 支持流式识别，用户体验好

## OCR 识别方案

### 方案选择策略

考虑到图片中可能含有数学公式，采用**主方案 + 补充方案**的组合策略。

### 1. 阿里云 Qwen-OCR 模型（主方案）

**适用场景**：基础图片识别

**技术特点**：
- 基于 Qwen 视觉模型的 OCR 能力
- 支持通用图片文字识别

**文档**：[Qwen-VL OCR API参考](https://help.aliyun.com/zh/model-studio/qwen-vl-ocr-api-reference?spm=a2c4g.11186623.help-menu-2400256.d_2_4_2.79a0256cr7T4m5&scm=20140722.H_2996283._.OR_help-T_cn~zh-V_1)

### 2. 阿里云教育识别场景（补充方案）

**适用场景**：识别图片中的数学公式

**技术特点**：
- 专门针对教育场景优化
- 支持数学公式识别

**API 文档**：
- [RecognizeEduFormula API](https://help.aliyun.com/zh/ocr/developer-reference/api-ocr-api-2021-07-07-recognizeeduformula?spm=5176.26934562.main.1.161521cazV8XNO)
- [教育公式识别](https://help.aliyun.com/zh/ocr/developer-reference/api-ocr-api-2021-07-07-recognizeeduformula?spm=a2c4g.11186623.help-menu-252763.d_3_2_4_6_0.2ede4f509IbfhI)

**使用策略**：
- 优先使用 Qwen-OCR 进行基础识别
- 如果识别结果包含数学公式或识别效果不佳，使用教育识别场景 API 进行补充识别

## 使用场景总结

### 语音识别场景

| 场景 | 推荐方案 | 调用方式 |
|------|---------|---------|
| 使用 Gemini 大模型 | Gemini 直接语音输入 | 音频直接传递给 Gemini API |
| 上传录音文件 | 阿里云 gummy-chat-v1 | OpenAPI 兼容方式 |
| 麦克风实时输入 | 阿里云 gummy-chat-v1 | WebSocket 流式方式 |

### OCR 识别场景

| 场景 | 推荐方案 | 说明 |
|------|---------|------|
| 普通文字识别 | Qwen-OCR | 基础图片识别 |
| 包含数学公式 | Qwen-OCR + 教育识别场景 | 先用 Qwen-OCR，必要时使用教育识别场景补充 |

## 相关链接

### 语音识别
- [Gummy一句话识别、翻译Python SDK](https://help.aliyun.com/zh/model-studio/sentence-python-sdk)
- [Gummy一句话识别、翻译WebSocket API](https://help.aliyun.com/zh/model-studio/sentence-websocket-api?spm=a2c4g.11186623.help-menu-2400256.d_2_7_3_2.41e7589aJ57js7)

### OCR 识别
- [Qwen-VL OCR API参考](https://help.aliyun.com/zh/model-studio/qwen-vl-ocr-api-reference?spm=a2c4g.11186623.help-menu-2400256.d_2_4_2.79a0256cr7T4m5&scm=20140722.H_2996283._.OR_help-T_cn~zh-V_1)
- [RecognizeEduFormula API](https://help.aliyun.com/zh/ocr/developer-reference/api-ocr-api-2021-07-07-recognizeeduformula?spm=5176.26934562.main.1.161521cazV8XNO)
- [教育公式识别](https://help.aliyun.com/zh/ocr/developer-reference/api-ocr-api-2021-07-07-recognizeeduformula?spm=a2c4g.11186623.help-menu-252763.d_3_2_4_6_0.2ede4f509IbfhI)

## 注意事项

1. **语音识别**：
   - 阿里云方案需要配置 API Key
   - 音频格式和采样率需要符合要求
   - 注意音频时长限制（不超过1分钟）

2. **OCR 识别**：
   - 图片格式和大小需要符合 API 要求
   - 数学公式识别可能需要多次调用或组合使用多个 API
   - 考虑识别准确率和成本平衡

3. **日志记录**：
   - 优先使用阿里云方案便于日志记录和问题追踪
   - 建议记录所有识别请求和结果，便于后续优化

**相关文档**: 技术栈详见 `tech-stack.md`

