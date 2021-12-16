# CSE 599 DL final project - LSTM modeling of streamflow in snow/glacier melt dominated catchments
Hordur Helgason
12/15/21

## Introduction
Applications of DL methods in streamflow modeling have yielded very good results, with DL models substantially outperforming benchmark physically based models (e.g. Kratzert et al., 2019). DL methods of modeling hydrologic processes in catchments where runoff primarily originates from snow and glacier melt have not yet received much attention from the scientific community. Accurately modeling river discharge in snow/glacier melt-dominated catchments is highly important for water resource management.
In this final project, I trained an LSTM model to predict streamflow in snow/glacier melt-dominated catchments. 

The aim of the project was to develop a model to be used in streamflow forecasts for hydropower operations in Iceland.

## Methods
### Data

Streamflow measurements and weather model output data from a selection of 10 glacial rivers in Iceland were used for model development. The locations of the measurement gauges and corresponding watersheds can be seen in figure 1. 

Weather parameters from an atmospheric reanalysis dataset (REFERENCE) on a 2x2 km grid over Iceland were averaged over each watershed. The specific weather parameters used are listed in table 1. The target variable is a timeseries of area-averaged streamflow values with a daily time resolution. The streamflow measurements are quality coded. The streamflow measurements for the 10 basins are visualized in figure 2.

The dataset was split into train (11 years: 1999-10-01- 2010-09-30), validation (5 years: 2010-10-01 to 2015-09-30) and test (4 years: 2015-10-01 to 2019-08-31) sets.


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
