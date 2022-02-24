## Context 

Alchemy is a crypto cloud services provider that provides useful APIs for blockchains. Most of us know Alchemy for Alchemy Supernode which provides blockchain API for Ethereum, Polygon, Arbitrum as etc. Alchemy also provides NFT API. This is useful for use cases like querying NFTs owned by an Address. This lets us do interesting things with the data. Opensea API often has limits. Alchemy being a cloud provider has a free tier and a paid tier if needed which makes it immediately accesible and scalable if requred.  

- I'll be using AWS Lambda to query the Alchemy NFT and will save the results( json file) in an S3 cloud storage bucket. 


## Steps 


- Create a project on alchemyapi.io to generate API keys 
  -    alchemyapi.io
- Make a get request to the alchemy NFT API endpoint to get the NFT owned by an address
- Store results in a Amazon S3 bucket 


## Code 

```python
import json
import requests
import boto3
import os

s3 = boto3.resource('s3')

# replace the bucket name below(alchemy-nft-example) with your Amazon S3 bucket name 
s3object = s3.Object('alchemy-nft-example', 'nft-data1.json')

key= os.environ['ALCHEMY_KEY']

def lambda_handler(event, context):
    # Note: replace they key variable with your API key. In this example, I have my API key stored as environment variable in AWS Lambda. Replace the Ethereum address with the address you want to query
    response= requests.get( f'https://eth-mainnet.g.alchemy.com/v2/{key}/getNFTs/?owner=0xaFeb5f1f3DB00F66e28ef38FCf9cdE0D469FAb4F&withMetadata=true')
    
    json_response= response.json()
        
    s3object.put(
    Body=(response.text), ContentType='application/json')

    #debug 
    print(json_response)
    
```

If you don't want to use AWS Lambda and S3 just use this portion in a python script

```python
import json
import requests

 response= requests.get( f'https://eth-mainnet.g.alchemy.com/v2/{key}/getNFTs/?owner=0xaFeb5f1f3DB00F66e28ef38FCf9cdE0D469FAb4F&withMetadata=true')
    
 json_response= response.json()
 print(json_response)

```



## Notes 



## Frameworks and tools used 
- AWS Lambda 
- Amazon S3 


## References 
- https://docs.alchemy.com/alchemy/enhanced-apis/nft-api
