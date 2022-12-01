# AWS Lambda :

- **Go to lambda Service :**
---
![a](https://user-images.githubusercontent.com/78825764/205051911-5486cd03-a5cb-4768-b550-bf79919dfd0c.PNG)

- **Click on Create Function :**
---

![a](https://user-images.githubusercontent.com/78825764/205052137-79a002d3-866d-46c6-9cf1-6db98e494700.PNG)

- **Add a name to your Lambda Function and choose python 3.9 Runtime, then click on create function :**
---

![a](https://user-images.githubusercontent.com/78825764/205054137-8548fcb1-ce70-4914-9eb6-a0f381a64fd7.PNG)


---
![a](https://user-images.githubusercontent.com/78825764/205054519-7afa668f-525b-4725-8d69-e168d699604e.PNG)


- **Now we will modify lambda_function.py code.Try to copy and paste this code :**

```
import os
import io
import boto3
import json
import csv

# grab environment variables
ENDPOINT_NAME = os.environ['ENDPOINT_NAME']
runtime= boto3.client('runtime.sagemaker')

def lambda_handler(event, context):
    print("Received event: " + json.dumps(event, indent=2))
    
    data = json.loads(json.dumps(event))
    payload = data['data']
    print(payload)
    
    response = runtime.invoke_endpoint(EndpointName=ENDPOINT_NAME,
                                       ContentType='text/csv',
                                       Body=payload)
    print(response)
    result = json.loads(response['Body'].read().decode())
    print(result)
    pred = int(result['predictions'][0]['score'])
    predicted_label = 'M' if pred == 1 else 'B'
    
    return predicted_label
```
- **Then Click on Deploy :**

---
- **Now we need to add Environment variables to connect our lambda function with our endpoint :**
---
- **Go to Configuration > Environment variables :**
---
![a](https://user-images.githubusercontent.com/78825764/205055993-2df08414-9f87-4b87-88ab-14da891c59df.PNG)

- **Click on Edit :**

---
![b](https://user-images.githubusercontent.com/78825764/205057508-d3ef40f0-fba8-42da-b3fa-430a6c170119.png)

- **Add ENDPOINT_NAME as the key and the name of your endpoint as the value :**
---
![a](https://user-images.githubusercontent.com/78825764/205058013-7798b6c2-0b35-482b-a358-5e5695141ab0.PNG)

- **Click on save.**
---
- **Go to IAM :**
---

![a](https://user-images.githubusercontent.com/78825764/205060313-47b114cc-65d8-46d6-92b6-657bc281f2f7.PNG)

- **Click on Roles then search for your lanbda function role :**
---
![a](https://user-images.githubusercontent.com/78825764/205074546-e503b5be-60ec-44ec-b0e5-5a8fab6709c2.PNG)

- **Double click on your lambda role :**
- **Then click on the policy name :**
---


![a](https://user-images.githubusercontent.com/78825764/205075567-ed2ca812-e824-4319-bc4b-c2722db34128.PNG)

- **Click on Edit policy :**
---

![a](https://user-images.githubusercontent.com/78825764/205076201-c9e00aa8-3b61-4707-abee-92cfb0ea9215.PNG)


- **Click on JSON :**
---
![a](https://user-images.githubusercontent.com/78825764/205076682-f9019ea4-7638-4d6a-9f86-aaeeb2808cae.PNG)

- **Add this code section to your JSON :**
---

```
{
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "sagemaker:InvokeEndpoint",
            "Resource": "*"
        }
```

![a](https://user-images.githubusercontent.com/78825764/205077812-998a3744-2cd5-4ce6-b559-9219f17d83b8.PNG)

- **Click on Review policy > save changes .**










