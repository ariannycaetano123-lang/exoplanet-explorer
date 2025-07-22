# exoplanet-explorer
# src/data_preprocessing.py

import pandas as pd

def load_and_clean_data(path):
    """
    Carrega e limpa dados de exoplanetas.
    Remove linhas com valores ausentes em colunas importantes.
    """
    df = pd.read_csv(path)
    df = df.dropna(subset=['pl_rade', 'pl_bmasse', 'pl_eqt', 'st_teff', 'koi_pdisposition'])
    # Transformar status de candidato/confirmado para coluna planet_status
    df['planet_status'] = df['koi_pdisposition'].map({
        'CONFIRMED': 'Confirmed',
        'CANDIDATE': 'Candidate',
        'FALSE POSITIVE': 'False Positive'
    })
    # Mantemos só Confirmed e Candidate para ML
    df = df[df['planet_status'].isin(['Confirmed', 'Candidate'])]
    return df
