# ML-LANUK-Luftqualität

PM10-Vorhersage mit Machine Learning und systematischer Feature-Ablation-Analyse.

## Projektübersicht

Dieses Projekt untersucht, wie stark ML-Modelle bei der Luftqualitätsvorhersage von Autokorrelation abhängen - und wie man das systematisch prüft.

**Kernerkenntnis:** Ein einziges Feature (`PM10_rolling_24h`) erklärt 85% der Modellperformance. Ohne Lag-Features sinkt R² von 0.872 auf 0.088.

## Datenbasis

- **Quelle:** LANUV NRW (Landesamt für Natur, Umwelt und Verbraucherschutz)
- **Stationen:** Köln, Duisburg
- **Zeitraum:** 2015-2025
- **Umfang:** 87.000 stündliche Messungen
- **Features:** PM10, NO2, Temperatur, Windgeschwindigkeit, Luftfeuchtigkeit + Lag-Features

## Methodik: Feature-Ablation

Statt nur Feature Importance zu betrachten, wurden gezielt Features entfernt:

| Test | Konfiguration | R² Validation |
|------|---------------|---------------|
| 0 | Baseline (alle Features) | 0.872 |
| 1 | Ohne PM10_lag_24h | 0.783 |
| 2 | Ohne PM10_rolling_24h | **0.132** |
| 3 | Ohne PM10_lag_168h | 0.870 |
| 4 | Ohne ALLE Lag-Features | 0.130 |
| 5 | Nur Meteorologie | 0.088 |

## Modelle

- XGBoost
- Random Forest
- Gradient Boosting
- Ensemble (Voting)

## Ergebnisse

Die Modelle lernen effektiv:

```
Y(t+1) ≈ mean(Y[t-24:t]) + ε
```

Das ist physikalisch korrekt (PM10 persistiert 5-8 Tage), aber keine echte Vorhersage - nur Persistenz.

## Repository-Struktur

```
├── ML-LANUK-Projekt-Ehmer_AKI.ipynb    # Haupt-Notebook
├── README.md
└── data/                                # Daten (nicht enthalten, siehe unten)
```

## Datenquellen

Die Rohdaten stammen von LANUV NRW Open Data:
- [LANUV Luftqualitätsdaten](https://www.lanuv.nrw.de/umwelt/luft/immissionen/messorte-und-werte)

## Verwendung

```bash
git clone https://github.com/Simeon1970-prog/ML-LANUK-Luftqualitaet.git
cd ML-LANUK-Luftqualitaet
jupyter notebook ML-LANUK-Projekt-Ehmer_AKI.ipynb
```

**Abhängigkeiten:**
- Python 3.8+
- pandas, numpy, scikit-learn, xgboost
- matplotlib, seaborn

## Kontext

Dieses Projekt entstand im Rahmen meines M.Sc. Applied AI Studiums. Es dokumentiert meinen Lernprozess - inklusive der Erkenntnis, dass R² = 0.87 kein Erfolg war, sondern ein Warnsignal.

## Lizenz

MIT License

## Kontakt

Simeon Ehmer  
[LinkedIn](https://www.linkedin.com/in/simeon-ehmer-70186b38)
