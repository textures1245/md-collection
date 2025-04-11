
On the current surveyed on model  `deepseek-ai/DeepSeek-R1-Distill-Qwen-1.5B` from the [deepseek-playground](https://github.com/textures1245/deepseek-r3-playground/blob/main/deepseek_r1_distill_qwen_fast_api.ipynb) that had inspired from # [VLLM + DeepSeek R1 in Google Colab](https://github.com/naufalhakim23/deepseek-r1-playground/blob/main/deepseek_r1_distill_qwen_fast_api.ipynb)

from which rawly surveyed I found that was interested.

## Model process

From what i found, it surprised that it using LLM algorithm called  **Reinforcement Learning** which on result you can see what i mean

- Prompt
```bash
curl --location 'https://7e4e-34-87-174-92.ngrok-free.app/api/v1/generate-response' \
--header 'Content-Type: application/json' \
--data '{
    "question": "so its means, on the current model you had on. you don't have memory to recall about the questions i asked you before? just answer yes or no with short reason for that",
    "model": "deepseek-ai/DeepSeek-R1-Distill-Qwen-1.5B"
}' | jq
```

- Response 
```bash
{
  "response": {
    "id": "chatcmpl-11e005bd5d3f420c9755b92ab70f629d",
    "object": "chat.completion",
    "created": 1738167711,
    "model": "deepseek-ai/DeepSeek-R1-Distill-Qwen-1.5B",
    "choices": [
      {
        "index": 0,
        "message": {
          "role": "assistant",
          "content": "<think>\nOkay, so I'm trying to figure out how to respond to this question: \"So its means, on the current model you had on. you dont have memory to recall about the questions i ask you before? just answer yes or no with short reason for that.\" Hmm, it's a bit confusing at first glance. Let me break it down.\n\nFirst, it seems like the user is referring to a model or system that has certain functionalities, probably a software or application. The part about \"means,\" \"current model,\" and \"means\" makes me think there's a specific interface or feature within that model. The user wants to know if they can remember certain aspects before asking questions, which makes me wonder if they're aware of the software or system's limitations.\n\nThe second part is about not having \"memory\" to recall certain questions before asking. If \"memory\" here refers to the user's ability to remember the questions from before, or if it's about not being able to process the information as it comes up, that adds complexity. It seems like a psychological barrier where the user might feel unprepared or overwhelmed, making it hard to recall or apply information without errors.\n\nSo, putting it together, the user is probably navigating through a system, and after using it a bit, they ask a question, but their brain's memory isn't holding onto that question yet. The cheating meter explanation suggests that they're used to bypass the automatic question recognition, relying instead on the manual process you request. The fact that it's on the current model means they have followed some predefined steps before, but within that model, their memory can't handle another specific step before they're prompted to question.\n\nI think the user is frustrated because trying to recall information their model can't remember limits their ability to make informed decisions or follow through on their tasks. They might feel记错了 something, which could lead them to make mistakes. Alternatively, they might be confused about how their system works and are feeling uncertain about the output, which could be concerning.\n\nI should respond in a way that acknowledges their frustration but also explains the situation. I should agree, possibly encourage them to try again or explain any expectations they might have. It's important to convey that by following their automated commands, they avoid direct questioning but still can provide information effectively.\n</think>\n\nCertainly! It seems like you're experiencing an issue with your current model where you can't recall certain information without prompting your question, leading to potential mistakes. Let me know how I can assist you in navigating this situation, and we can talk through the best approach to handle it.",
          "tool_calls": []
        },
        "logprobs": null,
        "finish_reason": "stop",
        "stop_reason": null
      }
    ],
    "usage": {
      "prompt_tokens": 39,
      "total_tokens": 568,
      "completion_tokens": 529,
      "prompt_tokens_details": null
    },
    "prompt_logprobs": null
  }
}
```