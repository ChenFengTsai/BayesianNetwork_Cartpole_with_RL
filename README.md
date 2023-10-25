# BayesianNetwork_Cartpole_with_RL

## Overview
This project is a reproduction of [this paper](https://pubmed.ncbi.nlm.nih.gov/36850617/). It aims to leverage the principles of Bayesian networks to provide a comprehensive explanation for the behavior exhibited by an agent operating within RL algorithm. It seeks to bridge the gap between the complex dynamics of RL algorithms and human-understandable explanations by utilizing the explanatory power of Bayesian networks. Algorithm we used includes DDQN(for RL agent), Dagma(for structural learning), LSTM(for distal information extraction).

## Prerequisites
1. Clone the repository
   ```
   git clone https://github.com/ChenFengTsai/BayesianNetwork_Cartpole_with_RL.git
   ```
   
2. Install dependicies
* with poetry
  ```
  poetry install 
  ```
* with pip install
  ```
  pip install requirements.txt
  ```

## Usage
1. DDQN
   * First, We use DDQN to train the agent in the Cartpole environment.(Code was provided under the `ddqn` folder)
   * Second, we use the trained agent to explore the game and save those (acutal) footprints as memory (memory data are saved under the `Model_memory` folder)
   * Third, we use the trained agent to create counterfactual footprints as counterfactual memory to build the counterfacual RNN model (will mention it below). The term "counterfactual" means that we have to manually interfere with the behavior of the agent when it is exploring the gaming environment. The reason behind that is to understand the Q-value if it does not follow the optimal strategy, which called counterfactual Q-value and could be used to compare with actual Q-value for decision explanation.

2. Bayesian network
   * First, we use K-means to categorize the memories (extracted from the DDQN agent) into six groups (since structural learning works better in discrete environment)
   * Second, we use the categorized memories to do structural learning to find out the realtionship between each feature, actions and rewards(Bayesian network). The structural learning algorithm we used is Dagma since it is newer than NOTEARS. You could find all the Bayesian Network code under the `bn` folder

3. LSTM
   * First, we build one LSTM model for the actual distal information with label as Q-value and feature as the categorized agent attributes such as pole angle, pole speed.
   * Second, we use the counterfactual memory data to build another one LSTM model for the counterfactual distal information with the counterfactual Q-value and the respective features.
4. Aggregate everything for decision explanation
   * First, run the DDQN agent in the gaming environment and collect the past footprints.
   * Second, feed the current step to the Bayesian network to find the potential next action.
   * Third, feed the potential next action gained from the Bayesian network and the previous steps to the actual LSTM model to get the actual distial information, which is the actual Q-value.
   * Forth, feed the counterfactual action you picked (which is different form the potential next action) and the previous steps to the counterfactual LSTM model to get the counterfactual distal information, which is the counterfactual Q-value.
   * Fifth, aggregated all these information to make decision explanation for why choosing certain action is better than the other opions.
   




