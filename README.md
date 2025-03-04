---
title: "Descriptive Statistik"
format: html
echo: false
editor: visual
---

# Erste Analysen

Es werden mit dem Paket `librarian` und der Funktion `shelf` zwei Pakete (bei Bedarf installiert und) geladen: `tidyverse` und `tidycomm` aus dem Repository von fretwurst. R macht das still, aslo ohne Output zu generieren.

```{r}
librarian::shelf(
  tidyverse, 
  fretwurst/tidycomm,
  quiet = TRUE)
```


Die Daten werden von der Seite unserer Begleiter:in geladen. Dann werden sie ...

```{r}

DATEN <- readRDS(url("https://ikmz-statistik.pages.uzh.ch/begleiterin/data/Stat_Einfuehrung_Befragung.RDS"))

DATEN <- readRDS("data/Stat-Einfuehrung-Befragung.RDS")
DATEN |> saveRDS("data/Stat-Einfuehrung-Befragung.RDS")

```

Hier wird mit dem Paket `sjmisc` eine Häufigkeitstabelle (`frq()`) ausgegeben. 

```{r}
DATEN |> 
  sjmisc::frq(E201_02)
```

Hier wird vor der Häufigkeitstabelle ...

```{r}

DATEN |> 
  filter(E201_02 > 0) |> 
  sjmisc::frq(E201_02)

```

Und hier wird vor der Häufigkeitstabelle noch ...

```{r}
DATEN |> 
  filter(E201_02 > 0) |>
  filter(!is.na(E201_02)) |> 
  sjmisc::frq(E201_02)
```

Und hier wird ...

```{r}
#| label: tbl-Statistikfreude
#| tbl-cap: Freude an Statistik

DATEN |> 
  filter(E201_02 > 0) |> 
  tidycomm::knit_frequencies(E201_02)
```

Hier wird der Datensatz DATEN verändert mit `mutate` und zwar indem ... (google: r if_else tidyverse) und schau das https://dplyr.tidyverse.org/reference/if_else.html an.

```{r}
DATEN <- DATEN |> 
  mutate(Statistik_Freude_Dummy = if_else(E201_02 > 3, 1, 0))

DATEN |> 
  filter(!is.na(E201_02)) |> 
    sjmisc::frq(Statistik_Freude_Dummy)
```
Jetzt wird ein ... 

google: r na.rm = TRUE

```{r}
DATEN |> 
  summarise(Durchschnitt = mean(Statistik_Freude_Dummy, na.rm = TRUE)) |> 
  round(2) |> 
  gt::gt()
```
