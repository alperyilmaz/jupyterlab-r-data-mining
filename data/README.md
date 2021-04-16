# Random forest data

QSAR dataset at [Zenodo](https://zenodo.org/record/3738729) were wrong so we downloaded the [original data](https://pubs.acs.org/doi/10.1021/ci4000213) as `ci4000213_si_001.xlsx`. 
Then used R to prepare the train and test datasets

```R
library(tidyverse)

clean <- readxl::read_excel("ci4000213_si_001.xlsx",1) %>% 
  select(-(`CAS-RN`:Smiles)) %>% 
  mutate(Class=ifelse(Class=="RB",1,0)) %>%
  relocate(Class, .after=nX)

clean %>% filter(Status=="Train") %>% select(-Status) %>% write_csv("train_rows_fixed.csv")
clean %>% filter(Status=="Test") %>% select(-c(Status,Class)) %>% write_csv("test_rows_fixed.csv")
clean %>% filter(Status=="Test") %>% select(-Status) %>% write_csv("test_rows_wlabels_fixed.csv")
```
