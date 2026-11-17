# Computational fact checking

My Computer Science thesis, written with a classmate in 2018. It judges a statement such as "Barack Obama was married to Michelle Obama" by the chain of Wikipedia links that joins the two articles. A chain through narrow articles, which few pages link to, supports the statement; a chain through broad ones, such as "United States", supports it little.

This repository is a fork of [jibalio/computational-fact-checking](https://github.com/jibalio/computational-fact-checking), where we did the work together. That one holds the finished version, with the final data and the paper. This fork stops on 5 October 2018 and adds the Windows mock-up in `Fact Checker/`.

## How it works

1. `PathFinder.py` finds the shortest chain of links between two articles, through the Degrees of Wikipedia site.
2. `Truth.py` scores each chain. It starts from the measure in Ciampaglia et al., "Computational Fact Checking from Knowledge Networks" (PLOS ONE, 2015): each article on the chain lowers the score by the log of how many pages link to it. We added how alike the first and last articles' texts are, by TF-IDF and cosine similarity, in two forms: metric closure counts every article on the chain, ultrametric closure only the broadest.
3. Every article it reads goes into a SQLite file, so later runs ask Wikipedia less.
4. `Computational Fact Checking.ipynb` runs the tests below.

## What we measured

- Members of the US Congress against 20 ideologies, from anarchism to white supremacy: a support vector machine told Democrats from Republicans by their scores. Under cross-validation it was right 69% of the time with metric closure, the same as the 2015 measure, and 50% with ultrametric closure.
- Statements whose answers we knew: US presidents and their spouses, world capitals, US state capitals, and film directors, each with an ROC curve.
- Birthplaces from Google's Relation Extraction Corpus: our scores agreed with its human ratings, with a Spearman correlation of 0.40.

## The window

`Fact Checker/` is a Windows Forms mock-up of how someone might type a statement and get an answer. It never calls the checker: it waits two seconds and always says True.

## Running it

The code dates from 2018 and runs on Python 3. It calls two outside services, Degrees of Wikipedia for the chains and a backlink counter on dispenser.info.tm, which may no longer answer. `requirements.txt` is empty; the code uses wikipedia, scikit-learn, pandas, NumPy, SciPy, NLTK, matplotlib and colorama.
