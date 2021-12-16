# LSTM modeling of streamflow in snow/glacier melt dominated catchments
CSE 599 Deep Learning final project - Hordur Helgason
12/15/21

[**Project video overview (closer to 4 minutes, I suggest 1.5x!)**](https://youtu.be/iFWH0bqFewA)

## Introduction
Applications of DL methods in streamflow modeling have yielded very good results, with DL models substantially outperforming benchmark physically based models (e.g. Kratzert et al., 2019). DL methods of modeling hydrologic processes in catchments where runoff primarily originates from snow and glacier melt have not yet received much attention from the scientific community. Accurately modeling river discharge in snow/glacier melt-dominated catchments is highly important for water resource management.
In this final project, I trained an LSTM model to predict streamflow in snow/glacier melt-dominated catchments. I also analized the states and activations of a trained LSTM, to see if the LSTM internals resemble physically meaningful quantities. 

The aim of the project was to develop a model to be used in streamflow forecasts for hydropower operations in Iceland.

## Methods
### Data

#### Target data: Streamflow measurements
#### Input data: Weather model output

Streamflow measurements and weather model output data from a selection of 10 glacial rivers in Iceland were used for model development. The locations of the measurement gauges and corresponding watersheds can be seen in figure 1. 
<!-- <img width="700" alt="image" src="https://github.com/hhelgason/CSE599-DL-final-project/blob/main/docs/assets/css/layout_1.png"> -->
<!-- *Figure 1: Locations of gauges and corresponding watersheds used in this project* -->

| ![layout_1.png](https://github.com/hhelgason/CSE599-DL-final-project/blob/main/docs/assets/css/layout_1.png) | 
|:--:| 
| *Figure 1: Locations of the gauges and corresponding watersheds that are used in this project* |

Weather parameters from an atmospheric reanalysis dataset on a 2x2 km grid over Iceland were averaged over each watershed. The specific weather parameters used are temperature, precipitation, solid ratio of precipitation, wind speed, and downwelling shortwave and longwave radiation. The target variable is a timeseries of area-averaged streamflow values with a daily time resolution. 

The dataset was split as follows: 
<!-- (11 years: 1999-10-01- 2010-09-30), validation (5 years: 2010-10-01 to 2015-09-30) and test (4 years: 2015-10-01 to 2019-08-31) sets. -->

| Period | Start date  | End date  | # of years  |
| ------- | --- | --- | --- |
| Train | 1999-10-01 | 2010-09-30 | 11 |
| Validation | 2010-10-01 | 2015-09-30 | 5 |
| Test | 2015-10-01 | 2019-08-31  | 4 |


### Code
The [NeuralHydrology](https://github.com/neuralhydrology/neuralhydrology) python package was utilized to perform the experiments. The python package was developed by the AI for Earth Science group at Institute for Machine Learning, Johannes Kepler University. The package is built on top of Pytorch.

Model training was performed on Google Colab.

### Model setup
The models were trained on all 10 basins simultaniously.

Two model classes of the NeuralHydrology library were tested, the CudaLSTM (a network that uses the standard PyTorch LSTM implementation) and an entity-aware LSTM model (EA-LSTM). The EA-LSTM ingests static attributes of the watersheds and uses these attributes to compute the input gate activations. In this study, the glaciated area of each basin was used as a static attribute.

The model training was run for 50 epochs. The following hyperparameters of the model were kept fixed in all runs:

Learning rate: Epochs 1-29: 0.01, epochs 30-39: 0.005, epochs 40-50: 0.001

Loss: NSE 

Optimizer: Adam

Output dropout: 0.4

Sequence length: 365

Output activation: Linear

Batch size: 256

Various numbers of hidden layers were tested (20, 64, 96, 128, 196, 256).

Results are evaluated with the NSE error metric.

Finally, the internal states and gate activations of the LSTM were analyzed. 

<!-- ### Fine tuning
Once a good mean NSE value for all basins had been obtained, the model was fine-tuned individually for two specific basins to maximize the performance for these basins. The two basins were selected because of they represent the inflow into hydropower reservoirs. Fine tuning involves.....

Finally, the models were evaluated on the unseen test dataset. Also, the internal states and gate activations of the LSTM were analyzed. 
 -->
## Results
For the model classes that were tested, the entity-aware (EA-LSTM) performed better than the cuda-LSTM. For a hidden layer of 20, the cuda-LSTM resulted in a mean NSE of 0.64 vs. 0.72 for EA-LSTM. Thus, various numbers of hidden layers were tested for the EA-LSTM model. The training and validation losses are shown in figure 2, with higher run numbers corresponding to more hidden layers.

| ![train_and_valid_losses.PNG](https://github.com/hhelgason/CSE599-DL-final-project/blob/main/docs/assets/css/train_and_valid_losses.PNG) | 
|:--:| 
| *Figure 2: Train and validation losses for several EA-LSTM model runs. * |

The mean NSE for all 10 basin for these runs is shown in figure 3.
| ![mean_nse_.PNG](https://github.com/hhelgason/CSE599-DL-final-project/blob/main/docs/assets/css/mean_nse_.PNG) | 
|:--:| 
| *Figure 3: Mean NSE for several EA-LSTM model runs. * |

<!-- https://github.com/hhelgason/CSE599-DL-final-project/blob/main/docs/assets/css/mean_nse_.PNG -->
<!-- https://github.com/hhelgason/CSE599-DL-final-project/blob/main/docs/assets/css/train_and_val_losses.PNG -->

We see that after 50 epochs, the model with 256 hidden layers performs the best overall (mean NSE of all 10 catchments). The figures indicate that the model has not fully converged, so training for more epochs would potentially further improve the results. We see a spike in losses between epochs 20 and 30 and validation accuracy drops considerably. The reason for this is not clear. However, results for epochs 30-50 look reasonably good.

<!-- ## Fine-tuning
We select three gauges for fine-tuning, gauges no. 96, 112 and 221. We perform fine-tuning for the EA-LSTM model that gives the best results for each basin. For gauge 96, this is  

We fine-tune this model (EA-LSTM), hidden=256) for two of the basins. An improvement of  validation NSE xx and xx was obtained. The simulated streamflow for the validation and test periods is shown in figures x.  -->
Three of the gauges in the dataset represent inflow into hydropower reservoirs. We therefore use the EA-LSTM model with a hidden size that gives the best results for each basin. In table 1, we see the validation and test NSE scores of these gauges, as well as the corresponding number of hidden layers.

| Gauge | Val. NSE  | Test NSE  | Hidden  |
| ------- | --- | --- | --- |
| 96 | 0.789 | 0.719 | 20 |
| 112 | 0.734 | 0.711 | 128 |
| 221 | 0.845 | 0.814  | 256 |

In figures 4-6, we plot the simulated vs. measured disharge for each gauge, for the unseen test period. 

| ![VHM96.png](https://github.com/hhelgason/CSE599-DL-final-project/blob/main/docs/assets/css/VHM96.png) | 
|:--:| 
| *Figure 4: Gauge 96: Tungnaá River. Measured vs. simulated streamflow for the test period.* |

| ![VHM112.png](https://github.com/hhelgason/CSE599-DL-final-project/blob/main/docs/assets/css/VHM112.png) | 
|:--:| 
| *Figure 5: Gauge 112: Þjórsá River. Measured vs. simulated streamflow for the test period.* |

| ![VHM221.png](https://github.com/hhelgason/CSE599-DL-final-project/blob/main/docs/assets/css/VHM221.png) | 
|:--:| 
| *Figure 6: Gauge 221: Jökulsá River. Measured vs. simulated streamflow for the test period.* |

We see that the performance of these models is rather good, especially for gauge 221 (Jökulsá River).

## LSTM states and activations
For a model run with 20 hidden layers, we take a look at how the actual cell state values evolve with time for a highly glaciated basin.
| ![18.png](https://github.com/hhelgason/CSE599-DL-final-project/blob/main/docs/assets/css/18.png) | 
|:--:| 
| *Figure 7: Cell states for gauge 102: Jökulsá á Fjöllum River. The highlighted cell looks like snow.* |

We see some definite seasonal patterns in many of the cell states. We can identify a cell state whose value looks similar to how snow accumulates in winter and melts in spring. We also see a state that looks like the seasonal variations of temperature (the state with the highest values). We also see some states that peak in late summer, when glacial melt is at its peak. 

We also take a look at timeseries of inputs, cell and hidden states, and gate activations while the LSTM processes a sequence of a full sample (365 days):
| ![run_9.png](https://github.com/hhelgason/CSE599-DL-final-project/blob/main/docs/assets/css/run_9.png) | 
|:--:| 
| *Figure 8: Timeseries of inputs, cell and hidden states, and gate activations while the LSTM processes a sequence of a full sample (365 days). Note that the x axis is days. * |

It would be interesting to analyzing this data in more detail to better understand what the LSTM model is actually learning.

# Further work
For this project, training data was only available for 10 basins. Training on more basins would allow the model to better generalize and yield more accurate results. Although the model was trained on relatively few basins, its accuracy was good. These models can be used in daily inflow forecasts for hydropower operations in Iceland. 

Thanks to the developers of NeuralHydrology. The code for LSTM state and activations was obtained from their tutorial. 

# References
Kratzert, F., Klotz, D., Herrnegger, M., Sampson, A. K., Hochreiter, S., & Nearing, G. (2019a). Toward improved predictions in ungauged basins: Exploiting the power of machine learning. Water Resources Research, 55(12), 11344-11354. https://doi.org/10.1029/2019WR026065


