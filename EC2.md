# EC2 (Elastic Cloud Compute)
resizable compute capacity in the cloud (cloud based virtual machines). Allows you to quickly scale up and down.

Changed the startup scene by making it very cheap to get set up on machines. Fast and low cost because you don't have to buy and set up physical machines.

#### EC2 Options
1. On Demand - fixed rate by the hour with no commitment
1. Reserved - provides a capacity reservation and offers a significant discount on the hourly charge for an instance. 1 or 3 year terms
1. Spot - enables you to bid whatever price you want for instance capacity, providing for even greater savings if your applications have flexible start and end times. (You are bidding for excess computing capacity)
1. Dedicated Hosts - Physical EC2 server dedicated for your use. Can help reduce costs when you have legacy server-bound software licenses.

##### On Demand
- You want low cost and flexibility with low up-front payment or long-term commitment
- Good for applications with short-term, spiky or unpredictable workloads that cannot be interrupted OR being developed/tested on EC2 for the first time

##### Reserved
- Applications with steady state or predictable usage
- Applications that require reserved capacity
- Able to make up front payments to reduce the total computing costs

##### Spot
- Applications that have flexible start and end times
- Applications that are only feasible at very low compute prices (Pharmacutical number crunching, etc)
- If an instance is terminated by Amazon because the spot price went above your bid price, you will not be charged for the partial hour of usage.
- If you terminate the instance yourself, you will be charged for any hour in which the instance ran


##### Dedicated Hosts
- Useful for regulatory requirements that may not support multi-tenant virtualization
- Great for licensing that does not support multi-tenancy or cloud deployments
- Can be purchased on-demand, or as a reservation for up to 70% off the on demand price
