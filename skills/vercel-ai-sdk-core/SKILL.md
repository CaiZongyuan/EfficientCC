---
name: vercel-ai-sdk-core
description: This skill should be used when users need to work with Vercel AI SDK core functionality for building AI-powered applications. It provides focused guidance on essential APIs including useChat for chatbot interfaces, generateText for non-interactive text generation, and streamText for real-time streaming responses.
---

# Vercel AI SDK Core

## 核心功能
提供Vercel AI SDK核心API的精简指南，专注于文本生成、流式响应和聊天机器人界面构建。

## 使用时机
This skill should be used when users need to work with Vercel AI SDK core functionality for building AI-powered applications. It provides focused guidance on essential APIs including useChat for chatbot interfaces, generateText for non-interactive text generation, and streamText for real-time streaming responses.

## 工作流程

### 文本生成流程
1. **非交互式生成**: 使用 `generateText` 创建完整文本响应
   - 适用于邮件草稿、文档摘要等场景
   - 一次性返回完整结果

2. **交互式流式生成**: 使用 `streamText` 实现实时响应
   - 适用于聊天界面、实时对话
   - 减少用户等待时间

### 聊天机器人构建流程
1. **设置useChat Hook**: 配置消息状态管理
2. **创建API路由**: 处理消息请求和流式响应
3. **构建UI组件**: 实现消息显示和输入界面
4. **错误处理**: 处理网络错误和API异常

## 关键API使用模式

### generateText 基础使用
```typescript
import { generateText } from 'ai';

const { text } = await generateText({
  model: yourModel,
  prompt: 'Write a professional email about project updates',
  temperature: 0.7,
});
```

### streamText 实时响应
```typescript
import { streamText } from 'ai';

const result = await streamText({
  model: yourModel,
  prompt: 'Explain quantum computing',
});

return result.toAIStreamResponse();
```

### useChat 聊天界面
```typescript
import { useChat } from 'ai/react';

const { messages, input, handleInputChange, handleSubmit } = useChat();

// 在组件中使用
return (
  <div>
    {messages.map(m => <div key={m.id}>{m.content}</div>)}
    <form onSubmit={handleSubmit}>
      <input value={input} onChange={handleInputChange} />
    </form>
  </div>
);
```

## 错误处理策略

### 常见错误场景
1. **网络超时**: 使用onError回调处理连接问题
2. **模型不可用**: 实现备用模型切换
3. **令牌限制**: 监控使用情况，提供优雅降级
4. **输入验证**: 验证用户输入，防止恶意请求

### 错误处理模式
```typescript
const { text, error } = await generateText({
  model: yourModel,
  prompt: userPrompt,
  onError: (error) => {
    console.error('Generation failed:', error);
    return null;
  }
});
```

## 性能优化

### 流控制
- 使用 `experimental_generateText` 进行预览生成
- 实现客户端缓存减少重复请求
- 合理设置temperature和maxTokens参数

### 内存管理
- 及时清理流式响应资源
- 使用AbortController取消长时间运行请求
- 监控客户端内存使用情况

## 资源引用

For detailed documentation and advanced configurations, refer to:
- `references/Generating-Text.md` - Complete text generation API reference
- `references/Chatbot.md` - Full chatbot implementation guide
- `references/Message-Metadata.md` - Message metadata handling
- `references/Transport.md` - Custom transport configuration

## 常见使用模式

### 简单文本生成
使用 `generateText` 进行一次性文本创建，适合内容生成场景。

### 实时对话系统
结合 `useChat` 和 `streamText` 构建交互式聊天应用。

### 批量处理
利用 `generateText` 的稳定性处理大量文本转换任务。

当需要高级功能如消息持久化、自定义提供商或复杂传输配置时，请查阅references/目录中的原始文档。