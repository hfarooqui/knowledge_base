# Well Architected AWS Framework

## Benefits of Cloud
- Almost zero upfront infrastructure investment
- Just-in-time infrastructure
- Efficient resource utilization
- Usage-based costing
- Reduced time to market
- Automation - "Infrastructure as Code"
- Auto scailing
- Proactive scaling
- Efficient development cycle
- Improved testability
- DR and business continuity
- "Overflow" the traffic to the cloud
![Securing_Applications](https://s3.amazonaws.com/hfcontents/kbimages/Securing_Applications.png "Securing_Applications")


## Design for failure
- Be a pessimist. Assume things will fail. In other words always design, implement and deploy for automated recovery from failure
- In perticular assume
   - Hardware failure
   - Application failure
   - Outages
   - More than expected number of requests per second some day
 - Netflix has Choes Monkey, choes snail

## Decouple your application
- Loose coupling isolates verious layers and components of your application so that each component intrects asynchronously with others and treates them as blackbox such that if one component fails other whould continue to work (SQS)
- For example you would isolate Web server, App server and DB server

## Elasticity
Can be implemented in following ways
- Proactive Cyclic scaling: Periodic scaling that occurs at fixed interval
- Proactive Event-based scaling: Scaling only when you are expecting big surge of traffic (new product launch, marketing campaign, black friday sale)
- Auto scaling: By using monitoring service, you system can send triggers to take appropriate action so that it scales up or down based on the matrics

## Secure your application
![Securing_Applications](https://s3.amazonaws.com/hfcontents/kbimages/Securing_Applications.png "Securing_Applications")
