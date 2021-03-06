Version: 2.3.9
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
goodmoves uses reinforcement learning to learn about the customers??? behaviour and suggest the next best action.
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





### API Docs
#### Introduction
We offer a RESTful API to interact securely with our service. Its URL includes the major version number of the software
before specifying the actual endpoint. In case of version 2 and the action endpoint, the URL has the following form:
https://api.goodmoves.ai/2/action

#### Versioning
Currently, our API is available in version 2.3.9. We strive for careful version updates with backwards compatibility.
To this end, we will keep previous versions of the API available for a reasonable migration period.

#### Authentication
The API client authenticates with API keys. You will be granted access to https://goodmoves.ai/auth/login once you sign
up for our service, where you will find information about your current API keys.
Error handling
If a problem occurs, the API will respond with a corresponding HTTP status code. In case the operation fails for only a
few resource items when used in batch mode, but is successful for the remainders, the response code will be 400. We will
return a list telling you which batch items had failed.


#### Endpoints
#### `POST /action` Request a prioritised list of actions
Return a sorted list of prioritised NBAs, best recommendation first.
State history shall be provided by clients and can be retrieved in multiple ways, depending on the existing tracking
setup and privacy requirements:
- SST (server side tracking)
- Client cookie
- Google Analytics (3 or 4)
- Other tracking providers

Request body

[img]

Responses

[img]

#### `PATCH /action` Tell goodmoves which action was selected
Record the action that was selected by the client from the list of actions returned by a previous call to POST /action.
Request body

[img]

Responses

[img]


#### `POST /reward` Send a reward related to customer
Process a list of rewards of a certain value for the given query_id.
Request body

[img]

Responses

[img]

#### `GET /` Retrieve version information of the API
Calling this endpoint returns basic API version information.
Request body

[img]

Responses

[img]
