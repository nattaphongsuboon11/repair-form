import streamlit as st
import pandas as pd
from datetime import datetime

st.set_page_config(page_title="ฟอร์มแจ้งซ่อมเครื่องจักร", layout="centered")
st.title("📋 ฟอร์มแจ้งซ่อมเครื่องจักร")

# ฟอร์มกรอกข้อมูล
with st.form("repair_form"):
    machine = st.text_input("1. ชื่อเครื่องจักร")
    number = st.text_input("2. เบอร์")
    house = st.text_input("3. บ้าน")
    symptom = st.text_input("4. อาการ")
    phenomenon = st.text_input("5. ปรากฏการณ์")
    part = st.text_input("6. ชิ้นส่วน")
    fix = st.text_input("7. การแก้ไข")
    technician = st.text_input("8. ช่างผู้ดำเนินการ")
    time_start = st.time_input("9. เวลาเริ่ม")
    time_end = st.time_input("10. เวลาเสร็จ")

    submitted = st.form_submit_button("✅ บันทึกข้อมูล")

if submitted:
    df = pd.DataFrame([{
        "ชื่อเครื่องจักร": machine,
        "เบอร์": number,
        "บ้าน": house,
        "อาการ": symptom,
        "ปรากฏการณ์": phenomenon,
        "ชิ้นส่วน": part,
        "การแก้ไข": fix,
        "ช่างผู้ดำเนินการ": technician,
        "เวลาเริ่ม": time_start.strftime("%H:%M"),
        "เวลาเสร็จ": time_end.strftime("%H:%M"),
        "เวลาบันทึก": datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    }])

    # บันทึกเป็น Excel
    excel_file = "แจ้งซ่อมเครื่องจักร.xlsx"
    try:
        old = pd.read_excel(excel_file)
        df = pd.concat([old, df], ignore_index=True)
    except FileNotFoundError:
        pass
    df.to_excel(excel_file, index=False)
    st.success("✅ บันทึกข้อมูลเรียบร้อยแล้ว!")
