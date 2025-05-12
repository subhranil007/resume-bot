# 🤖 Amazon Lex Chatbot – ResumeBot

A step-by-step guide to building an intelligent resume chatbot using **Amazon Lex** and **AWS Lambda**, designed to act as a virtual version of Subhranil Sengupta.

---

## 📌 Table of Contents

1. [Introduction](#1-introduction)
2. [Project Objective](#2-project-objective)
3. [What is Amazon Lex?](#3-what-is-amazon-lex)
4. [Prerequisites](#4-prerequisites)
5. [Creating the Lex Bot](#5-creating-the-lex-bot)
6. [Defining Intents](#6-defining-intents)
   - [WelcomeIntent](#61-welcomeintent)
   - [FallbackIntent](#62-fallbackintent)
7. [Creating Custom Slot Types](#7-creating-custom-slot-types)
8. [Connecting AWS Lambda](#8-connecting-aws-lambda)
9. [Testing the Chatbot](#9-testing-the-chatbot)
10. [Technologies Used](#10-technologies-used)
11. [Future Enhancements](#11-future-enhancements)

---

## 1. 📖 Introduction

This project demonstrates how to create an intelligent chatbot using **Amazon Lex** that serves as a virtual resume interface. It can answer questions about your career, skills, and past experiences using natural language processing (NLP).

---

## 2. 🎯 Project Objective

To develop a chatbot that:
- Acts as a virtual clone of the user (Subhranil Sengupta)
- Engages recruiters in conversation
- Shares relevant skills, experience, and project information
- Enhances job application interaction through automation

---

## 3. 💡 What is Amazon Lex?

Amazon Lex is a fully managed AI service for building conversational interfaces using voice and text. It powers **Alexa** and allows developers to:

- Understand and classify intents
- Build multi-turn conversations
- Use slots for data capture
- Integrate with Lambda for dynamic responses
- Deploy securely and scalably with AWS infrastructure

---

## 4. 🔧 Prerequisites

- AWS account
- IAM user with permissions to access:
  - Amazon Lex
  - AWS Lambda
- Basic knowledge of Python (for Lambda functions)
- Resume content (structured or plain text)

---

## 5. 🛠️ Creating the Lex Bot

1. Log into the [AWS Management Console](https://aws.amazon.com/console).
2. Navigate to **Amazon Lex V2**.
3. Click **"Create bot"**.
4. Select **"Create a blank bot"** under the **Traditional** tab.
5. Bot Name: `ResumeBot`
6. Description:  
   *Resume bot to help HRs and recruiters interact with me 24/7 and understand my career path, skills, and project experience.*
7. IAM Permissions: Choose **Create a role with basic Amazon Lex permissions**.
8. COPPA: Select **No**.
9. Session Timeout: **5 minutes**
10. Language: **English**
11. Voice: Optional (e.g., Danielle for voice-enabled bots)
12. Confidence Threshold: Keep default (0.40)
13. Click **Create**.

---

## 6. 🧠 Defining Intents

### 6.1 WelcomeIntent

- **Intent Name**: `WelcomeIntent`
- **Description**: Greets the user.
- **Sample Utterances**:
  - Hi
  - Hello
  - Can we talk?
  - Are you a data engineer?
  - I have a job offer
  - I have an opportunity for you
- **Closing Response**:
```

Hello, I am Subhranil Sengupta Bot. I am an AI clone of Subhranil Sengupta. I can share my skills, projects, and experience with you!

```

### 6.2 FallbackIntent

- **Triggered when**: No other intent is matched.
- **Response**:
```

Sorry, I didn't understand that. I can:

1. Share details about Subhranil Sengupta’s career.
2. Provide skills, experiences, and project information.
   Can you please rephrase?

````

---

## 7. 📥 Creating Custom Slot Types

Slots are variables used by intents to gather necessary data.

### Steps to Add Custom Slots:

1. Go to **Slot types** on the left panel.
2. Click **"Add slot type"** > **"Add blank slot type"**.
3. Name: `HrQuestions`
4. Enable **Restrict to slot values**.
5. Add slot values such as:
 - Checking
 - JobRole
 - Interview
6. Save Slot Type.

You can now use this slot in relevant intents to ask follow-up questions like:
> "What role are you hiring for?"

---

## 8. 🔗 Connecting AWS Lambda

Amazon Lex can delegate fulfillment logic to Lambda functions.

### Step-by-Step:

1. Go to AWS Lambda → Create Function
2. Runtime: Python 3.9 (or 3.8)
3. Function Name: `ResumeBotLambda`

### Sample Python Lambda Code:
```python
def lambda_handler(event, context):
  intent_name = event['sessionState']['intent']['name']
  
  if intent_name == 'UnderstandUser':
      return {
          "sessionState": {
              "dialogAction": {
                  "type": "Close"
              },
              "intent": {
                  "name": "UnderstandUser",
                  "state": "Fulfilled"
              }
          },
          "messages": [{
              "contentType": "PlainText",
              "content": "I have worked as a Data Engineer at Amazon. Some of my key projects include setting up data pipelines using Glue and Athena..."
          }]
      }
````

4. Deploy the function.
5. In Lex → Intent settings → **Lambda Initialization and Fulfillment**, attach the Lambda function.

---

## 9. 🧪 Testing the Chatbot

### How to Test:

1. In the Lex Console, go to **Test Bot**.
2. Type any of the utterances defined in `WelcomeIntent`, e.g., “Hi”
3. Try unknown phrases to trigger `FallbackIntent`
4. Ask about skills or job roles to test Lambda integration.

---

## 10. 🧰 Technologies Used

| Tool           | Purpose                             |
| -------------- | ----------------------------------- |
| Amazon Lex     | Chatbot framework                   |
| AWS Lambda     | Backend logic & dynamic fulfillment |
| IAM            | Secure permissions                  |
| Python         | Lambda scripting                    |
| Amazon Console | Development & testing UI            |

---

## 11. 🚀 Future Enhancements

* Add more intents for:

  * Certifications
  * Education
  * Personal projects
* Connect chatbot to a database or S3 bucket to fetch resume dynamically
* Deploy on a website or Slack using API Gateway
* Enable voice interaction for IVR or Alexa

---

## 🙌 Conclusion

This chatbot allows recruiters to interact with a resume conversationally, making candidate evaluation more intuitive and interactive. With Amazon Lex and AWS Lambda, you can scale this idea into a full-fledged HR assistant.
