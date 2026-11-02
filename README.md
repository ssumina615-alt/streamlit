import streamlit as st
import time

# Set up page styling
st.set_page_config(page_title="Water Safety Predictor", page_icon="💧", layout="centered")

st.title("💧 Water Safety Predictor Prototype")
st.write("An interactive prototype for testing water portability metrics.")

st.info("💡 Running in **Prototype Mode** (Using standard water quality baseline guidelines).")

# 1. User Input Form
st.subheader("Enter Water Quality Metrics")

col1, col2 = st.columns(2)

with col1:
    ph = st.number_input("pH Level (0.0 - 14.0)", min_value=0.0, max_value=14.0, value=7.2, step=0.1)
    turbidity = st.number_input("Turbidity (NTU)", min_value=0.0, max_value=20.0, value=2.5, step=0.1)
    dissolved_oxygen = st.number_input("Dissolved Oxygen (mg/L)", min_value=0.0, max_value=20.0, value=7.0, step=0.5)

with col2:
    tds = st.number_input("Total Dissolved Solids (ppm)", min_value=0.0, max_value=3000.0, value=300.0, step=10.0)
    chlorine = st.number_input("Free Chlorine (mg/L)", min_value=0.0, max_value=10.0, value=1.5, step=0.1)
    temperature = st.number_input("Temperature (°C)", min_value=0.0, max_value=50.0, value=22.0, step=0.5)

# 2. Trigger Prediction
if st.button("Predict Safety Status", type="primary"):
    
    # Visual loading status for user experience
    with st.spinner('Analyzing metrics...'):
        time.sleep(0.8) # Simulate AI processing time
    
    st.write("---")
    st.subheader("Prediction Results")
    
    # 3. Baseline Validation Rules (Simulating AI Classification)
    # Safe ranges based on general standard water guidelines:
    # pH: 6.5 - 8.5 | Turbidity: < 5.0 NTU | TDS: < 500 ppm | Chlorine: 0.2 - 4.0 mg/L
    
    is_ph_safe = 6.5 <= ph <= 8.5
    is_turbidity_safe = turbidity <= 5.0
    is_tds_safe = tds <= 500.0
    is_chlorine_safe = 0.2 <= chlorine <= 4.0
    
    # Calculate a mock confidence score based on how many parameters pass
    metrics = [is_ph_safe, is_turbidity_safe, is_tds_safe, is_chlorine_safe]
    passed_metrics = sum(metrics)
    
    # Safe if most vital parameters pass
    if passed_metrics >= 3:
        confidence = 75.0 + (passed_metrics * 5.5)
        if confidence > 100.0: confidence = 99.4
        
        st.success(f"✅ **Water appears SAFE for consumption!**")
        st.metric(label="AI Confidence Score", value=f"{confidence:.1f}%")
        
        # Educational feedback
        st.write("The tested parameters fall within acceptable baseline parameters for safe drinking water.")
    else:
        confidence = 65.0 + ((4 - passed_metrics) * 8.0)
        if confidence > 100.0: confidence = 98.7
        
        st.error(f"❌ **Water is flagged as UNSAFE!**")
        st.metric(label="AI Risk Confidence", value=f"{confidence:.1f}%")
        
        # Dynamic warning details to show the prototype works logically
        st.warning("⚠️ **Flagged Issues:**")
        if not is_ph_safe: st.write("- pH level is outside the optimal 6.5–8.5 range.")
        if not is_turbidity_safe: st.write("- Turbidity is too high (Water is too cloudy).")
        if not is_tds_safe: st.write("- Total Dissolved Solids exceed recommended levels.")
        if not is_chlorine_safe: st.write("- Chlorine content is outside safe disinfection limits.")
