# Reproducible output with Azure OpenAI GPT-4-Turbo v1106
Generating reproducible output in GPT-4-Turbo with the "seed" parameter in Python SDK v1.x. This feature was originally announced by OpenAI during their Dev Day on 6th of November, and is available in Azure OpenAI from 17th of November.

To use the latest version of openai python packahe, you can upgrade it wth the following pip command:
```
pip install --upgrade openai
```

## Table of contents:
- [Pre-requisites](https://github.com/LazaUK/AOAI-ReproducibleOutput-SDKv1#pre-requisites)
- [Option 1: Testing without seed]()
- [Option 2: Testing with seed]()
- [Verifying reproducible outcome]()

## Pre-requisites
1. Ensure that you deploy GPT model of v1106, either GPT-4-Turbo or GPT-35-Turbo.
![screenshot_0_deployment](images/seed_pr_1_deployment.png)
2. Set API endpoint name, version and key, along with the Azure OpenAI deployment name to the relevant environment variables. Provided code assumes that environment variables are **OPENAI_API_BASE**, **OPENAI_API_VERSION**, **OPENAI_API_KEY** and **OPENAI_API_DEPLOY**.
![screenshot_0_deployment](images/seed_pr_1_environment.png)
3. To "almost always" reproduce the same output, your "**seed**" parameter should be set to the same integer value. In this example, it's set to **42**.
4. All the other parameters (like "**temperature**", "**messages**", etc.) in the Chat Completions API call should also stay the same.

## Option 1: Testing without seed
To test the model's behaviour when temperature is above 0, we create a simple lits with 2 identical prompts:
``` JSON
[
    "Create a story about red panda.",
    "Create a story about red panda."
]
```
If we'll submit it now to GPT-4-Turbo with the temperature value of 0.1 and without seed paramater:
``` Python
completion = client.chat.completions.create(
    model = AOAI_Deployment, # model = "Azure OpenAI deployment name".
    temperature = 0.1,
    messages = [
        {"role": "system", "content": "You always produce 3-sentence answers."},
        {"role": "user", "content": prompt}
    ]        
)
```
Then we may get slightly differenet outputs for two separate submissiosn of the same prompt:
``` JSON
--------------------
In the lush forests of the Himalayas, a curious red panda named Pabu spent his days frolicking among the trees. One day, Pabu stumbled upon a hidden grove filled with the sweetest bamboo he'd ever tasted, but it was guarded by a mischievous monkey. With cleverness and a dash of bravery, Pabu outwitted the monkey, sharing the grove's bounty with his fellow pandas, becoming a legend in the forest.
--------------------
In the lush forests of the Himalayas, a curious red panda named Pabu spent his days frolicking among the trees. One day, Pabu stumbled upon a hidden grove filled with the sweetest bamboo he'd ever tasted, which he decided to keep as his secret snack spot. Little did he know, his delightful discovery would soon attract a band of fellow pandas, leading to the most enchanting bamboo feasts the forest had ever seen.
--------------------
```


## Option 2: Testing with seed

## Verifying reproducible outcome
