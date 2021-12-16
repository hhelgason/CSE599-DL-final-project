# LSTM modeling of streamflow in snow/glacier melt dominated catchments
CSE 599 Deep Learning final project - Hordur Helgason
12/15/21

## Introduction
Applications of DL methods in streamflow modeling have yielded very good results, with DL models substantially outperforming benchmark physically based models (e.g. Kratzert et al., 2019). DL methods of modeling hydrologic processes in catchments where runoff primarily originates from snow and glacier melt have not yet received much attention from the scientific community. Accurately modeling river discharge in snow/glacier melt-dominated catchments is highly important for water resource management.
In this final project, I trained an LSTM model to predict streamflow in snow/glacier melt-dominated catchments. I also analized the states and activations of a trained LSTM, to see if the LSTM internals resemble physically meaningful quantities. 

The aim of the project was to develop a model to be used in streamflow forecasts for hydropower operations in Iceland.

## Methods
### Data

Streamflow measurements and weather model output data from a selection of 10 glacial rivers in Iceland were used for model development. The locations of the measurement gauges and corresponding watersheds can be seen in figure 1. 
<!-- <img width="700" alt="image" src="https://github.com/hhelgason/CSE599-DL-final-project/blob/main/docs/assets/css/layout_1.png"> -->
<!-- *Figure 1: Locations of gauges and corresponding watersheds used in this project* -->

| ![map.jpg](https://github.com/hhelgason/CSE599-DL-final-project/blob/main/docs/assets/css/layout_1.png) | 
|:--:| 
| *Figure 1: Locations of the gauges and corresponding watersheds that are used in this project* |

Weather parameters from an atmospheric reanalysis dataset on a 2x2 km grid over Iceland were averaged over each watershed. The specific weather parameters used are temperature, precipitation, solid ratio of precipitation, wind speed, and downwelling shortwave and longwave radiation. The target variable is a timeseries of area-averaged streamflow values with a daily time resolution. 

The dataset was split into train (11 years: 1999-10-01- 2010-09-30), validation (5 years: 2010-10-01 to 2015-09-30) and test (4 years: 2015-10-01 to 2019-08-31) sets.

### Code
The [NeuralHydrology](https://github.com/neuralhydrology/neuralhydrology) python package was utilized to perform the experiments. The python package was developed by the AI for Earth Science group at Institute for Machine Learning, Johannes Kepler University. The package is built on top of Pytorch.

Model training was performed on Google Colab.

### Model setup
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

The mean validation NSE for these runs is shown in figure 3.

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

In figures 4-6, we plot the simulated vs. measured disharge for each gauge. 

| ![gauge_96.jpg](https://github.com/hhelgason/CSE599-DL-final-project/blob/main/docs/assets/css/VHM96.png) | 
|:--:| 
| *Figure 4: Gauge 96: Tungnaá River. Measured vs. simulated streamflow for the test period.* |

| ![gauge_112.jpg](https://github.com/hhelgason/CSE599-DL-final-project/blob/main/docs/assets/css/VHM112.png) | 
|:--:| 
| *Figure 5: Gauge 112: Þjórsá River. Measured vs. simulated streamflow for the test period.* |

| ![gauge_221.jpg](https://github.com/hhelgason/CSE599-DL-final-project/blob/main/docs/assets/css/VHM221.png) | 
|:--:| 
| *Figure 6: Gauge 221: Jökulsá River. Measured vs. simulated streamflow for the test period.* |

We see that the performance of these models is rather good, especially for gauge 221 (Jökulsá River).

## LSTM states and activations

# Further work
For this project, training data was only available for 10 basins. Training on more basins would allow the model to better generalize and yield more accurate results. Although the model was trained on relatively few basins, its accuracy was good. These models can be used in daily inflow forecasts for hydropower operations in Iceland. 




You can use the [editor on GitHub](https://github.com/hhelgason/CSE599-DL-final-project/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/hhelgason/CSE599-DL-final-project/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
