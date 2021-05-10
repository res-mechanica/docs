Version: 3.1.0
# integration-manual


### Table of Contents

1. [Introduction](#Introduction)
1. [Motivation](#Motivation)
1. [Communication flow with goodmoves SaaS](#Communication-flow-with-goodmoves-SaaS)
1. [Integration](#Integration)
1. [Important data](#Important-data)
1. [state](#state)
1. [actions](#actions)
1. [reward](#reward)
1. [query_id](#query_id)
1. [Sources of Data](#Sources-of-Data)
1. [Ways to integrate](#Ways-to-integrate)
1. [GDPR Notice](#GDPR-Notice)
1. [API Docs](#API-Docs)  
    1. [Introduction](#Introduction)  
    1. [Versioning](#Versioning)  
    1. [Authentication](#Authentication)
    1. [Error handling](#Error-handling)
    1. [Endpoints](#Endpoints)
        1. [POST /action - Request a prioritised list of actions](#POST-/action-Request-a-prioritised-list-of-actions)
        1. [PATCH /action - Tell goodmoves which action was selected](#PATCH-/action-Tell-goodmoves-which-action-was-selected)
        1. [POST /reward - Send a reward related to customer](#POST-/reward-Send-a-reward-related-to-customer)
        1. [GET /  - Retrieve version information of the API](#GET-/-Retrieve-version-information-of-the-API)
  

### Introduction
#### Motivation
This document addresses developers and tech deciders who want to integrate goodmoves into the tools and systems they use and maintain.
As Software-as-a-Service, goodmoves brings the power of actual artificial intelligence to next best actions (NBA).
When communicating with a customer, be it through a webshop, an online marketing campaign or a phone call,
one faces a difficult challenge: customers should purchase a product, accept an upsell offer, or renew their contract.
No matter what situation a customer is in, there is one optimal course of action.

[img flow]
Example case (traditional): A customer might consider cancelling a contract. No action is taken, so the customer terminates.
At that point, a 10 % bonus is offered to win back the customer.

In such situations, knowing the optimal next best action is invaluable. For example, based on past interactions, goodmoves
infers that an incentive needs to be offered to a certain type of customer to prevent churn.
But more than that, goodmoves can even suggest the necessary level of incentive and the channel through which it should be offered. 
This way, an automated email could be sent to the customer offering a not too small, yet not too large discount as a
motivation to renew the contract.

[img flow]

Example case (with goodmoves NBA): Instead of initially not offering any discount followed by an expensive win-back bonus,
the first signs of contract termination trigger a discounted offer. goodmoves picks the best option from a list of possible
actions. Here, the algorithm has learned that a 5 % discount is the most profitable way to prevent this customer from churn.


#### Communication flow with goodmoves SaaS
goodmoves uses reinforcement learning to learn about the customers’ behaviour and suggest the next best action.
This is achieved through a state-action-reward cycle. Here, the state represents the available information about a customer,
for which the decision (an action) has to be made. It consists of customer data like an ID, demographic information,
historic data (such as former purchases) and can be supplemented by additional features such as the time of day.

[img]

In the example case from the previous chapter, a customer is in danger of cancelling a contract. When requesting an
action for that customer to prevent churn, you send us the state. In addition, we need the list of allowed actions to
choose from, depending on the current circumstances. For instance, you might want to exclude actions that would not be
suitable for this customer due to company policies.

[img]
[img]

Based on the state, goodmoves responds with a prioritised list of actions for a given customer. These recommendations
let you know what the optimal next best action is. In our example, that means to offer a 5% discount on contract renewal.

To allow the goodmoves AI to learn from interactions, it needs to be made aware whether the selected action was successful
in practice. This is the reward. The machine learning algorithm immediately incorporates this feedback,
continuously improving its recommendations.

[img]


### Integration

#### Important data
A few concepts are key to the use of our service. In this chapter, we provide short definitions and explain their usage
within goodmoves.

state
The state is a dictionary that contains information that can be used to characterize an individual customer. It can include
customer profile data like contract type, contract duration and payment histories as well as personal data like age or gender.
actions
Actions are strings that identify a set of alternative communication options to choose from for a specific customer.
The goodmoves algorithm can then be used to rank the list of actions according to their predicted rate of success.
reward
The reward, a single number, is a measure for the actual success, e.g., the observed outcome after taking a specific action.
It provides feedback to the AI and is used to improve the algorithm and monitor its performance.
query_id
Our service requires a unique identifier that must be consistently used over the course of each action and reward cycle:
the query_id. It is an arbitrary string that allows us to relate the separate calls to our API for action request,
action update, and reward submission.


[img]


Ways to integrate


What an end-to-end solution will look like highly depends on the software and processes present. Here are some basic
approaches on how to integrate goodmoves within your current system:


GDPR Notice
We strive for responsible data handling. The use of our service does not require sensitive customer data like names or
addresses. It falls within the responsibility of our clients to implement adequate schemes for anonymization.
When provided with anonymized data, it is not possible to relate customer information to a real person with reasonable
effort, or even at all, within the goodmoves SaaS.


### API Docs
#### Introduction
We offer a RESTful API to interact securely with our service. Its URL includes the major version number of the software
before specifying the actual endpoint. In case of version 2 and the action endpoint, the URL has the following form:
https://api.goodmoves.ai/v3/action/l59sdl42gkj

#### Versioning
Currently, our API is available in version 3.1.0. We strive for careful version updates with backwards compatibility.
To this end, we will keep previous versions of the API available for a reasonable migration period.

#### Authentication
FIXME: JWT AUTH
The API client authenticates with API keys. You will be granted access to https://goodmoves.ai/auth/login once you sign
up for our service, where you will find information about your current API keys.
Error handling
If a problem occurs, the API will respond with a corresponding HTTP status code. In case the operation fails for only a
few resource items when used in batch mode, but is successful for the remainders, the response code will be 400. We will
return a list telling you which batch items had failed.


#### Endpoints 
##### `GET /` Retrieve version information of the API
Calling this endpoint returns basic API version information.

Responses

``` json
```

#### Endpoints NBA
##### `POST /action/{case_id}` Request a prioritised list of actions
Return a sorted list of prioritised NBAs, best recommendation first.
State history shall be provided by clients and can be retrieved in multiple ways, depending on the existing tracking
setup and privacy requirements:
- SST (server side tracking)
- Client cookie
- Google Analytics (3 or 4)
- Other tracking providers

Request body

``` json
{
  "query_id": "af76e5",
  "consumer_id": "w3e0-p8xc",
  "state": {
    "contract": "XL", 
    "age": 37,
  },
  "actions": [
    "actionA",
    "actionB",
    "actionN",
  ],
  "limit": 3,
  "test_api": false,
  "status": 1
}
```

Responses

``` json
{
  "query_id": "af76e5",
  "actions": [
    "actionC",
    "actionA",
    "actionB",
  ],
  "status": 1
}
```

##### `POST /action/{case_id}/select` Tell goodmoves which action was selected
Record the action that was selected by the client from the list of actions returned by a previous call to POST /action.
Request body

``` json
{
  "query_id": "af76e5",
  "action": "actionC",
  "test_api": false
}
```

Responses

``` json
{
  "status": 1
}
```


##### `POST /reward/{case_id}` Send a reward related to customer
Process a rewards signal for the given query_id.
Request body

``` json
{
  "query_id": "af76e5",
  "consumer_id": "w3e0-p8xc",
  "event_type": "add_to_basket",
  "value": 1.337,
  "test_api": false
}
```

Responses
``` json
{
  "status": 1
}
```


#### Endpoints NBA Batch
##### `POST /action/{case_id}/batch` Request a prioritised list of actions
Return a sorted list of prioritised NBAs, best recommendation first.
State history shall be provided by clients and can be retrieved in multiple ways, depending on the existing tracking
setup and privacy requirements:
- SST (server side tracking)
- Client cookie
- Google Analytics (3 or 4)
- Other tracking providers

Request body

``` json
{
  "customers": [
    {
      "query_id": "af76e5",
      "consumer_id": "w3e0-p8xc",
      "state": {
        "contract": "XL", 
        "age": 37,
      }
    },
  ],
  "actions": [
    "actionA",
    "actionB",
    "actionN",
  ],
  "limit": 3,
  "test_api": false
}
```

Responses

``` json
[
  {
    "query_id": "af76e5",
      "actions": [
        "actionC",
        "actionA",
        "actionB",
      ]
  },
]
```

##### `POST /action/{case_id}/select/batch` Tell goodmoves which action was selected
Record the action that was selected by the client from the list of actions returned by a previous call to POST /action.
Request body

``` json
{
  "selection": [
    {
      "query_id": "af76e5",
      "action": "actionC"
    },
  ],
  "test_api": false
}
```

Responses

``` json
{
  "status": 1
}
```


##### `POST /reward/{case_id}/batch` Send a reward related to customer
Process a rewards signal for the given query_id.
Request body

``` json
{
  "rewards":[
    {
      "query_id": "af76e5",
      "consumer_id": "w3e0-p8xc",
      "event_type": "add_to_basket",
      "value": 1.337
    },
  ],
  "test_api": false
}
```

Responses
``` json
{
  "status": 1
}
```

#### Endpoints Recommendations
##### `POST /recommendation/{case_id}`

Request body
``` json
{
  "limit": 3,
  "test_api": false,
  "customer_id": "c62wz-sr8vq-91yr7-exadpjx6ll",
  "query_id": "af76e514-f597-e73b-fb49-d61573d2f63a",
  "state": {
    "page_id": "products/529385c",
    "search_strings": [
      "Couch",
      "Chair"
    ],
    "products_viewed": [
      "876539a",
      "404955e",
      "019643a"
    ],
    "products_scrolled": [
      "876539a",
      "404955e",
      "019643a"
    ],
    "products_basket": [
      "876539a",
      "404955e",
      "019643a"
    ],
    "products_bought": [
      "876539a",
      "404955e",
      "019643a"
    ]
  }
}
```
Response

``` json
{
  "status": 0,
  "query_id": "af76e514-f597-e73b-fb49-d61573d2f63a",
  "actions": [
    "actionC"
  ],
  "status": 1
}
```
##### `POST /feed/{case_id}`

Response

``` json
{
  "status": 1
}
```


##### `POST /feed/{case_id}/update`

Response

``` json
{
  "status": 1
}
```

