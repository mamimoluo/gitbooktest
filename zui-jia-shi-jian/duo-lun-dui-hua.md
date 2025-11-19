# 多轮对话

在智能客服、角色扮演等场景中，用户期望模型能结合之前的交互内容生成连贯回复。由于通义千问 API 是无状态的（Stateless），不会自动保存对话状态，实现多轮对话需在每次请求中显式传入对话历史。本文介绍如何实现多轮对话，并提供管理对话上下文、节省 Token 的实用方法。

### 工作原理 <a href="#id-5b03a2da4d9lz" id="id-5b03a2da4d9lz"></a>

实现多轮对话的核心是维护一个 `messages` 数组。每一轮对话都需要将用户的最新提问和模型的回复追加到此数组中，并将其作为下一次请求的输入。

以下示例为多轮对话时 `messages` 的状态变化：

1.  第一轮对话

    向`messages` 数组添加用户问题。

    &#x20;

    ```json
    // 使用文本模型
    [
        {"role": "user", "content": "推荐一部关于太空探索的科幻电影。"}
    ]

    // 使用多模态模型，以 Qwen-VL 为例
    // {"role": "user",
    //       "content": [{"type": "image_url","image_url": {"url": "https://help-static-aliyun-doc.aliyuncs.com/file-manage-files/zh-CN/20251031/ownrof/f26d201b1e3f4e62ab4a1fc82dd5c9bb.png"}},
    //                   {"type": "text", "text": "请问图片展现了有哪些商品？"}]
    // }
    ```
2.  第二轮对话

    向`messages`数组添加大模型回复内容与用户的最新提问。

    &#x20;

    ```json
    // 使用文本模型
    [
        {"role": "user", "content": "推荐一部关于太空探索的科幻电影。"},
        {"role": "assistant", "content": "我推荐《xxx》，这是一部经典的科幻作品。"},
        {"role": "user", "content": "这部电影的导演是谁？"}
    ]

    // 使用多模态模型，以 Qwen-VL 为例
    //[
    //    {"role": "user", "content": [
    //                    {"type": "image_url","image_url": {"url": "https://help-static-aliyun-doc.aliyuncs.com/file-manage-files/zh-CN/20251031/ownrof/f26d201b1e3f4e62ab4a1fc82dd5c9bb.png"}},
    //                   {"type": "text", "text": "请问图片展现了有哪些商品？"}]},
    //    {"role": "assistant", "content": "图片展示了三件商品：一件浅蓝色背带裤、一件蓝白条纹短袖衬衫和一双白色运动鞋。"},
    //    {"role": "user", "content": "它们属于什么风格？"}
    //]
    ```

### 快速开始 <a href="#id-393ed51341wq3" id="id-393ed51341wq3"></a>

OpenAI兼容DashScopePythonNode.jscurl&#x20;

```python
import os
from openai import OpenAI

client = OpenAI(
    # 若没有配置环境变量，请用阿里云百炼API Key将下行替换为：api_key="sk-xxx",
    # 新加坡和北京地域的API Key不同。获取API Key：https://help.aliyun.com/zh/model-studio/get-api-key
    api_key=os.getenv("DASHSCOPE_API_KEY"),
    # 以下是北京地域base_url，如果使用新加坡地域的模型，需要将base_url替换为：https://dashscope-intl.aliyuncs.com/compatible-mode/v1
    base_url="https://dashscope.aliyuncs.com/compatible-mode/v1",
)

def get_response(messages):
    completion = client.chat.completions.create(
        model="qwen-plus",
        messages=messages
    )
    return completion.choices[0].message.content

# 初始化 messages
messages = []

# 第 1 轮
messages.append({"role": "user", "content": "推荐一部关于太空探索的科幻电影。"})
print("第1轮")
print(f"用户：{messages[0]['content']}")
assistant_output = get_response(messages)
messages.append({"role": "assistant", "content": assistant_output})
print(f"模型：{assistant_output}\n")

# 第 2 轮
messages.append({"role": "user", "content": "这部电影的导演是谁？"})
print("第2轮")
print(f"用户：{messages[-1]['content']}")
assistant_output = get_response(messages)
messages.append({"role": "assistant", "content": assistant_output})
print(f"模型：{assistant_output}\n")
```

### 多模态模型的多轮对话 <a href="#id-36eaa79f55dz7" id="id-36eaa79f55dz7"></a>

**说明**

* 本章节适用于[`Qwen-VL`](https://help.aliyun.com/zh/model-studio/vision)、[`Qwen-Audio`](https://help.aliyun.com/zh/model-studio/audio-language-model)模型。对于 `Qwen-Omni` 具体实现方法请参见[全模态](https://help.aliyun.com/zh/model-studio/qwen-omni)。
* `Qwen-VL-OCR`、`Qwen3-Omni-Captioner` 是为特定单轮任务设计的模型，不支持多轮对话。

多模态模型支持在对话中加入图片、音频等内容，其多轮对话的实现方式与文本模型主要有以下不同：

* 用户消息（user message）的构造方式：多模态模型的用户消息不仅包含文本，还包含图片、音频等多模态信息。
* DashScope SDK接口：使用 DashScope Python SDK 时，需调用 `MultiModalConversation` 接口；使用DashScope Java SDK 时，需调用 `MultiModalConversation` 类。

OpenAI兼容DashScopePythonNode.jscurl&#x20;

```python
from openai import OpenAI
import os

client = OpenAI(
    # 若没有配置环境变量，请用百炼API Key将下行替换为：api_key="sk-xxx" 
    # 新加坡和北京地域的API Key不同。获取API Key：https://help.aliyun.com/zh/model-studio/get-api-key
    api_key=os.getenv("DASHSCOPE_API_KEY"),
    # 以下是北京地域base_url，如果使用新加坡地域的模型，需要将base_url替换为：https://dashscope-intl.aliyuncs.com/compatible-mode/v1
    base_url="https://dashscope.aliyuncs.com/compatible-mode/v1"
)
messages = [
        {
        "role": "user",
        "content": [
            {
                "type": "image_url",
                "image_url": {
                    "url": "https://help-static-aliyun-doc.aliyuncs.com/file-manage-files/zh-CN/20251031/ownrof/f26d201b1e3f4e62ab4a1fc82dd5c9bb.png"
                },
            },
            {"type": "text", "text": "请问图片展现了有哪些商品？"},
        ],
    }
]

completion = client.chat.completions.create(
    model="qwen3-vl-plus",  # 可按需更换为其它多模态模型，并修改相应的 messages
    messages=messages,
    )
    
print(f"第一轮输出：{completion.choices[0].message.content}")

assistant_message = completion.choices[0].message
messages.append(assistant_message.model_dump())
messages.append({
        "role": "user",
        "content": [
        {
            "type": "text",
            "text": "它们属于什么风格？"
        }
        ]
    })
completion = client.chat.completions.create(
    model="qwen3-vl-plus",
    messages=messages,
    )
    
print(f"第二轮输出：{completion.choices[0].message.content}")
```

### 思考模型的多轮对话 <a href="#id-9c315c95a8omt" id="id-9c315c95a8omt"></a>

思考模型返回`reasoning_content`（思考过程）与`content`（回复内容）两个字段。更新 messages 数组时，仅保留`content`字段，忽略`reasoning_content`字段。

&#x20;

```json
[
    {"role": "user", "content": "推荐一部关于太空探索的科幻电影。"},
    {"role": "assistant", "content": "我推荐《xxx》，这是一部经典的科幻作品。"}, # 添加上下文时请勿添加reasoning_content字段
    {"role": "user", "content": "这部电影的导演是谁？"}
]
```

> 思考模型详情参见：[深度思考](https://help.aliyun.com/zh/model-studio/deep-thinking)、[视觉理解](https://help.aliyun.com/zh/model-studio/vision)、[视觉推理](https://help.aliyun.com/zh/model-studio/visual-reasoning)。

> Qwen3-Omni-Flash（思考模式）实现多轮对话请参见[全模态](https://help.aliyun.com/zh/model-studio/qwen-omni#76b04b353ds7i)。

OpenAI兼容DashScopePythonNode.jsHTTP

#### 示例代码 <a href="#dc6474fd70932" id="dc6474fd70932"></a>

&#x20;

```python
from openai import OpenAI
import os

# 初始化OpenAI客户端
client = OpenAI(
    # 如果没有配置环境变量，请用阿里云百炼API Key替换：api_key="sk-xxx"
    # 新加坡和北京地域的API Key不同。获取API Key：https://help.aliyun.com/zh/model-studio/get-api-key
    api_key = os.getenv("DASHSCOPE_API_KEY"),
    # 以下是北京地域base_url，如果使用新加坡地域的模型，需要将base_url替换为：https://dashscope-intl.aliyuncs.com/compatible-mode/v1
    base_url="https://dashscope.aliyuncs.com/compatible-mode/v1"
)

messages = []
conversation_idx = 1
while True:
    reasoning_content = ""  # 定义完整思考过程
    answer_content = ""     # 定义完整回复
    is_answering = False   # 判断是否结束思考过程并开始回复
    print("="*20+f"第{conversation_idx}轮对话"+"="*20)
    conversation_idx += 1
    user_msg = {"role": "user", "content": input("请输入你的消息：")}
    messages.append(user_msg)
    # 创建聊天完成请求
    completion = client.chat.completions.create(
        # 您可以按需更换为其它深度思考模型
        model="qwen-plus",
        messages=messages,
        extra_body={"enable_thinking": True},
        stream=True,
        # stream_options={
        #     "include_usage": True
        # }
    )
    print("\n" + "=" * 20 + "思考过程" + "=" * 20 + "\n")
    for chunk in completion:
        # 如果chunk.choices为空，则打印usage
        if not chunk.choices:
            print("\nUsage:")
            print(chunk.usage)
        else:
            delta = chunk.choices[0].delta
            # 打印思考过程
            if hasattr(delta, 'reasoning_content') and delta.reasoning_content != None:
                print(delta.reasoning_content, end='', flush=True)
                reasoning_content += delta.reasoning_content
            else:
                # 开始回复
                if delta.content != "" and is_answering is False:
                    print("\n" + "=" * 20 + "完整回复" + "=" * 20 + "\n")
                    is_answering = True
                # 打印回复过程
                print(delta.content, end='', flush=True)
                answer_content += delta.content
    # 将模型回复的content添加到上下文中
    messages.append({"role": "assistant", "content": answer_content})
    print("\n")
```

### 应用于生产环境 <a href="#b45721fe6484z" id="b45721fe6484z"></a>

多轮对话会带来巨大的 Token 消耗，且容易超出大模型上下文最大长度导致报错。以下策略可帮助您有效管理上下文与控制成本。

#### 1. 上下文管理 <a href="#id-4e96dbe7fa5br" id="id-4e96dbe7fa5br"></a>

`messages` 数组会随对话轮次增加而变长，最终可能超出模型的 Token 限制。建议参考以下内容，在对话过程中管理上下文长度。

**1.1. 上下文截断**

当对话历史过长时，保留最近的 N 轮对话历史。该方式实现简单，但会丢失较早的对话信息。

**1.2. 滚动摘要**

为了在不丢失核心信息的前提下动态压缩对话历史，控制上下文长度，可随着对话的进行对上下文进行摘要：

a. 对话历史达到一定长度（如上下文长度最大值的 70%）时，将对话历史中较早的部分（如前一半）提取出来，发起独立 API 调用使大模型对这部分内容生成“记忆摘要”；

b. 构建下一次请求时，用“记忆摘要”替换冗长的对话历史，并拼接最近的几轮对话。

**1.3. 向量化召回**

滚动摘要会丢失部分信息，为了使模型可以从海量对话历史中“回忆”起相关信息，可将对话管理从“线性传递”转变为“按需检索”：

a. 每轮对话结束后，将该轮对话存入向量数据库；

b. 用户提问时，通过相似度检索相关对话记录；

c. 将检索到的对话记录与最近的用户输入拼接后输入大模型。 &#x20;

#### 2. 成本控制 <a href="#id-9703598cafzpi" id="id-9703598cafzpi"></a>

输入 Token 数会随着对话轮数增加，显著增加使用成本，以下成本管理策略供您参考。

**2.1. 减少输入 Token**

通过上文介绍的上下文管理策略减少输入 Token，降低成本。

**2.2. 使用支持上下文缓存的模型**

发起多轮对话请求时，`messages` 部分会重复计算并计费。阿里云百炼对`qwen-max`、`qwen-plus`等模型提供了[上下文缓存](https://help.aliyun.com/zh/model-studio/context-cache)功能，可以降低使用成本并提升响应速度，建议优先使用支持上下文缓存的模型。

> 上下文缓存功能自动开启，无需修改代码。

### 错误码 <a href="#efc117f59a2gm" id="efc117f59a2gm"></a>

如果模型调用失败并返回报错信息，请参见[错误信息](https://help.aliyun.com/zh/model-studio/error-code)进行解决。
