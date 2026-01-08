```plantUML
@startuml MathMate Data Model (Revised)

' 知识体系模型
class KnowledgeNode {
  + node_id: UUID <<PK>>
  + parent_node_id: UUID <<FK>>
  + name: String
  + chapter: String
  + topic: String
  + content: Text
  + is_core_concept: Boolean
  + prerequisite_nodes: JSON
}

class KnowledgeState {
  + state_id: UUID <<PK>>
  + knowledge_node_id: UUID <<FK>>
  + mastery_level: Integer
  + is_weak_point: Boolean
  + last_reviewed_at: DateTime
  + review_count: Integer
  + next_review_at: DateTime
  + evidence_count: Integer
  + confidence_score: Float
}

' 对话与交互模型
class Conversation {
  + conversation_id: UUID <<PK>>
  + started_at: DateTime
  + ended_at: DateTime
  + main_topic: String
  + identified_nodes: JSON
  + sentiment_trend: JSON
}

class Message {
  + message_id: UUID <<PK>>
  + conversation_id: UUID <<FK>>
  + sender: Enum
  + content: Text
  + input_type: Enum
  + raw_input: String
  + timestamp: DateTime
  + is_user_question: Boolean
  + is_ai_guidance: Boolean
  + guidance_strategy: String
}

class QuestionSession {
  + session_id: UUID <<PK>>
  + conversation_id: UUID <<FK>>
  + user_question: Text
  + knowledge_nodes: JSON
  + guidance_rounds: Integer
  + resolution_type: Enum
  + final_understanding: Boolean
  + created_at: DateTime
}

' 题目与资源模型
class QuestionBank {
  + question_id: UUID <<PK>>
  + knowledge_node_id: UUID <<FK>>
  + type: Enum
  + difficulty: Integer
  + content: Text
  + options: JSON
  + correct_answer: String
  + explanation: Text
  + image_url: String
  + usage_count: Integer
  + success_rate: Float
}

class AIGeneratedQuestion {
  + generated_id: UUID <<PK>>
  + conversation_id: UUID <<FK>>
  + knowledge_node_id: UUID <<FK>>
  + prompt_context: Text
  + question_content: Text
  + options: JSON
  + correct_answer: String
  + generation_timestamp: DateTime
  + user_response: String
  + is_correct: Boolean
}

class MicroCourseLink {
  + link_id: UUID <<PK>>
  + knowledge_node_id: UUID <<FK>>
  + title: String
  + url: String
  + platform: String
  + duration: Integer
  + description: Text
  + recommended_count: Integer
}

' 学习诊断模型
class ErrorPattern {
  + pattern_id: UUID <<PK>>
  + knowledge_node_id: UUID <<FK>>
  + error_description: Text
  + typical_mistakes: JSON
  + root_cause_analysis: Text
  + identified_at: DateTime
}

class LearningEvent {
  + event_id: UUID <<PK>>
  + conversation_id: UUID <<FK>>
  + event_type: Enum
  + knowledge_node_id: UUID <<FK>>
  + description: Text
  + impact_on_mastery: Integer
  + timestamp: DateTime
  + ai_observation: Text
}

' 激励系统模型
class PointLog {
  + log_id: UUID <<PK>>
  + points_change: Integer
  + reason: String
  + conversation_id: UUID <<FK>>
  + ai_comment: Text
  + timestamp: DateTime
}

class GameProgress {
  + current_points: Integer
  + current_level: Integer
  + level_title: String
  + achievements: JSON
  + last_updated: DateTime
}

class RewardVoucher {
  + voucher_id: UUID <<PK>>
  + points_spent: Integer
  + reward_description: String
  + qr_code: String
  + verification_code: String
  + is_redeemed: Boolean
  + created_at: DateTime
  + redeemed_at: DateTime
}

' 复习与推送模型
class ReviewQueue {
  + queue_id: UUID <<PK>>
  + knowledge_node_id: UUID <<FK>>
  + scheduled_at: DateTime
  + review_type: Enum
  + priority: Integer
  + is_completed: Boolean
  + completed_at: DateTime
}

class AIRecommendation {
  + recommendation_id: UUID <<PK>>
  + knowledge_node_id: UUID <<FK>>
  + recommendation_type: Enum
  + content_id: UUID
  + reason: Text
  + priority: Integer
  + created_at: DateTime
  + is_presented: Boolean
  + user_feedback: String
}

' 关系定义
KnowledgeNode "1" -- "0..*" KnowledgeNode : parent-child
KnowledgeNode "1" -- "1" KnowledgeState
KnowledgeNode "1" -- "0..*" QuestionBank
KnowledgeNode "1" -- "0..*" AIGeneratedQuestion
KnowledgeNode "1" -- "0..*" MicroCourseLink
KnowledgeNode "1" -- "0..*" ErrorPattern
KnowledgeNode "1" -- "0..*" LearningEvent
KnowledgeNode "1" -- "0..*" ReviewQueue
KnowledgeNode "1" -- "0..*" AIRecommendation

Conversation "1" -- "0..*" Message
Conversation "1" -- "0..*" QuestionSession
Conversation "1" -- "0..*" AIGeneratedQuestion
Conversation "1" -- "0..*" LearningEvent
Conversation "1" -- "0..*" PointLog

GameProgress "1" -- "0..*" PointLog
GameProgress "1" -- "0..*" RewardVoucher

@enduml

```

核心设计调整说明
1. 没有用户表
所有表都没有 user_id 外键，因为只有一个用户使用。GameProgress 表成为单例表，只有一条记录。

2. AI 驱动的出题机制

**AIGeneratedQuestion 表**

- 记录 AI 在对话中实时生成的题目
- 保存生成上下文（prompt_context），便于 AI 学习改进
- 记录用户响应和正确性，积累数据

**QuestionBank 表**

- 不是 AI 随机选题的来源
- 作为参考题库：AI 可以分析题目结构、难度分布、高质量解析等
- usage_count 和 success_rate 字段可以让 AI 了解题目质量，但不直接选择

3. 知识诊断的核心
**LearningEvent 表**

- 记录所有学习事件（提问、答题、复习等）
- ai_observation 字段：AI 对该事件的分析笔记
- impact_on_mastery 字段：该事件对掌握度的影响权重
- KnowledgeState 表新增字段
- evidence_count：有多少次证据支撑当前评估
- confidence_score：AI 对该知识点掌握度评估的置信度

**ErrorPattern 表**

- 不再关联特定错题，而是归纳知识点的典型错误模式
- AI 可以识别"用户在二次函数求根时总是忘记判别式"这类规律

4. AI 灵活掌控积分
**PointLog 表**

- ai_comment 字段：AI 可以记录为什么给这些积分（"今天思路特别清晰，奖励 5 分"）
- reason 字段：简单分类（ask_question/answer_correct/creative_thinking）

**GameProgress 单例表**

- 不是从交易表累加，而是 AI 直接更新
- achievements 用 JSON 存储，AI 可以灵活定义新成就

5. 智能推荐系统
**AIRecommendation 表**

- AI 决定推荐什么：题目/微课/复习内容
- reason 字段：AI 解释推荐理由（"你三角函数还不够熟练，建议看这个微课"）
- user_feedback 字段：用户可以反馈推荐是否有用，帮助 AI 改进

**MicroCourseLink 表**

- 存储外部微课链接
- recommended_count 统计推荐次数，帮助 AI 了解哪些资源受欢迎

与需求的对应关系
| 需求编号       | 实现方式                                                                  |
| ---------- | --------------------------------------------------------------------- |
| F-P1/P2    | QuestionSession 记录引导轮次；Message 表区分引导类型                                |
| F-P3       | Conversation 的 sentiment_trend；QuestionSession 的 guidance_rounds 触发托底 |
| F-P4       | KnowledgeState + LearningEvent + ErrorPattern 构建动态诊断                  |
| F-P5       | AIRecommendation 表实现靶向推送                                              |
| F-P6       | Conversation 和 QuestionSession 保留完整历史                                 |
| F-P7       | ReviewQueue 表实现艾宾浩斯复习                                                 |
| F-M1/M2/M3 | PointLog（AI 灵活评分）+ GameProgress（可视化）+ RewardVoucher                   |