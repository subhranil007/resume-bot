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
   - [What is an Intent?](#what-is-an-intent)  
   - [WelcomeIntent](#61-welcomeintent)  
   - [FallbackIntent](#62-fallbackintent)  
7. [Creating Custom Slot Types](#7-creating-custom-slot-types)  
   - [What are Slots?](#what-are-slots)  
8. [Connecting AWS Lambda](#8-connecting-aws-lambda)  
   - [What is AWS Lambda?](#what-is-aws-lambda)  
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

**Amazon Lex** is a fully managed AI service by AWS for building conversational interfaces into any application using voice and text. It powers **Amazon Alexa** and uses automatic speech recognition (ASR) and natural language understanding (NLU).

### ✅ Key Features:
- Understands user intent and context
- Integrates with AWS Lambda for dynamic responses
- Pay-as-you-go pricing
- Scales to handle high traffic

---

## 4. 🔧 Prerequisites

- An [AWS account](https://aws.amazon.com)
- IAM user with permissions to access:
  - Amazon Lex
  - AWS Lambda
- Basic Python knowledge (for Lambda)
- Resume content to use in responses

---

## 5. 🛠️ Creating the Lex Bot

1. Log into the [AWS Management Console](https://aws.amazon.com/console)
2. Go to **Amazon Lex V2**
3. Click **"Create bot"**
4. Choose **"Create a blank bot"** (under the *Traditional* tab)
5. Bot Name: `ResumeBot`
6. Description:  
   *Helps recruiters understand my career and projects through a chatbot interface*
7. IAM Permissions: Select **"Create a role with basic Lex permissions"**
8. COPPA: Select **"No"**
9. Session Timeout: `5 minutes`
10. Language: English
11. Voice: Optional (e.g., Danielle)

---

## 6. 🧠 Defining Intents

### 🔍 What is an Intent?

An **Intent** represents the purpose or goal of a user’s input. For example, when a user says “Hi” or “I have a job offer,” they are trying to start a conversation. Each intent maps to a specific action or response.

### 6.1 WelcomeIntent

- **Intent Name**: `WelcomeIntent`
- **Description**: Responds to user greetings or job-related phrases
- **Sample Utterances**:
  - Hi
  - Hello
  - Can we talk?
  - Are you a data engineer?
- **Response**:

  ```
  Hello, I am Subhranil Sengupta Bot. I am an AI clone of Subhranil Sengupta. I can share my skills, projects, and experience with you!
  ```

### 6.2 FallbackIntent

- **Purpose**: Catch-all intent when no other intent matches
- **When triggered**: Bot fails to understand the user’s input
- **Response**:

  ```
  Sorry, I didn't understand that. I can:

  1. Share details about Subhranil Sengupta’s career.
  2. Provide skills, experiences, and project information.
  
  Can you please rephrase?
  ````

---

## 7. 📥 Creating Custom Slot Types

### 🔍 What are Slots?

**Slots** are variables in a conversation used to capture specific pieces of information from the user — like names, dates, or job roles.

Amazon Lex provides built-in slot types (e.g., date, number), but you can also create custom ones.

### Steps to Create a Custom Slot Type:

1. In Lex, go to **Slot Types**
2. Click **“Add slot type”**
3. Name: `HrQuestions`
4. Enable: **Restrict to slot values**
5. Add custom values:
 - Checking
 - Interview
 - JobRole
6. Save

You can now use this slot to ask, for example:  
> “What role are you hiring for?”

---

## 8. 🔗 Connecting AWS Lambda

### 🔍 What is AWS Lambda?

**AWS Lambda** is a serverless compute service that runs backend code in response to events (like user input). It lets you run logic without provisioning or managing servers.

### Steps to Integrate Lambda:

1. Go to **AWS Lambda → Create Function**
2. Runtime: `Python 3.8+`
3. Function name: `ResumeBotLambda`

### Sample Lambda Code:

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

4. Save and deploy
5. In Amazon Lex, go to the relevant intent → **Lambda Initialization and Fulfillment**
6. Attach your Lambda function

---

## 9. 🧪 Testing the Chatbot

1. In Amazon Lex Console, click **“Test Bot”**
2. Try greeting messages like:

   * “Hi”
   * “Can we talk?”
3. Try unknown phrases to test `FallbackIntent`
4. Try Lambda-based queries like:

   * “What projects have you worked on?”

---

## 10. 🧰 Technologies Used

| Tool        | Description                             |
| ----------- | --------------------------------------- |
| Amazon Lex  | NLP engine for building chatbots        |
| AWS Lambda  | Serverless logic for custom responses   |
| IAM         | Access control and permissions          |
| Python      | Language used in Lambda                 |
| AWS Console | UI for building, testing, and deploying |

---

## 11. 🚀 Future Enhancements

* Add more intents for:

  * Education
  * Certifications
  * Soft skills
* Fetch content dynamically from a resume file or database
* Deploy on platforms like Slack, websites, or mobile apps
* Add voice interaction or IVR flow for call-based interactions

---

## 🙌 Conclusion

You've now created a fully functional, AI-powered chatbot using Amazon Lex that can:

* Respond to user inputs
* Handle errors gracefully
* Offer dynamic, customized responses via Lambda

With further tuning, this can evolve into a full-featured virtual assistant for job seekers and recruiters alike.

---
