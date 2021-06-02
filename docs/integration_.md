### Integration

#### Important data
A few concepts are key to the use of our service. In this chapter, we provide short definitions and explain their usage
within goodmoves.


*state*


The state is a dictionary that contains information that can be used to characterize an individual customer. It can include
customer profile data like contract type, contract duration and payment histories as well as personal data like age or gender.


*actions*


Actions are strings that identify a set of alternative communication options to choose from for a specific customer.
The goodmoves algorithm can then be used to rank the list of actions according to their predicted rate of success.


*reward*


The reward, a single number, is a measure for the actual success, e.g., the observed outcome after taking a specific action.
It provides feedback to the AI and is used to improve the algorithm and monitor its performance.
<br>
*query_id*


Our service requires a unique identifier that must be consistently used over the course of each action and reward cycle:
the query_id. It is an arbitrary string that allows us to relate the separate calls to our API for action request,
action update, and reward submission.
<br>


> Sources of Data
> 
> For a seamless integration, it is important to know where you can get the necessary data from.
> The larger a company, the more tools and systems may be at work. Consider which would support real-time interaction best, and which allow you to inform the recipient (employee or customer) about the next best action.
> 
> Here are some suggestions: 
>  - Product feeds
>  - Google Tag Manager
>  - CRM software
>  - campaign management
>  - marketing automation
>  - e-commerce-suite
>  - data warehouse / data lake


## Ways to integrate


What an end-to-end solution will look like highly depends on the software and processes present. Here are some basic
approaches on how to integrate goodmoves within your current system:

![](./static/Integration-Ways.png)


## GDPR Notice
We strive for responsible data handling. The use of our service does not require sensitive customer data like names or
addresses. It falls within the responsibility of our clients to implement adequate schemes for anonymization.
When provided with anonymized data, it is not possible to relate customer information to a real person with reasonable
effort, or even at all, within the goodmoves SaaS.