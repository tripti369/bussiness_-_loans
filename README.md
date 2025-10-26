# bussiness_-_loans
# Step 1: Install dependencies
!pip install streamlit pandas faker
!npm install -g localtunnel

# Step 2: Create synthetic datasets
import pandas as pd
from faker import Faker
import random

fake = Faker('en_IN')

# ---------- Dataset 1: Businesses ----------
businesses = []
for _ in range(2000):  # keep smaller for Colab demo
    area = fake.city()
    business = random.choice([
        "Cafe", "Grocery Store", "Mobile Repair", "Salon", "Pharmacy",
        "Online Clothing Store", "Tuition Center", "Food Truck", "Printing Shop",
        "Bakery", "Eco Store", "Book Shop", "Gym", "Pet Care", "Organic Farm"
    ])
    money_needed = random.randint(50000, 500000)
    probability_growth = round(random.uniform(0.5, 0.95), 2)
    flaws = random.choice([
        "High competition", "Seasonal demand", "Low footfall area",
        "High setup cost", "Supply chain issues", "Local license required"
    ])
    businesses.append([area, business, money_needed, probability_growth, flaws])

business_df = pd.DataFrame(businesses, columns=[
    "area", "business_suitable", "money_needed", "probability_growth", "major_flaws"
])
business_df.to_csv("businesses_dataset.csv", index=False)

# ---------- Dataset 2: Loans ----------
loans = []
for _ in range(500):
    loan_type = random.choice([
        "PMEGP", "Mudra Loan", "Startup India", "SIDBI", "SBI Business Loan",
        "HDFC Small Business Loan", "Axis Bank SME Loan", "Private Micro Finance"
    ])
    min_amt = random.randint(50000, 100000)
    max_amt = min_amt + random.randint(500000, 1000000)
    interest = round(random.uniform(6.5, 12.5), 2)
    eligibility = random.choice([
        "18+ years, Indian citizen, business plan required",
        "Existing business min. 6 months old",
        "New entrepreneur with valid Aadhaar and PAN",
        "Good credit score (700+)"
    ])
    link = random.choice([
        "https://www.mudra.org.in/",
        "https://pmegp.gov.in/",
        "https://www.startupindia.gov.in/",
        "https://www.sidbi.in/en",
        "https://sbi.co.in/",
        "https://www.hdfcbank.com/",
        "https://www.axisbank.com/"
    ])
    loans.append([loan_type, min_amt, max_amt, interest, eligibility, link])

loans_df = pd.DataFrame(loans, columns=[
    "loan_name", "min_amount", "max_amount", "interest_rate", "eligibility", "apply_link"
])
loans_df.to_csv("loans_dataset.csv", index=False)

# Step 3: Create Streamlit App
%%writefile app.py
import streamlit as st
import pandas as pd

st.set_page_config(page_title="Business & Loan Suggestion App", page_icon="💰")
st.title("💼 AI Business & Loan Suggestion App for India")

# Load data
business_df = pd.read_csv("businesses_dataset.csv")
loans_df = pd.read_csv("loans_dataset.csv")

# Inputs
name = st.text_input("👤 Enter your name")
budget = st.number_input("💰 Enter your available money (in ₹)", min_value=1000)
location = st.text_input("📍 Enter your area or city")

if st.button("🔍 Get Suggestions"):
    if name and location:
        st.success(f"Hi {name}, based on your ₹{budget} budget in {location}:")

        # Business suggestions
        filtered = business_df[
            (business_df["area"].str.contains(location, case=False, na=False)) &
            (business_df["money_needed"] <= budget)
        ]

        if not len(filtered):
            filtered = business_df[business_df["money_needed"] <= budget].sample(5)

        st.subheader("🏪 Best Business Options for You")
        st.dataframe(filtered)

        # Loan suggestions
        eligible_loans = loans_df[(loans_df["min_amount"] <= budget)]
        st.subheader("🏦 Possible Loans You Can Apply For")
        st.dataframe(eligible_loans)

        st.info("Click on 'apply_link' in the table to visit the loan portal.")
    else:
        st.warning("Please fill all fields before proceeding!")

# Step 4: Run Streamlit and expose via localtunnel
!nohup streamlit run app.py --server.port 8501 --server.address 0.0.0.0 &
!npx localtunnel --port 8501
# ============================================================
# 🔥 Tripti's "Business & Loan Suggestion App" - Full Colab Code
# ============================================================

# Step 1: Install dependencies
!pip install streamlit pandas faker
!npm install -g localtunnel

# Step 2: Create synthetic datasets
import pandas as pd
from faker import Faker
import random

fake = Faker('en_IN')

# ---------- Dataset 1: Businesses ----------
businesses = []
for _ in range(2000):  # keep smaller for Colab demo
    area = fake.city()
    business = random.choice([
        "Cafe", "Grocery Store", "Mobile Repair", "Salon", "Pharmacy",
        "Online Clothing Store", "Tuition Center", "Food Truck", "Printing Shop",
        "Bakery", "Eco Store", "Book Shop", "Gym", "Pet Care", "Organic Farm"
    ])
    money_needed = random.randint(50000, 500000)
    probability_growth = round(random.uniform(0.5, 0.95), 2)
    flaws = random.choice([
        "High competition", "Seasonal demand", "Low footfall area",
        "High setup cost", "Supply chain issues", "Local license required"
    ])
    businesses.append([area, business, money_needed, probability_growth, flaws])

business_df = pd.DataFrame(businesses, columns=[
    "area", "business_suitable", "money_needed", "probability_growth", "major_flaws"
])
business_df.to_csv("businesses_dataset.csv", index=False)

# ---------- Dataset 2: Loans ----------
loans = []
for _ in range(500):
    loan_type = random.choice([
        "PMEGP", "Mudra Loan", "Startup India", "SIDBI", "SBI Business Loan",
        "HDFC Small Business Loan", "Axis Bank SME Loan", "Private Micro Finance"
    ])
    min_amt = random.randint(50000, 100000)
    max_amt = min_amt + random.randint(500000, 1000000)
    interest = round(random.uniform(6.5, 12.5), 2)
    eligibility = random.choice([
        "18+ years, Indian citizen, business plan required",
        "Existing business min. 6 months old",
        "New entrepreneur with valid Aadhaar and PAN",
        "Good credit score (700+)"
    ])
    link = random.choice([
        "https://www.mudra.org.in/",
        "https://pmegp.gov.in/",
        "https://www.startupindia.gov.in/",
        "https://www.sidbi.in/en",
        "https://sbi.co.in/",
        "https://www.hdfcbank.com/",
        "https://www.axisbank.com/"
    ])
    loans.append([loan_type, min_amt, max_amt, interest, eligibility, link])

loans_df = pd.DataFrame(loans, columns=[
    "loan_name", "min_amount", "max_amount", "interest_rate", "eligibility", "apply_link"
])
loans_df.to_csv("loans_dataset.csv", index=False)

# Step 3: Create Streamlit App
%%writefile app.py
import streamlit as st
import pandas as pd

st.set_page_config(page_title="Business & Loan Suggestion App", page_icon="💰")
st.title("💼 AI Business & Loan Suggestion App for India")

# Load data
business_df = pd.read_csv("businesses_dataset.csv")
loans_df = pd.read_csv("loans_dataset.csv")

# Inputs
name = st.text_input("👤 Enter your name")
budget = st.number_input("💰 Enter your available money (in ₹)", min_value=1000)
location = st.text_input("📍 Enter your area or city")

if st.button("🔍 Get Suggestions"):
    if name and location:
        st.success(f"Hi {name}, based on your ₹{budget} budget in {location}:")

        # Business suggestions
        filtered = business_df[
            (business_df["area"].str.contains(location, case=False, na=False)) &
            (business_df["money_needed"] <= budget)
        ]

        if not len(filtered):
            filtered = business_df[business_df["money_needed"] <= budget].sample(5)

        st.subheader("🏪 Best Business Options for You")
        st.dataframe(filtered)

        # Loan suggestions
        eligible_loans = loans_df[(loans_df["min_amount"] <= budget)]
        st.subheader("🏦 Possible Loans You Can Apply For")
        st.dataframe(eligible_loans)

        st.info("Click on 'apply_link' in the table to visit the loan portal.")
    else:
        st.warning("Please fill all fields before proceeding!")

# Step 4: Run Streamlit and expose via localtunnel
!nohup streamlit run app.py --server.port 8501 --server.address 0.0.0.0 &
!npx localtunnel --port 8501
# ============================================================
# 🔥 Tripti's "Business & Loan Suggestion App" - Full Colab Code
# ============================================================

# Step 1: Install dependencies
!pip install streamlit pandas faker
!npm install -g localtunnel

# Step 2: Create synthetic datasets
import pandas as pd
from faker import Faker
import random

fake = Faker('en_IN')

# ---------- Dataset 1: Businesses ----------
businesses = []
for _ in range(2000):  # keep smaller for Colab demo
    area = fake.city()
    business = random.choice([
        "Cafe", "Grocery Store", "Mobile Repair", "Salon", "Pharmacy",
        "Online Clothing Store", "Tuition Center", "Food Truck", "Printing Shop",
        "Bakery", "Eco Store", "Book Shop", "Gym", "Pet Care", "Organic Farm"
    ])
    money_needed = random.randint(50000, 500000)
    probability_growth = round(random.uniform(0.5, 0.95), 2)
    flaws = random.choice([
        "High competition", "Seasonal demand", "Low footfall area",
        "High setup cost", "Supply chain issues", "Local license required"
    ])
    businesses.append([area, business, money_needed, probability_growth, flaws])

business_df = pd.DataFrame(businesses, columns=[
    "area", "business_suitable", "money_needed", "probability_growth", "major_flaws"
])
business_df.to_csv("businesses_dataset.csv", index=False)

# ---------- Dataset 2: Loans ----------
loans = []
for _ in range(500):
    loan_type = random.choice([
        "PMEGP", "Mudra Loan", "Startup India", "SIDBI", "SBI Business Loan",
        "HDFC Small Business Loan", "Axis Bank SME Loan", "Private Micro Finance"
    ])
    min_amt = random.randint(50000, 100000)
    max_amt = min_amt + random.randint(500000, 1000000)
    interest = round(random.uniform(6.5, 12.5), 2)
    eligibility = random.choice([
        "18+ years, Indian citizen, business plan required",
        "Existing business min. 6 months old",
        "New entrepreneur with valid Aadhaar and PAN",
        "Good credit score (700+)"
    ])
    link = random.choice([
        "https://www.mudra.org.in/",
        "https://pmegp.gov.in/",
        "https://www.startupindia.gov.in/",
        "https://www.sidbi.in/en",
        "https://sbi.co.in/",
        "https://www.hdfcbank.com/",
        "https://www.axisbank.com/"
    ])
    loans.append([loan_type, min_amt, max_amt, interest, eligibility, link])

loans_df = pd.DataFrame(loans, columns=[
    "loan_name", "min_amount", "max_amount", "interest_rate", "eligibility", "apply_link"
])
loans_df.to_csv("loans_dataset.csv", index=False)

# Step 3: Create Streamlit App
%%writefile app.py
import streamlit as st
import pandas as pd

st.set_page_config(page_title="Business & Loan Suggestion App", page_icon="💰")
st.title("💼 AI Business & Loan Suggestion App for India")

# Load data
business_df = pd.read_csv("businesses_dataset.csv")
loans_df = pd.read_csv("loans_dataset.csv")

# Inputs
name = st.text_input("👤 Enter your name")
budget = st.number_input("💰 Enter your available money (in ₹)", min_value=1000)
location = st.text_input("📍 Enter your area or city")

if st.button("🔍 Get Suggestions"):
    if name and location:
        st.success(f"Hi {name}, based on your ₹{budget} budget in {location}:")

        # Business suggestions
        filtered = business_df[
            (business_df["area"].str.contains(location, case=False, na=False)) &
            (business_df["money_needed"] <= budget)
        ]

        if not len(filtered):
            filtered = business_df[business_df["money_needed"] <= budget].sample(5)

        st.subheader("🏪 Best Business Options for You")
        st.dataframe(filtered)

        # Loan suggestions
        eligible_loans = loans_df[(loans_df["min_amount"] <= budget)]
        st.subheader("🏦 Possible Loans You Can Apply For")
        st.dataframe(eligible_loans)

        st.info("Click on 'apply_link' in the table to visit the loan portal.")
    else:
        st.warning("Please fill all fields before proceeding!")

# Step 4: Run Streamlit and expose via localtunnel
!nohup streamlit run app.py --server.port 8501 --server.address 0.0.0.0 &
!npx localtunnel --port 8501
# ============================================================
# 🔥 Tripti's "Business & Loan Suggestion App" - Full Colab Code
# ============================================================

# Step 1: Install dependencies
!pip install streamlit pandas faker
!npm install -g localtunnel

# Step 2: Create synthetic datasets
import pandas as pd
from faker import Faker
import random

fake = Faker('en_IN')

# ---------- Dataset 1: Businesses ----------
businesses = []
for _ in range(2000):  # keep smaller for Colab demo
    area = fake.city()
    business = random.choice([
        "Cafe", "Grocery Store", "Mobile Repair", "Salon", "Pharmacy",
        "Online Clothing Store", "Tuition Center", "Food Truck", "Printing Shop",
        "Bakery", "Eco Store", "Book Shop", "Gym", "Pet Care", "Organic Farm"
    ])
    money_needed = random.randint(50000, 500000)
    probability_growth = round(random.uniform(0.5, 0.95), 2)
    flaws = random.choice([
        "High competition", "Seasonal demand", "Low footfall area",
        "High setup cost", "Supply chain issues", "Local license required"
    ])
    businesses.append([area, business, money_needed, probability_growth, flaws])

business_df = pd.DataFrame(businesses, columns=[
    "area", "business_suitable", "money_needed", "probability_growth", "major_flaws"
])
business_df.to_csv("businesses_dataset.csv", index=False)

# ---------- Dataset 2: Loans ----------
loans = []
for _ in range(500):
    loan_type = random.choice([
        "PMEGP", "Mudra Loan", "Startup India", "SIDBI", "SBI Business Loan",
        "HDFC Small Business Loan", "Axis Bank SME Loan", "Private Micro Finance"
    ])
    min_amt = random.randint(50000, 100000)
    max_amt = min_amt + random.randint(500000, 1000000)
    interest = round(random.uniform(6.5, 12.5), 2)
    eligibility = random.choice([
        "18+ years, Indian citizen, business plan required",
        "Existing business min. 6 months old",
        "New entrepreneur with valid Aadhaar and PAN",
        "Good credit score (700+)"
    ])
    link = random.choice([
        "https://www.mudra.org.in/",
        "https://pmegp.gov.in/",
        "https://www.startupindia.gov.in/",
        "https://www.sidbi.in/en",
        "https://sbi.co.in/",
        "https://www.hdfcbank.com/",
        "https://www.axisbank.com/"
    ])
    loans.append([loan_type, min_amt, max_amt, interest, eligibility, link])

loans_df = pd.DataFrame(loans, columns=[
    "loan_name", "min_amount", "max_amount", "interest_rate", "eligibility", "apply_link"
])
loans_df.to_csv("loans_dataset.csv", index=False)

# Step 3: Create Streamlit App
%%writefile app.py
import streamlit as st
import pandas as pd

st.set_page_config(page_title="Business & Loan Suggestion App", page_icon="💰")
st.title("💼 AI Business & Loan Suggestion App for India")

# Load data
business_df = pd.read_csv("businesses_dataset.csv")
loans_df = pd.read_csv("loans_dataset.csv")

# Inputs
name = st.text_input("👤 Enter your name")
budget = st.number_input("💰 Enter your available money (in ₹)", min_value=1000)
location = st.text_input("📍 Enter your area or city")

if st.button("🔍 Get Suggestions"):
    if name and location:
        st.success(f"Hi {name}, based on your ₹{budget} budget in {location}:")

        # Business suggestions
        filtered = business_df[
            (business_df["area"].str.contains(location, case=False, na=False)) &
            (business_df["money_needed"] <= budget)
        ]

        if not len(filtered):
            filtered = business_df[business_df["money_needed"] <= budget].sample(5)

        st.subheader("🏪 Best Business Options for You")
        st.dataframe(filtered)

        # Loan suggestions
        eligible_loans = loans_df[(loans_df["min_amount"] <= budget)]
        st.subheader("🏦 Possible Loans You Can Apply For")
        st.dataframe(eligible_loans)

        st.info("Click on 'apply_link' in the table to visit the loan portal.")
    else:
        st.warning("Please fill all fields before proceeding!")

# Step 4: Run Streamlit and expose via localtunnel
!nohup streamlit run app.py --server.port 8501 --server.address 0.0.0.0 &
!npx localtunnel --port 8501
# ============================================================
# 🔥 Tripti's "Business & Loan Suggestion App" - Full Colab Code
# ============================================================

# Step 1: Install dependencies
!pip install streamlit pandas faker
!npm install -g localtunnel

# Step 2: Create synthetic datasets
import pandas as pd
from faker import Faker
import random

fake = Faker('en_IN')

# ---------- Dataset 1: Businesses ----------
businesses = []
for _ in range(2000):  # keep smaller for Colab demo
    area = fake.city()
    business = random.choice([
        "Cafe", "Grocery Store", "Mobile Repair", "Salon", "Pharmacy",
        "Online Clothing Store", "Tuition Center", "Food Truck", "Printing Shop",
        "Bakery", "Eco Store", "Book Shop", "Gym", "Pet Care", "Organic Farm"
    ])
    money_needed = random.randint(50000, 500000)
    probability_growth = round(random.uniform(0.5, 0.95), 2)
    flaws = random.choice([
        "High competition", "Seasonal demand", "Low footfall area",
        "High setup cost", "Supply chain issues", "Local license required"
    ])
    businesses.append([area, business, money_needed, probability_growth, flaws])

business_df = pd.DataFrame(businesses, columns=[
    "area", "business_suitable", "money_needed", "probability_growth", "major_flaws"
])
business_df.to_csv("businesses_dataset.csv", index=False)

# ---------- Dataset 2: Loans ----------
loans = []
for _ in range(500):
    loan_type = random.choice([
        "PMEGP", "Mudra Loan", "Startup India", "SIDBI", "SBI Business Loan",
        "HDFC Small Business Loan", "Axis Bank SME Loan", "Private Micro Finance"
    ])
    min_amt = random.randint(50000, 100000)
    max_amt = min_amt + random.randint(500000, 1000000)
    interest = round(random.uniform(6.5, 12.5), 2)
    eligibility = random.choice([
        "18+ years, Indian citizen, business plan required",
        "Existing business min. 6 months old",
        "New entrepreneur with valid Aadhaar and PAN",
        "Good credit score (700+)"
    ])
    link = random.choice([
        "https://www.mudra.org.in/",
        "https://pmegp.gov.in/",
        "https://www.startupindia.gov.in/",
        "https://www.sidbi.in/en",
        "https://sbi.co.in/",
        "https://www.hdfcbank.com/",
        "https://www.axisbank.com/"
    ])
    loans.append([loan_type, min_amt, max_amt, interest, eligibility, link])

loans_df = pd.DataFrame(loans, columns=[
    "loan_name", "min_amount", "max_amount", "interest_rate", "eligibility", "apply_link"
])
loans_df.to_csv("loans_dataset.csv", index=False)

# Step 3: Create Streamlit App
%%writefile app.py
import streamlit as st
import pandas as pd

st.set_page_config(page_title="Business & Loan Suggestion App", page_icon="💰")
st.title("💼 AI Business & Loan Suggestion App for India")

# Load data
business_df = pd.read_csv("businesses_dataset.csv")
loans_df = pd.read_csv("loans_dataset.csv")

# Inputs
name = st.text_input("👤 Enter your name")
budget = st.number_input("💰 Enter your available money (in ₹)", min_value=1000)
location = st.text_input("📍 Enter your area or city")

if st.button("🔍 Get Suggestions"):
    if name and location:
        st.success(f"Hi {name}, based on your ₹{budget} budget in {location}:")

        # Business suggestions
        filtered = business_df[
            (business_df["area"].str.contains(location, case=False, na=False)) &
            (business_df["money_needed"] <= budget)
        ]

        if not len(filtered):
            filtered = business_df[business_df["money_needed"] <= budget].sample(5)

        st.subheader("🏪 Best Business Options for You")
        st.dataframe(filtered)

        # Loan suggestions
        eligible_loans = loans_df[(loans_df["min_amount"] <= budget)]
        st.subheader("🏦 Possible Loans You Can Apply For")
        st.dataframe(eligible_loans)

        st.info("Click on 'apply_link' in the table to visit the loan portal.")
    else:
        st.warning("Please fill all fields before proceeding!")

# Step 4: Run Streamlit and expose via localtunnel
!nohup streamlit run app.py --server.port 8501 --server.address 0.0.0.0 &
!npx localtunnel --port 8501
vv
