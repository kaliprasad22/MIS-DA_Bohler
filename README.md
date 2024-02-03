import pandas as pd

# Step 1: Create a new field named 'Smoker?' in the ahasmokers.xlsx file
ahas_mokers = pd.read_excel('ahasmokers.xlsx')
ahas_mokers['Smoker?'] = 1
ahas_mokers.to_excel('ahasmokers_updated.xlsx', index=False)

# Step 2: Create a new field named 'Smoker?' in the ahanonsmokers.xlsx file
aha_nonsmokers = pd.read_excel('ahanonsmokers.xlsx')
aha_nonsmokers['Smoker?'] = 0
aha_nonsmokers.to_excel('ahanonsmokers_updated.xlsx', index=False)

# Step 3: Combine records from the two files into a single file
combined_data = pd.concat([ahas_mokers, aha_nonsmokers], ignore_index=True)
combined_data.to_excel('ahastudy.xlsx', index=False)

# Step 4: Add a new field named 'SevereHyp' to indicate severe hypertension
combined_data['SevereHyp'] = combined_data['SystolicBP'].apply(lambda x: 'Yes' if x > 180 else 'No')
combined_data.to_excel('ahaalert.xlsx', index=False)

# (a) Provide the average risk for stroke for subjects with systolic BP higher than 192
average_risk_high_systolic = combined_data[combined_data['SystolicBP'] > 192]['Risk'].mean()
print("Average risk for stroke for subjects with systolic BP > 192:", round(average_risk_high_systolic, 1))

# (b) Provide the average age for subjects with diastolic BP lower than 64
average_age_low_diastolic = combined_data[combined_data['DiastolicBP'] < 64]['Age'].mean()
print("Average age for subjects with diastolic BP < 64:", round(average_age_low_diastolic, 1))
