# Mining Reward Calculation Logic

Mining rewards for Tabi Captain Nodes are calculated based on multiple parameters that determine both the total network emissions and individual node-specific outputs. Here is the core logic behind these calculations:

**General Parameters:**

| Parameter                  | Value      | Description                             |
| -------------------------- | ---------- | --------------------------------------- |
| **TimoutCommit**           | 3s         | Block time                              |
| **BlocksPerDay**           | 28,800     | Number of blocks per day                |
| **BlocksPerYear**          | 10,519,200 | Number of blocks per year               |
| **ConstentA**              | 9,000,000  | Fixed constant                          |
| **ClaimsInitRewardRate**   | 40%        | Initial reward pool allocation          |
| **MaximumPowerOnPeriod**   | 24         | Maximum node operation period           |
| **InflationBase**          | 10 billion | Base for inflation calculation          |
| **InflationRate**          | 3%         | Annual inflation rate                   |
| **CurrentTotalTabi**       | Dynamic    | Total current $veTABI + $Tabi           |
| **CurrentLevelForSale**    | 1          | Current technological progress level    |
| **CurrentRunDays**         | 60         | Number of days since system launch      |
| **PledgeTotalCountFromXn** | Dynamic    | Total amount staked per user            |
| **MintTotalCountFromXn**   | Dynamic    | Total mining output per user            |
| **OnlinePeriodFromXn**     | Dynamic    | Node uptime (Individual Operation Rate) |

#### **Reward Calculation Steps**

**1. Daily Tabi Issuance Calculation:**

$$
\text{DailyTabiIssuance} = \frac{\text{InflationBase} \times \text{InflationRate}}{\text{BlocksPerYear}} \times \text{BlocksPerDay}
$$

2\. **Halving Cycle Calculation**:

If CaptainNodeMintTotalCount ≥(InflationBase \* ClaimsInitRewardRate \* 0.5), Halving Cycle = 0.5 Else, Halving Cycle = 1

If (InflationBase \* ClaimsInitRewardRate \* 0.5) ≥CaptainNodeMintTotalCount ≥(InflationBase \* ClaimsInitRewardRate \* 0.75), Halving Cycle = 0.25 Else, Halving Cycle = 0.5

3\. **Technological Progress Factor Calculation**:

TechProgressCoefficient = 1.6 ^ (CurrentMaxLevelMine - 1)

4\. **Global Base Emission Calculation**:

CaptainNodeDailyBaseEmission = ConstentA \* TechProgressCoefficient \* Halving Cycle

5\. **Global Operational Rate Calculation**:

$$
\text{OperationalRate} = \frac{\text{Nodes meeting threshold per day}}{\text{Total activated nodes}}
$$

If the operational rate is less than 0.1, it is set to 0.1.

If it is greater than or equal to 0.1, the calculated value is used.

6\. **Personal Stake Rate Calculation**:

$$
\text{Personal Stake Rate} = \frac{\text{PledgeTotalCountFromXN}}{\text{MintTotalCountFromXN}}
$$

* This rate is based on the user's total stake across validators and their node's mining output.

7\. **Global Staking Total Calculation**:

$$
\text{PledgeTotalCount} = \sum_{i=1}^{n} \text{PledgeTotalCountFromXi}
$$

**8. Global Mining Output Calculation:**

$$
\text{MintTotalCount} = \sum_{i=1}^{n} \text{MintTotalCountFromXi}
$$

9\. **Global Staking Rate Calculation**:

$$
\text{Global Staking Rate} = \frac{\text{PledgeTotalCount}}{\text{ClaimTotalCount}}
$$

**10. Daily Total Emission Calculation:**

CaptainNodeDailyActualEmission = CaptainNodeDailyBaseEmission \* OperationalRate \* Global Staking Rate

The per-block emission is calculated as:

$$
\text{CaptainNodeBlockActualEmission} = \frac{\text{CaptainNodeDailyActualEmission}}{\text{BlocksPerDay}}
$$

11\. **Node Uptime Calculation**:

$$
\text{Individual Operation Rate} = \frac{\text{OnlinePeriodFromXn}}{\text{MaximumPowerOnPeriod}}
$$

12\. **Node Mining Power Calculation**:

$$
(\text{Base Node Power} + \text{Reward Power}) \times \left( e^{6.66 \times \text{Personal Stake Rate}} \right) \times \text{Individual Operation Rate}
$$



If **Personal Stake Rate** > 100%, cap at 100%.

13\. **Global Mining Power Calculation**:

$$
\text{Global Mining Power} = \sum_{i=1}^{n} \text{Node Mining Power}
$$

14\. **Mining Power Ratio Calculation**:

$$
\text{Mining Power Ratio} = \frac{\text{Node Mining Power}}{\text{Global Mining Power}}
$$

15\. **Node Mining Reward Calculation**:

CaptainNodeDailyReward = CaptainNodeDailyActualEmission \* Mining Power Ratio

The per-block mining reward:

CaptainNodeBlockReward = CaptainNodeBlockActualEmission \* Mining Power Ratio
