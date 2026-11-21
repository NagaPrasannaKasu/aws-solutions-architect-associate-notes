# AWS EC2 Purchase Options – Simplified Study Notes

These notes explain the EC2 purchasing options from the original rough notes in a clearer, easy-to-revise format.

> **Pricing note:** Discount percentages below are the indicative figures recorded in the original notes. Actual discounts and prices depend on the current AWS offer, Region, instance family, operating system, and commitment details. Check AWS pricing before making a purchasing decision.

## 1. On-Demand Instances

- Pay for compute capacity as you use it.
- No long-term commitment is required.
- No upfront payment is required.
- Pricing is predictable based on usage, but the cost per unit is generally higher than commitment-based options.
- Suitable for:
  - Short-term workloads.
  - Workloads with unpredictable demand.
  - Applications that need to run without a long-term commitment.

**Remember:** Flexible usage, no commitment, usually higher cost.

## 2. Reserved Instances (1 or 3 years)

- Commit to using specified EC2 capacity attributes for a term of **1 year or 3 years**.
- The original notes record discounts of up to **72%** compared with On-Demand pricing.
- The reservation is based on attributes such as:
  - Instance type.
  - AWS Region.
  - Tenancy.
  - Operating system / platform.
- Payment options:
  - **No Upfront:** Pay over the reservation term; usually the smallest initial payment.
  - **Partial Upfront:** Pay part of the cost at the beginning and the rest over the term.
  - **All Upfront:** Pay the full amount at the beginning; generally offers the largest payment-related discount.
- Reserved Instance scope can be **Regional** or **Zonal**.
- Suitable for steady, predictable workloads that are expected to run for a long period, such as a database or application server.
- Reserved Instances can be bought or sold through the Reserved Instance Marketplace, subject to AWS Marketplace conditions.

**Remember:** Long-term, predictable usage in exchange for a potential discount.

### 2.1 Convertible Reserved Instances

- A type of Reserved Instance that provides more flexibility to change the reservation configuration.
- The original notes record discounts of up to **66%**.
- Changes may include:
  - Instance family or type.
  - Operating system / platform.
  - Scope.
  - Tenancy.
- Useful when you need a long-term commitment but may need to change the instance configuration during the reservation term.

**Remember:** More flexibility than Standard Reserved Instances, with a potentially smaller discount.

## 3. Savings Plans (1 or 3 years)

- Commit to a certain amount of eligible compute usage, measured in **dollars per hour**, for 1 or 3 years.
- You commit to a level of spend rather than reserving one exact instance configuration.
- The original notes record discounts of up to **72%**.
- The notes describe the commitment as being tied to a specific instance family and AWS Region; check the selected Savings Plan type because flexibility differs by plan type.
- Suitable for long-running workloads with predictable baseline usage.

**Remember:** Commit to a consistent hourly spend instead of reserving a particular instance.

## 4. Spot Instances

- Use spare AWS EC2 capacity at a much lower price than On-Demand.
- The original notes record discounts of up to **90%**.
- AWS can interrupt a Spot Instance when it needs the capacity back, so the instance may not be available for the full time you want.
- Suitable for flexible, interruption-tolerant workloads, such as:
  - Batch jobs.
  - Data analysis.
  - Image processing.
  - Distributed workloads.
  - Workloads with flexible start and end times.
- Not suitable for workloads that cannot tolerate interruption, such as critical databases or applications that require continuous, uninterrupted compute.
- A Spot request can be cancelled in supported request states (the original notes list Open, Active, and Disabled). Check the request and instance status separately; cancelling a request and terminating a running instance are not necessarily the same action.

**Remember:** Very low cost, but capacity can be interrupted.

## 5. Dedicated Hosts

- Reserve an entire physical server dedicated to your use.
- Provides visibility and control over the underlying physical server and instance placement.
- Can help meet certain compliance requirements.
- Supports Bring Your Own License (BYOL) scenarios for eligible server-bound software licences, such as licences based on:
  - Physical sockets.
  - Physical cores.
  - Individual virtual machines.
- Purchase options:
  - **On-Demand:** Pay for the Dedicated Host while it is active.
  - **Reserved:** Commit for 1 or 3 years, with No Upfront, Partial Upfront, or All Upfront payment options.
- The original notes describe Dedicated Hosts as one of the most expensive EC2 purchasing options.
- Suitable for workloads with specific licensing, compliance, or physical-server placement requirements.

**Remember:** A whole physical server is dedicated to you, with greater control over placement and licensing.

## 6. Dedicated Instances

- EC2 instances run on hardware dedicated to a single AWS customer.
- Other AWS customers do not share that dedicated hardware.
- Instances belonging to the same AWS account may share the dedicated hardware with one another.
- You do not get the same control over physical server placement as with Dedicated Hosts.
- AWS may move an instance to different hardware after a stop/start operation.

**Remember:** Dedicated hardware for your account, but less physical-server control than a Dedicated Host.

## 7. On-Demand Capacity Reservations

- Reserve EC2 capacity in a **specific Availability Zone (AZ)**.
- Helps ensure that EC2 capacity is available when you need to launch instances in that AZ.
- Can be created or cancelled at any time; there is no 1-year or 3-year term commitment.
- A Capacity Reservation does **not** provide a billing discount.
- You are charged at the applicable On-Demand rate whether or not you launch an instance into the reserved capacity.
- The original notes mention combining Capacity Reservations with Regional Reserved Instances or Savings Plans:
  - Capacity Reservation helps with **capacity availability**.
  - Reserved Instances or Savings Plans may help with **eligible usage discounts**.
- Suitable for short-term or ongoing workloads that must have capacity available in a particular AZ.

**Remember:** Reserves capacity in a chosen AZ; it does not itself reduce the price.

## 8. Spot Fleet

- A Spot Fleet is a collection of Spot capacity requests that can launch multiple EC2 instances.
- It can use Spot Instances and, optionally, On-Demand Instances.
- You define a target capacity and constraints such as price.
- A fleet can choose from multiple launch pools. A launch pool is a possible combination of instance type, operating system, and Availability Zone.
- The fleet selects suitable pools according to the allocation strategy you choose.
- It stops launching more instances when it reaches the target capacity or the configured limits.

### 8.1 Spot Fleet Allocation Strategies

| Strategy | How it works | Main benefit / use |
|---|---|---|
| **Lowest Price** | Selects capacity from the pool with the lowest price. | Cost optimisation for flexible, short workloads. |
| **Diversified** | Spreads instances across several pools. | Better resilience because capacity is distributed across pools. |
| **Capacity Optimized** | Selects pools based on available capacity. | Helps reduce the chance of interruption due to limited capacity. |
| **Price Capacity Optimized** | Prioritises pools with the strongest available capacity, then considers price. | Balances capacity availability and cost; the original notes describe this as the recommended general-purpose choice. |

**Remember:** Spot Fleet can select from multiple pools automatically, according to your target capacity and allocation strategy.

## 9. Quick Comparison – Which Option Should I Choose?

| EC2 purchase option | Best suited for | Main trade-off |
|---|---|---|
| **On-Demand** | Short-term or unpredictable workloads. | Flexible, but generally higher cost. |
| **Reserved Instances** | Steady, predictable workloads over 1 or 3 years. | Lower cost in exchange for a commitment. |
| **Convertible Reserved Instances** | Long-term workloads that may need configuration changes. | More flexibility, potentially less discount. |
| **Savings Plans** | Predictable compute usage over 1 or 3 years. | Requires a usage-spend commitment. |
| **Spot Instances** | Flexible, interruption-tolerant workloads. | Low cost, but instances can be interrupted. |
| **Dedicated Hosts** | BYOL, compliance, or physical-server placement needs. | Greater control, but typically expensive. |
| **Dedicated Instances** | Workloads requiring dedicated hardware without host-level placement control. | Dedicated hardware, less placement control. |
| **Capacity Reservations** | Workloads that need EC2 capacity in a particular AZ. | Capacity assurance, but no discount by itself. |
| **Spot Fleet** | Workloads that can use several Spot pools and need a target capacity. | Automated pool selection, but Spot capacity remains interruptible. |

## 10. Quick Revision Points

- **On-Demand:** No long-term commitment; pay for what you use.
- **Reserved Instances:** Commit for 1 or 3 years for a potential discount.
- **Convertible Reserved Instances:** More flexibility to change reservation attributes.
- **Savings Plans:** Commit to a defined amount of eligible compute spend per hour.
- **Spot Instances:** Low cost, but AWS can interrupt them.
- **Dedicated Hosts:** Entire physical server dedicated to you; useful for certain BYOL and compliance needs.
- **Dedicated Instances:** Run on hardware dedicated to one AWS customer, without Dedicated Host placement control.
- **Capacity Reservations:** Reserve capacity in a specific AZ; no billing discount by themselves.
- **Spot Fleet:** Uses multiple launch pools and an allocation strategy to meet target capacity.

## 11. Exam Scenario Clues

- A workload is short-term and unpredictable → **On-Demand Instances**.
- A workload is steady and predictable for years → **Reserved Instances or Savings Plans**.
- The configuration may need to change during a reservation term → **Convertible Reserved Instances**.
- A batch or data-processing job can tolerate interruption → **Spot Instances**.
- A software licence is tied to physical sockets or cores → **Dedicated Hosts** may be appropriate.
- EC2 capacity must be available in one specific AZ → **On-Demand Capacity Reservation**.
- A workload can use several Spot pools and needs automated capacity selection → **Spot Fleet**.

---

*Study note: These notes are based on the supplied rough notes. Treat discount percentages as indicative rather than guaranteed, and check current AWS documentation and pricing for exact terms before purchasing.*
