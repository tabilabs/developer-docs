# Captain Node

## Captain Node Mining Mechanism

### Core Logic
#### Mining Reward Calculation
General parameter information is as follows:
#### Calculation Steps
1. Daily Tabi Issuance Calculation (DailyTabiIssuance):
- DailyTabiIssuance = InflationBase * InflationRate / BlocksPerYear * BlocksPerDay
2. Halving Cycle Calculation (Halving Cycle):
- If CaptainNodeMintTotalCount >= InflationBase * ClaimsInitRewardRate * 0.5, set Halving Cycle to 0.5; otherwise, set to 1
- If InflationBase * ClaimsInitRewardRate * 0.5 >= CaptainNodeMintTotalCount >= InflationBase * ClaimsInitRewardRate * 0.75, set Halving Cycle to 0.25; otherwise, set to 0.5
3. Technological Progress Factor Calculation (Technological Progress Factor):
- Technological Progress Factor = 1.6 ^ (CurrentMaxLevelMine - 1)
4. Global Emission Base Value Calculation (CaptainNodeDailyBaseEmission):
- CaptainNodeDailyBaseEmission = ConstentA * Technological Progress Factor * Halving Cycle
5. The Global Operational Rate Calculation:
- The Global Operational Rate = Number of units meeting the threshold per day / Total units sold
    - If the operational rate is less than 0.1, set to 0.1
    - If the operational rate is greater than or equal to 0.1, use the calculated value
6. Personal Stake Rate Calculation (Personal Stake Rate):
- Personal Stake Rate = PledgeTotalCountFromXN / MintTotalCountFromXN
7. Global Pledge Total Calculation (PledgeTotalCount):
- PledgeTotalCount = PledgeTotalCountFromX1 + ... + PledgeTotalCountFromXn
8. Global Emission Total Output Calculation (MintTotalCount):
- MintTotalCount = MintTotalCountFromX1 + ... + MintTotalCountFromXn
9. Global Staking Rate Calculation (Stake Rate):
- Stake Rate = PledgeTotalCount / ClaimTotalCount
    - If the global staking rate is less than 0.3, set to 0.3
    - If the global staking rate is greater than or equal to 0.3, use the calculated value
10. Daily Total Output Calculation:
- Daily Total Output = CaptainNodeDailyBaseEmission * (The Global Operational Rate ^ 1) * (Stake Rate ^ 1)
- CaptainNodeBlockActualEmission = Daily Total Output / BlocksPerDay
11. Individual Operation Rate Calculation:
- Individual Operation Rate = OnlinePeriodFromXn / MaximumPowerOnPeriod
12. Node Mining Power Calculation (Node Mining Power):
- Node Mining Power = (Base Node Power + Reward Node Power) * (e ^ (Personal Stake Rate / 0.5)) * Individual Operation Rate
    - If Personal Stake Rate > 100%, use 100%
13. The Global Mining Power Calculation (The Global Mining Power):
- The Global Mining Power = ComputingPowerForX1 + ... + ComputingPowerForXn
14. Mining Power Ratio Calculation (Mining Power Ratio):
- Mining Power Ratio = Node Mining Power / ComputingPower
15. Node Emission Output Calculation (CaptainNodeDailyReward):
- CaptainNodeDailyReward = Daily Total Output * Mining Power Ratio
- CaptainNodeBlockReward = CaptainNodeBlockActualEmission * Mining Power Ratio
#### Reward Distribution
- Distribution Implementation: On-chain calculations only, rewards must be manually claimed by users.
#### Governance Parameters
- TotalCountCaptains: 200,000 (maximum number of nodes)
- MinimumPowerOnPeriod: 6
- MaximumPowerOnPeriod: 24
- ConstentA: 9,000,000
- CurrentLevelForSale: 1
#### Running Time Calculation
- Running Time Coefficient: Actual running time / 24
    - Assuming: Running time coefficient is y, actual running time is x (x >= 6), maximum running time is constant k=24
    - Then: y = x/k (x>=6) or y = 0 (x<6)
##### Forecaster Layer
      The Tabi team has implemented a Forecaster layer to collect the operating status of all Captain Nodes, submitting this information at a set epoch. The current epoch is 24 hours UTC.

#### Node Power Rewards
Users can earn node power rewards in the following ways:
- Mining Commission #Direct Referral: Users (addresses) invite others to purchase Captain Nodes and receive unclaimed node power rewards.
- Node Power Bonus #Self Purchase: Users (addresses) receive additional node power through invitation codes or coupons when purchasing Captain Nodes themselves.
- Node Power Reward #Game: Users (addresses) enter Tabi Node, select game tasks, and upon completion, receive corresponding unclaimed node power rewards.