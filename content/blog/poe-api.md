---
title: "How users are getting free access to GPT-4?!"
description: "Get access to paid models for free which include GPT-4 (2k & 32K tokens) Claude and many more provided by poe."
dateString: Jul 22, 2023
draft: false
tags: ["Chatgpt", "Hacking", "Reverse Engineering", "Artificial Intelligence","Cybersecurity"]
weight: 101
cover:
    image: "/blog/poi-api/poe-api.jpg"
---





## What is Poe 
[Poe](https://poe.com/) is a platform by Quora that lets you create and talk to AI chatbots that can do amazing things with words. You can use Poe to chat with bots that are powered by OpenAI's ChatGPT and GPT-4, as well as [Anthropic's Claude](https://www.anthropic.com/index/claude-2). These are some of the most advanced AI language models in the world, and they can generate text on any topic or task you want.

But there is a catch: *Poe is not free*. You have to pay a monthly fee to use its features. 

That's why some people have found a way to access Poe features for free by hacking its API. The API is the secret code that allows Poe to communicate with your device and other apps. By reverse engineering the API, you can trick Poe into thinking that you are a paying customer, and get access to all its features without paying a dime.

In this blog post, I will show you how to hack Poe's API and get free access to its features. I will also tell you why this is not a good idea, and what risks you may face by doing so.

## What is Reverse Engineering?
Reverse engineering is the process of analyzing a system or a product to understand how it works and how it was made. It can be used for various purposes, such as learning, improving, modifying, or copying something.

In the context of Poe, reverse engineering means figuring out how the API works by inspecting the network traffic between Poe and the devices that use it. The API defines how requests and responses are formatted, what parameters are required, and what actions are possible. The API also uses authentication and encryption mechanisms to ensure that only authorized users can access Poe's features.

By reverse engineering the API, one can create their own tools or libraries that can access Poe's features for free, without paying for a subscription. These tools or libraries mimic the behavior of the official Poe app or website, but bypass the authentication and encryption checks. They also allow users to customize the parameters and options of the API calls, such as the bot ID, the message content, the response length, or the device ID.


## How People are Hacking Poe's API?

To hack Poe's API, you need to use some tools and skills to spy on and manipulate the messages that Poe sends and receives. Here are some steps that you can follow:

**1.** Log into Poe on any desktop web browser, then open your browser's developer tools (also known as "inspect") and look for the value of the p-b cookie in the following menus:

- Chromium: Devtools > Application > Cookies > poe.com
- Firefox: Devtools > Storage > Cookies
- Safari: Devtools > Storage > Cookies

This cookie contains your token, which is a secret code that identifies you as a user of Poe. You will need this token later to send messages to Poe.

**2.** Install a tool such as Wireshark, Fiddler, or Burp Suite on your device. These tools can spy on and change the messages that Poe sends and receives. You can also use them to repeat or test the messages.

**3.** Set up your tool to watch the messages between your browser and Poe. You may need to change some settings or install some files on your device to do this. Check the instructions of your tool for more details.

**4** Start watching the messages and filter them by poe.com. You should see some messages that look like this:

```
POST /api/graphql HTTP/1.1
Host: poe.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.107 Safari/537.36
Content-Type: application/json
Accept: */*
Origin: https://poe.com
Referer: https://poe.com/
Cookie: p-b=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Content-Length: 123

{"query":"query GetBots {\n  bots {\n    id\n    name\n    description\n    icon\n  }\n}","variables":null,"operationName":"GetBots"}

HTTP/1.1 200 OK
Date: Fri, 22 Jul 2023 01:02:25 GMT
Content-Type: application/json
Content-Length: 456

{"data":{"bots":[{"id":"bot_1","name":"ChatGPT","description":"A friendly chatbot powered by OpenAI's ChatGPT model.","icon":"https://poe.com/icons/chatgpt.png"},{"id":"bot_2","name":"GPT-4","description":"A powerful chatbot powered by OpenAI's GPT-4 model.","icon":"https://poe.com/icons/gpt4.png"},{"id":"bot_3","name":"Claude","description":"A creative chatbot powered by Anthropic's Claude model.","icon":"https://poe.com/icons/claude.png"}]}}
```
These are examples of GraphQL messages, which is the language that Poe uses for its API. You can learn more about GraphQL here: https://graphql.org/


**5** Study the messages to understand how the API works. You can use tools such as Postman, curl, or Python requests to send and receive messages to Poe. You can also use tools such as GraphiQL or GraphQL Playground to explore the API structure and documentation.

**For example, you can use Postman to send a message like this:**

```
POST https://poe.com/api/graphql
Content-Type: application/json
Cookie: p-b=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

{"query":"mutation SendMessage($botId: ID!, $message: String!) {\n  sendMessage(botId: $botId, message: $message) {\n    id\n    content\n    createdAt\n  }\n}","variables":{"botId":"bot_1","message":"Hello, world!"},"operationName":"SendMessage"}
```
**And get a response like this:**

```
{
  "data": {
    "sendMessage": {
      "id": "msg_1",
      "content": "Hello! I'm ChatGPT, a friendly chatbot powered by OpenAI's ChatGPT model.",
      "createdAt": "2023-07-22T01:02:25.123Z"
    }
  }
}

```
This message sends a message to the bot with the ID bot_1, which is ChatGPT, and gets a response from the bot. You can see the parameters and fields that are needed and returned by the API.

**6.** Repeat steps 4 and 5 for different messages, such as creating or editing bots, deleting messages, purging conversations, etc. You can also use different bots or messages to see how they affect the results.

**7.** Also you can use Python to write a tool like this:

```
import requests

class PoeTool:
    def __init__(self, token):
        self.token = token
        self.base_url = "https://poe.com/api/graphql"
        self.headers = {
            "Content-Type": "application/json",
            "Cookie": f"p-b={token}"
        }

    def send_message(self, bot_id, message):
        query = """
        mutation SendMessage($botId: ID!, $message: String!) {
          sendMessage(botId: $botId, message: $message) {
            id
            content
            createdAt
          }
        }
        """
        variables = {
            "botId": bot_id,
            "message": message
        }
        data = {
            "query": query,
            "variables": variables,
            "operationName": "SendMessage"
        }
        response = requests.post(self.base_url, json=data, headers=self.headers)
        return response.json()

    # Add more functions for other messages

```


## Why You Should Not Hack Poe's API?
Hacking Poe's API may sound fun and exciting, but it is not a good idea. There are many reasons why you should not hack Poe's API:

- It is not fair: Poe is a platform that provides a valuable service to its users. It costs money and time to develop and maintain Poe and its AI models. By hacking Poe's API, you are stealing their work and their revenue. You are also hurting the developers and researchers who work on Poe and its AI models.
- It is not safe: The tools that you use to hack Poe's API may contain viruses or spyware that can harm your device and your data. You may also expose your messages and bots to hackers who can steal or misuse them for malicious purposes.

- It is not legal: Poe's API is a proprietary software that belongs to Quora and its licensors. It is protected by copyright and trade  secret laws. It also incorporates AI models that are developed by OpenAI and Anthropic, which are also protected by intellectual property laws. By hacking Poe's API, you are violating their terms of service and infringing on their rights. You may face lawsuits, fines, injunctions, damages, or criminal charges.

## Conclusion
In this blog post, I have shown you how to hack Poe's API and get free access to its features. I have also told you why this is not a good idea, and what risks you may face by doing so. My purpose was educational and to explain the theory behind it.

I hope this blog post has helped you understand how Poe works and why you should not hack its API. If you want to use Poe's features legally and safely, you should pay for a subscription or use alternative platforms that offer similar or better services.

Thank you for reading this blog post. 