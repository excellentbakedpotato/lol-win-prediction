---
layout: default
title: Home
---

## Introduction
League of Legends (LoL) is a multiplayer online battle arena video game developed and published by Riot Games. Players pilot “Champions” battle in teams of 5 each, in a player-vs-player format. League of Legends is the world's largest esport, with several large international tournaments. We are working with a dataset of professional LoL matches developed by Oracle’s Elixir. In this project, we only used data for matches that took place in 2025. Each row represents a single match played by a summoner, including key in-game metrics such as gold earned, kills, assists, and objective control at various points in the game.
The focus of this project is on the following question:
Can we predict whether a team will win a match using only statistics available at the 15-minute mark?

This question is important because early-game performance often drives match outcomes, and identifying predictive factors early can help teams make strategic decisions during live play. For fans and analysts, understanding early indicators of victory makes the game more engaging and provides insight into the mechanics of competitive play.

The dataset had 120636 rows and 165 columns, of which, we used: 
| Column               | Description                                                                                              |
| -------------------- | -------------------------------------------------------------------------------------------------------- |
| `league`             | Name of the professional league in which the match took place.                                           |
| `patch`              | Game version or patch number at the time of the match.                                                   |
| `side`               | The team’s side (e.g., blue or red).                                                                     |
| `firstPick`          | Indicates whether the team had the first pick during champion selection.                                 |
| `result`             | Match outcome: 1 if the team won, 0 if the team lost.                                                    |
| `firstblood`         | 1 if the team secured first blood, 0 otherwise.                                                          |
| `firstdragon`        | 1 if the team secured the first dragon, 0 otherwise.                                                     |
| `firstherald`        | 1 if the team secured the first Rift Herald, 0 otherwise.                                                |
| `firsttower`         | 1 if the team destroyed the first tower, 0 otherwise.                                                    |
| `firstmidtower`      | 1 if the team destroyed the first mid-lane tower, 0 otherwise.                                           |
| `firsttothreetowers` | 1 if the team destroyed three towers before the opponent, 0 otherwise.                                   |
| `golddiffat15`       | Gold difference between the team and opponent at 15 minutes. Positive values indicate the team is ahead. |
| `xpdiffat15`         | Experience difference at 15 minutes. Positive values indicate the team is ahead.                         |
| `csdiffat15`         | Creep score (minion kills) difference at 15 minutes.                                                     |
| `killsat15`          | Number of kills the team had at 15 minutes.                                                              |
| `assistsat15`        | Number of assists the team had at 15 minutes.                                                            |
| `deathsat15`         | Number of deaths the team had at 15 minutes.                                                             |

Each 'gameid' corresponds to up to 12 rows – one for each of the 5 players on both teams and 2 containing summary data for the two teams.
These columns capture early-game performance and objective control, which are crucial for predicting match outcomes. By focusing on the first 15 minutes of the game, we aim to identify which metrics most strongly indicate victory, providing insights for players, coaches, analysts, and fans alike.

## Data Cleaning

## Missingness Analysis

## Model

## Results

## Conclusion
