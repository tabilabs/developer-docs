# Mining Reward Calculation Logic

Mining rewards for Tabi Captain Nodes are calculated based on multiple parameters that determine both the total network emissions and individual node-specific outputs. Here is the core logic behind these calculations:

**General Parameters:**

| Parameter                  | Value     | Description                             |
| -------------------------- | --------- | --------------------------------------- |
| **ConstentA**              | 9,000,000 | Fixed constant                          |
| **ClaimsInitRewardRate**   | 40%       | Initial reward pool allocation          |
| **MaximumPowerOnPeriod**   | 24        | Maximum node operation period           |
| **InflationBase**          | 6 billion | Base for inflation calculation          |
| **InflationRate**          | 3%\~5%    | Annual inflation rate                   |
| **CurrentTotalTabi**       | Dynamic   | Total current $veTABI + $Tabi           |
| **CurrentLevelForSale**    | 1         | Current technological progress level    |
| **CurrentRunDays**         | 60        | Number of days since system launch      |
| **PledgeTotalCountFromXn** | Dynamic   | Total amount staked per user            |
| **MintTotalCountFromXn**   | Dynamic   | Total mining output per user            |
| **OnlinePeriodFromXn**     | Dynamic   | Node uptime (Individual Operation Rate) |

#### **Reward Calculation Steps**

1\. **Halving Cycle Calculation**:

If CaptainNodeMintTotalCount ≥(InflationBase \* ClaimsInitRewardRate \* 0.5), Halving Cycle = 0.5 Else, Halving Cycle = 1

If (InflationBase \* ClaimsInitRewardRate \* 0.5) ≥CaptainNodeMintTotalCount ≥(InflationBase \* ClaimsInitRewardRate \* 0.75), Halving Cycle = 0.25 Else, Halving Cycle = 0.5

2\. **Tech Progress Factor**:

TechProgressFactor = 1.6 ^ (CurrentMaxLevelMine - 1)

3\. **Global Base Emission**:

CaptainNodeDailyBaseEmission = ConstentA \* TechProgressCoefficient \* Halving Cycle

4\. **Global Operation Rate** :

$$
\text{GlobalOperationRate} = \frac{\text{Nodes meeting threshold per day}}{\text{Total activated nodes}}
$$

If the global operation rate is less than 0.1, it is set to 0.1.

If it is greater than or equal to 0.1, the calculated value is used.

5\. **Personal Stake Rate**:

$$
\text{PersonalStakeRate} = \frac{\text{StakedTotalCount veTABI}}{\text{ClaimedTotal veTABI}}
$$

* This rate is based on the user's total stake across validators and their node's claimed $veTABI.

6\. **Global Staked Count**:

$$
\text{GlobalStakedCount} = \sum_{i=1}^{n} \text{StakedTotal veTABI}
$$

**7. Global Claimed Count:**

$$
\text{ GlobalClaimedCount} = \sum_{i=1}^{n} \text{ClaimedTotalCount veTABI}
$$

8\. **Global Staking Rate**:

$$
\text{GlobalStakingRate} = \frac{\text{StakedTotalCount}}{\text{ClaimedTotalCount}}
$$

**9. Daily Total Emission:**

CaptainNodeDailyEmission = CaptainNodeDailyBaseEmission \* OperationRate \* Global Staking Rate

10\. **Node Operation Rate**:

$$
\text{NodeOperationRate} = \frac{\text{OnlinePeriodFromXn}}{\text{24h}}
$$

11\. **Node Mining Power**:

$$
(\text{Base Node Power} + \text{Reward Power}) \times \left( e^{6.66 \times \text{Personal Stake Rate}} \right) \times \text{Node Operation Rate}
$$



If **Personal Stake Rate** > 100%, cap at 100%.

12\. **Global Mining Power**:

$$
\text{GlobalMiningPower} = \sum_{i=1}^{n} \text{NodeMiningPower}
$$

13\. **Mining Power Ratio**:

$$
\text{MiningPowerRatio} = \frac{\text{NodeMiningPower}}{\text{GlobalMiningPower}}
$$

14\. **Node Mining Reward**:

CaptainNodeDailyReward = CaptainNodeDailyActualEmission \* Mining Power Ratio



