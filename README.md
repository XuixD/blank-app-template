import streamlit as st
import pandas as pd
import requests

st.title("📊 Buscador de Datos Deportivos - El Xui Stats")

league = st.selectbox("Selecciona una liga", [
    "Premier League",
    "La Liga",
    "Serie A",
    "Bundesliga",
    "Ligue 1",
    "MLS"
])

st.write(f"Buscando datos de: **{league}**...")

url = "https://www.scorebat.com/video-api/v3/feed/?token=demo"

try:
    data = requests.get(url).json()
    matches = data.get("response", [])
    
    df = pd.DataFrame([{
        "Partido": m["title"],
        "Competición": m["competition"],
        "Fecha": m["date"]
    } for m in matches if league.lower() in m["competition"].lower()])

    st.dataframe(df)

except Exception as e:
    st.error("Error al obtener datos.")
