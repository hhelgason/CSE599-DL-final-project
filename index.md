# LSTM modeling of streamflow in snow/glacier melt dominated catchments
CSE 599 Deep Learning final project - Hordur Helgason
12/15/21

## Introduction
Applications of DL methods in streamflow modeling have yielded very good results, with DL models substantially outperforming benchmark physically based models (e.g. Kratzert et al., 2019). DL methods of modeling hydrologic processes in catchments where runoff primarily originates from snow and glacier melt have not yet received much attention from the scientific community. Accurately modeling river discharge in snow/glacier melt-dominated catchments is highly important for water resource management.
In this final project, I trained an LSTM model to predict streamflow in snow/glacier melt-dominated catchments. 

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





> We propose an embarrassingly simple but very effective scheme for high-quality dense stereo reconstruction: (i) generate an approximate reconstruction with your favourite stereo matcher; (ii) rewarp the input images with that approximate model; (iii) with the initial reconstruction and the warped images as input, train a deep network to enhance the reconstruction by regressing a residual correction; and (iv) if desired, iterate the refinement with the new, improved reconstruction.




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
