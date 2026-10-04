# Bahrain Grand Prix in Malaysia 2026 winner likelihood

This is a hobby project. I am a Ferrari and Formula 1 fan, and I built it for fun after watching Mariana Antaya's race predictions.

The model is inspired by those predictions and by her 2026 scripts:

https://github.com/mar-antaya/2026_f1_predictions

This version uses the same method as the Baku project. It trains an XGBoost regressor on the 2026 season only, using FastF1, then runs 25,000 simulated races. The race being predicted is round 16, the Gulf Air Bahrain Grand Prix at Sepang. Qualifying is already complete. The race result is not used.

It is a personal experiment, not a betting tool and not an official prediction.

## What it uses

- 2026 rounds 1–16 only. 2024 and 2025 are not used.
- Qualifying position and the gap to pole
- Practice pace, recent driver and team form, pit stops, and how hard it is to pass at each circuit
- Qualifying weather: air temperature and wet or dry
- An XGBoost regressor that predicts a finishing position

Rounds 1–11 train a test model. Rounds 12–15 check it. The final model trains on rounds 1–15 and predicts round 16.

## Requirements

- Python 3.11 or newer
- The packages in `requirements.txt`:

```
fastf1>=3.4,<4
xgboost>=2.1,<4
scikit-learn>=1.4,<2
pandas>=2.2
numpy>=1.26
requests>=2.31
```

## How to run

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python predict_bahrain_malaysia.py
```

On Windows, activate the environment with `.venv\Scripts\activate`.

The script needs a network connection the first time it asks FastF1 for a round that is not already in `cache/weekends_2026.json`.

## Result

Qualifying weather at Sepang: 32.8°C, dry. 25,000 simulations.

The test on the four races before this one picked 3 of 4 winners. It missed Italy and got the Netherlands, Spain, and Azerbaijan right.

| Place | Driver | Team | Qualifying | Simulations won | Likelihood |
| --- | --- | --- | --- | --- | --- |
| 1 | Max Verstappen | Red Bull Racing | P1 | 11,903 | 47.6% |
| 2 | Kimi Antonelli | Mercedes | P4 | 3,752 | 15.0% |
| 3 | Lewis Hamilton | Ferrari | P2 | 2,921 | 11.7% |
| 4 | Isack Hadjar | Red Bull Racing | P3 | 1,505 | 6.0% |
| 5 | Lando Norris | McLaren | P6 | 1,468 | 5.9% |
| 6 | Charles Leclerc | Ferrari | P5 | 1,334 | 5.3% |
| 7 | George Russell | Mercedes | P8 | 761 | 3.0% |
| 8 | Oscar Piastri | McLaren | P7 | 256 | 1.0% |
| 9 | Pierre Gasly | Alpine | P9 | 237 | 0.9% |
| 10 | Arvid Lindblad | Racing Bulls | P16 | 172 | 0.7% |
| 11 | Franco Colapinto | Alpine | P15 | 146 | 0.6% |
| 12 | Liam Lawson | Racing Bulls | P11 | 137 | 0.5% |
| 13 | Lance Stroll | Aston Martin | P14 | 113 | 0.5% |
| 14 | Gabriel Bortoleto | Audi | P10 | 111 | 0.4% |
| 15 | Carlos Sainz | Williams | P13 | 80 | 0.3% |
| 16 | Fernando Alonso | Aston Martin | P12 | 65 | 0.3% |
| 17 | Nico Hulkenberg | Audi | P17 | 17 | 0.1% |
| 18 | Esteban Ocon | Haas F1 Team | P19 | 10 | 0.0% |
| 19 | Oliver Bearman | Haas F1 Team | P18 | 6 | 0.0% |
| 20 | Sergio Perez | Cadillac | P22 | 3 | 0.0% |
| 21 | Valtteri Bottas | Cadillac | P21 | 2 | 0.0% |
| 22 | Alexander Albon | Williams | P20 | 1 | 0.0% |

Highest likelihood: Max Verstappen (Red Bull Racing), 11,903 of 25,000 simulations (47.6%).

The same table is saved to `bahrain_malaysia_likelihoods.csv`. Training rows go to `training_2026.csv`. The fitted model is saved as `bahrain_malaysia_2026_regressor.json`.

These percentages are the model's count of simulated wins. They are not a promise of the race result.
