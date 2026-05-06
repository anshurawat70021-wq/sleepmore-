import streamlit as st
import pandas as pd
from datetime import datetime

st.set_page_config(page_title="Sleep Tracker 😴", layout="centered")

st.title("😴 Sleep Tracker App")
st.write("Track your sleep schedule and improve your routine!")

# File to store data
DATA_FILE = "sleep_data.csv"

# Load existing data
try:
    df = pd.read_csv(DATA_FILE)
except:
    df = pd.DataFrame(columns=["Date", "Sleep Time", "Wake Time", "Duration (hrs)"])

# Input Section
st.header("📝 Add Sleep Record")

date = st.date_input("Select Date")
sleep_time = st.time_input("Sleep Time")
wake_time = st.time_input("Wake Up Time")

if st.button("Add Record"):
    sleep_dt = datetime.combine(date, sleep_time)
    wake_dt = datetime.combine(date, wake_time)

    # Handle next day wake-up
    if wake_dt <= sleep_dt:
        wake_dt = wake_dt.replace(day=wake_dt.day + 1)

    duration = (wake_dt - sleep_dt).seconds / 3600

    new_data = pd.DataFrame({
        "Date": [date],
        "Sleep Time": [sleep_time],
        "Wake Time": [wake_time],
        "Duration (hrs)": [round(duration, 2)]
    })

    df = pd.concat([df, new_data], ignore_index=True)
    df.to_csv(DATA_FILE, index=False)

    st.success("✅ Sleep record added!")

# Show Data
st.header("📊 Sleep History")

if not df.empty:
    st.dataframe(df)

    avg_sleep = df["Duration (hrs)"].mean()
    st.info(f"💡 Average Sleep: {round(avg_sleep,2)} hours")

    if avg_sleep < 6:
        st.warning("⚠️ You should sleep more!")
    elif avg_sleep > 9:
        st.warning("⚠️ You might be oversleeping!")
    else:
        st.success("👍 Your sleep is healthy!")

else:
    st.write("No data yet. Start adding records!")
