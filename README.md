# AMCAT-EDA

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

data = pd.read_excel('/content/train.xlsx')

data.head()

data.shape

data.info()

data.describe()

for col in data.columns:
    print('--------------------------{}--------------------------'.format(col))
    print(data[col].unique())

data.drop('Unnamed: 0',axis=1,inplace=True)

data

"""From the above steps we can see that:

1. DOL is not in datatime format and it contains a value 'present'.
2. JobCity, Specialization, Board of Examination columns have several redundant values.
3. GraduationYear column contains 0.
4. Jobcity also conatins -1 value.
5. All the score value columns contain -1 value.

## Data Cleaning

### DOJ and DOL columns
"""

data['DOJ']=pd.to_datetime(data['DOJ'])

data['DOL'].replace(to_replace='present',value=pd.Timestamp.now(),inplace=True)
data['DOL']=pd.to_datetime(data['DOL'])

data['Tenure'] = (data['DOL']-data['DOJ'])/np.timedelta64(1, 'm')
data['Tenure'] = data['Tenure'].astype(int)
data['Tenure'].unique()

data[data['Tenure']<0]

"""### DOB Column"""

data['DOB']=pd.to_datetime(data['DOB'])
data['DOB'].dtype

from datetime import datetime, date

current_date = datetime.now()

# Calculate the timedelta representing age
data['Age'] = (current_date - data['DOB'])

# Convert timedelta to total seconds and then to years
data['Age'] = (data['Age'].dt.total_seconds() / (60 * 60 * 24 * 365.25)).astype(int) # Divide by the approximate number of seconds in a year (365.25 to account for leap years)

print(data['Age'])



data['Age'].unique()

"""### 10th Board"""

data['10board'].unique()

data[data['10board']==0]

XBoard={'board ofsecondary education,ap':'STATE', 'cbse':'CBSE', 'state board':'STATE',
       'mp board bhopal':'STATE', 'icse':'ICSE',
       'karnataka secondary school of examination':'STATE', 'up':'STATE',
       'karnataka state education examination board':'STATE', 'ssc':'STATE',
       'kerala state technical education':'STATE', 0:'Other', 'bseb':'STATE',
       'state board of secondary education, andhra pradesh':'STATE',
       'matriculation':'Other', 'gujarat state board':'STATE', 'karnataka state board':'STATE',
       'wbbse':'STATE', 'maharashtra state board':'STATE', 'icse board':'ICSE', 'up board':'STATE',
       'board of secondary education(bse) orissa':'STATE',
       'little jacky matric higher secondary school':'Other',
       'uttar pradesh board':'STATE', 'bsc,orissa':'STATE', 'mp board':'STATE', 'upboard':'STATE',
       'matriculation board':'Other', 'j & k bord':'STATE', 'rbse':'STATE',
       'central board of secondary education':'CBSE', 'pseb':'STATE', 'jkbose':'STATE',
       'haryana board of school education,(hbse)':'STATE', 'metric':'Other', 'ms board':'STATE',
       'kseeb':'STATE', 'stateboard':'STATE', 'maticulation':'Other',
       'karnataka secondory education board':'STATE', 'mumbai board':'STATE', 'sslc':'STATE',
       'kseb':'STATE', 'board secondary  education':'STATE', 'matric board':'Other',
       'board of secondary education':'STATE',
       'west bengal board of secondary education':'STATE',
       'jharkhand secondary examination board,ranchi':'STATE', 'u p board':'STATE',
       'bseb,patna':'STATE', 'hsc':'STATE', 'bse':'STATE', 'sss pune':'STATE',
       'karnataka education board (keeb)':'STATE', 'kerala':'STATE',
       'state board of secondary education( ssc)':'STATE', 'gsheb':'STATE',
       'up(allahabad)':'STATE', 'nagpur':'STATE', 'don bosco maatriculation school':'ICSE',
       'karnataka state secondary education board':'STATE', 'maharashtra':'STATE',
       'karnataka secondary education board':'STATE',
       'himachal pradesh board of school education':'STATE',
       'certificate of middle years program of ib':'Other',
       'karnataka board of secondary education':'STATE',
       'board of secondary education rajasthan':'STATE', 'uttarakhand board':'STATE',
       'ua':'STATE', 'board of secendary education orissa':'STATE',
       'karantaka secondary education and examination borad':'STATE', 'hbsc':'STATE',
       'kseeb(karnataka secondary education examination board)':'STATE',
       'cbse[gulf zone]':'CBSE', 'hbse':'STATE', 'state(karnataka board)':'STATE',
       'jharkhand accademic council':'STATE',
       'jharkhand secondary examination board (ranchi)':'STATE',
       'karnataka secondary education examination board':'STATE', 'delhi board':'STATE',
       'mirza ahmed ali baig':'STATE', 'jseb':'STATE', 'bse, odisha':'STATE', 'bihar board':'STATE',
       'maharashtra state(latur board)':'STATE', 'rajasthan board':'STATE', 'mpboard':'STATE',
       'upbhsie':'STATE', 'secondary board of rajasthan':'STATE',
       'tamilnadu matriculation board':'Other', 'jharkhand secondary board':'STATE',
       'board of secondary education,andhara pradesh':'STATE', 'up baord':'STATE',
       'state':'STATE', 'board of intermediate education':'Other',
       'state board of secondary education,andhra pradesh':'STATE',
       'up board , allahabad':'STATE',
       'stjosephs girls higher sec school,dindigul':'Other', 'maharashtra board':'STATE',
       'education board of kerala':'STATE', 'board of ssc':'STATE',
       'maharashtra state board pune':'STATE',
       'board of school education harayana':'STATE',
       'secondary school cerfificate':'STATE', 'maharashtra sate board':'STATE', 'ksseb':'STATE',
       'bihar examination board, patna':'STATE', 'latur':'STATE',
       'board of secondary education, rajasthan':'STATE', 'state borad hp':'STATE',
       'cluny':'CBSE', 'bsepatna':'STATE', 'up borad':'STATE', 'ssc board of andrapradesh':'STATE',
       'matric':'Other', 'bse,orissa':'STATE', 'ssc-andhra pradesh':'STATE', 'mp':'STATE',
       'karnataka education board':'STATE', 'mhsbse':'STATE',
       'karnataka sslc board bangalore':'STATE', 'karnataka':'STATE', 'u p':'STATE',
       'secondary school of education':'STATE', 'state board of karnataka':'STATE',
       'karnataka secondary board':'STATE', 'andhra pradesh board ssc':'STATE',
       'stjoseph of cluny matrhrsecschool,neyveli,cuddalore district':'CBSE',
       'hse,orissa':'STATE', 'national public school':'ICSE', 'nagpur board':'STATE',
       'jharkhand academic council':'STATE', 'bsemp':'STATE',
       'board of secondary education, andhra pradesh':'STATE',
       'board of secondary education orissa':'STATE',
       'board of secondary education,rajasthan(rbse)':'STATE',
       'board of secondary education,ap':'STATE',
       'board of secondary education,andhra pradesh':'STATE',
       'jawahar navodaya vidyalaya':'CBSE', 'aisse':'CBSE',
       'karnataka board of higher education':'STATE', 'bihar':'STATE',
       'kerala state board':'STATE', 'cicse':'ICSE', 'tn state board':'STATE',
       'kolhapur divisional board, maharashtra':'STATE',
       'bharathi matriculation school':'Other', 'uttaranchal state board':'STATE',
       'wbbsce':'STATE', 'mp state board':'STATE', 'seba(assam)':'STATE', 'anglo indian':'Other', 'gseb':'STATE',
       'uttar pradesh':'STATE', 'ghseb':'STATE', 'board of school education uttarakhand':'STATE',
       'msbshse,pune':'STATE', 'tamilnadu state board':'STATE', 'kerala university':'STATE',
       'uttaranchal shiksha avam pariksha parishad':'STATE',
       'bse(board of secondary education)':'STATE',
       'bright way college, (up board)':'STATE',
       'school secondary education, andhra pradesh':'STATE',
       'secondary state certificate':'STATE',
       'maharashtra state board of secondary and higher secondary education,pune':'STATE',
       'andhra pradesh state board':'STATE', 'stmary higher secondary':'CBSE', 'cgbse':'STATE',
       'secondary school certificate':'STATE', 'rajasthan board ajmer':'STATE', 'mpbse':'STATE',
       'pune board':'STATE', 'cbse ':'CBSE', 'board of secondary education,orissa':'STATE',
       'maharashtra state board,pune':'STATE', 'up bord':'STATE',
       'kiran english medium high school':'Other', 'state board (jac, ranchi)':'STATE',
       'gujarat board':'STATE', 'state board ':'STATE', 'sarada high scchool':'Other',
       'kalaimagal matriculation higher secondary school':'Other',
       'karnataka board':'STATE', 'maharastra board':'STATE', 'sslc board':'STATE',
       'ssc maharashtra board':'STATE', 'tamil nadu state':'STATE', 'uttrakhand board':'STATE',
       'bihar secondary education board,patna':'STATE',
       'haryana board of school education':'STATE',
       'sri kannika parameswari highier secondary school, udumalpet':'STATE',
       'ksseb(karnataka state board)':'STATE', 'nashik board':'STATE',
       'jharkhand secondary education board':'STATE', 'himachal pradesh board':'STATE',
       'maharashtra satate board':'STATE',
       'maharashtra state board mumbai divisional board':'STATE',
       'dav public school,hehal':'CBSE',
       'state board of secondary education, ap':'STATE',
       'rajasthan board of secondary education':'STATE', 'hsce':'STATE',
       'karnataka secondary education':'STATE',
       'board of secondary education,odisha':'STATE', 'maharashtra nasik board':'STATE',
       'west bengal board of secondary examination (wbbse)':'STATE',
       'holy cross matriculation hr sec school':'Other', 'cbsc':'CBSE', 'apssc':'STATE',
       'bseb patna':'STATE', 'kolhapur':'STATE', 'bseb, patna':'STATE', 'up board allahabad':'STATE',
       'biharboard':'STATE', 'nagpur board,nagpur':'STATE', 'pune':'STATE', 'gyan bharati school':'CBSE',
       'rbse,ajmer':'STATE', 'board of secondaray education':'STATE',
       'secondary school education':'STATE', 'state bord':'STATE', 'jbse,jharkhand':'STATE',
       'hse':'STATE', 'madhya pradesh board':'STATE', 'bihar school examination board':'STATE',
       'west bengal board of secondary eucation':'STATE', 'state boardmp board ':'STATE',
       'icse board , new delhi':'ICSE',
       'board of secondary education (bse) orissa':'STATE',
       'maharashtra state board for ssc':'STATE',
       'board of secondary school education':'STATE', 'latur board':'STATE',
       "stmary's convent inter college":'CBSE', 'nagpur divisional board':'STATE',
       'ap state board':'STATE', 'cgbse raipur':'STATE', 'uttranchal board':'STATE', 'ksbe':'STATE',
       'central board of secondary education, new delhi':'CBSE',
       'bihar school examination board patna':'CBSE', 'cbse board':'CBSE',
       'sslc,karnataka':'STATE', 'mp-bse':'STATE', 'up bourd':'STATE', 'dav public school sec 14':'CBSE',
       'board of school education haryana':'STATE',
       'council for indian school certificate examination':'Other',
       'aurangabad board':'STATE', 'j&k state board of school education':'STATE',
       'maharashtra state board of secondary and higher secondary education':'STATE',
       'maharashtra state boar of secondary and higher secondary education':'STATE',
       'ssc regular':'STATE', 'karnataka state examination board':'STATE', 'nasik':'STATE',
       'west bengal  board of secondary education':'STATE', 'up board,allahabad':'STATE',
       'bseb ,patna':'STATE',
       'state board - west bengal board of secondary education : wbbse':'STATE',
       'maharashtra state board of secondary & higher secondary education':'STATE',
       'delhi public school':'CBSE', 'karnataka secondary eduction':'STATE',
       'secondary education board of rajasthan':'STATE',
       'maharashtra board, pune':'STATE', 'rbse (state board)':'STATE', 'apsche':'STATE',
       'board of  secondary education':'STATE',
       'board of high school and intermediate education uttarpradesh':'STATE',
       'kea':'STATE', 'board of secondary education - andhra pradesh':'STATE',
       'ap state board for secondary education':'STATE', 'seba':'STATE',
       'punjab school education board, mohali':'STATE',
       'jharkhand acedemic council':'STATE', 'hse,board':'STATE',
       'board of ssc education andhra pradesh':'STATE', 'up-board':'STATE', 'bse,odisha':'STATE'}

data['XBoard'] = data['10board'].replace(XBoard)

data['XBoard'].unique()

"""### Graduation Year"""

pd.set_option('display.max_columns',None)
data[data['GraduationYear']==0]

data['GraduationYear'].replace(to_replace = 0, value=2014,inplace = True)
data['GraduationYear'].unique()

"""### Specialization"""

Domains={'computer engineering':'CS','electronics and communication engineering':'EE',
 'information technology':'IT','computer science & engineering':'CS',
 'mechanical engineering':'ME','electronics and electrical engineering':'EE',
 'electronics & telecommunications':'EE','instrumentation and control engineering':'EE','computer application':'CS',
 'electronics and computer engineering':'EE','electrical engineering':'EE',
 'applied electronics and instrumentation':'EE','electronics & instrumentation eng':'EE','information science engineering':'IT',
 'civil engineering':'CE','mechanical and automation':'ME','industrial & production engineering':'Other',
 'control and instrumentation engineering':'EE','metallurgical engineering':'Other',
 'electronics and instrumentation engineering':'EE', 'electronics engineering':'EE',
 'ceramic engineering':'Other','chemical engineering': 'Chem', 'aeronautical engineering':'AE',
 'other':'Other','biotechnology':'Other', 'embedded systems technology':'EE',
 'electrical and power engineering':'EE', 'computer science and technology':'CS',
 'mechatronics':'ME','automobile/automotive engineering':'ME', 'polymer technology':'Other',
 'mechanical & production engineering':'ME', 'power systems and automation':'EE',
 'instrumentation engineering':'EE' ,'telecommunication engineering':'IT',
 'industrial & management engineering':'ME', 'industrial engineering':'ME',
 'computer and communication engineering':'CS',
 'information & communication technology':'IT', 'information science':'CS',
 'internal combustion engine':'Other', 'computer networking':'CS',
 'biomedical engineering':'Other', 'electronics':'EE', 'computer science':'CS'}

data['Specialization'].replace(Domains,inplace=True)

data['Specialization'].unique()

data[data['Domain']== -1]

"""### JobCity"""

len(list(data['JobCity'].unique()))

data['JobCity'].replace(-1,'Remote/Others',inplace=True)

choice=['AM','Agra','Ahmedabad','Ahmednagar','Saudi Arabia','Allahabad','Alwar','Ambala','Asansol','Aurangabad','Australia','Angul','Ariyalur',
 'Bhopal','Baddi HP','Bahadurgarh','Bangalore','Bankura','Bareli','Baripada','Baroda','Bathinda','Beawar','Belgium',
 'Bellary','Bhagalpur','Bharuch','Bhilai','Bihar','Bhiwadi','Bhubaneshwar','Bikaner','Bilaspur','Bulandshahar','Bundi','Burdwan',
 'CHEYYAR','Calicut','Chandigarh','Chandrapur','Chennai','Chennai & Mumbai','Chennai, Bangalore','Coimbatore',
 'Daman and Diu','Dammam','Dausa','Dehradun','Delhi','Dhanbad','Dharamshala','Dharmapuri','Dharuhera','Dubai','Durgapur',
'Ernakulam','Faridabad','Gagret','Gandhi Nagar','Ganjam','Ghaziabad','Gonda','Gorakhpur','Greater Noida','Gulbarga',
 'Gurga', 'Gurgaon','Guwahati','Gwalior','Haldia','Haridwar','Hissar','Hospete','Howrah','Hubli','Hyderabad','Haryana',
 'Indirapuram, Ghaziabad','Indore','Jabalpur','Jagdalpur','Jaipur','Jalandhar','Jammu','Jamnagar','Jamshedpur','Jaspur','Jeddah Saudi Arabia',
 'Jhajjar','Jhansi','Jodhpur','Johannesburg','Joshimath','Jowai','Kakinada','Kalmar, Sweden','Kalamb','Kanpur','Karad','Karnal','Khopoli','kharagpur',
 'Kolhapur','kudankulam ,tarapur','Latur (Maharashtra )','Kochi','Kochi/Cochin, Chennai and Coimbatore','Kolkata','Kota','Kurnool','Kerala','London','Lucknow','Ludhiana','Madurai','Maharajganj','Mainpuri','Manesar',
 'Mangalore','Meerut','Mettur, Tamil Nadu ','Miryalaguda','Mohali','Mumbai','Muvattupuzha','Muzaffarnagar','Muzaffarpur',
 'Mysore','Nagari','Nagpur','Nalagarh','Nanded','Nashik','Navi Mumbai , Hyderabad','Neemrana','NCR','Nellore','Noida','Ongole','PATNA',
 'Panchkula','Pantnagar','Patiala','Patna','Phagwara','Pilani','Pondicherry','Pune','RAE BARELI','RAS AL KHAIMAH',
 'Raigarh','Raipur','Rajasthan','Rajkot','Rajpura','Ranchi','Ratnagiri','Rayagada, Odisha','Rewari','Rohtak','Roorkee',
 'Rourkela','Rudrapur','SHAHDOL','Sahibabad','Salem','Sambalpur','Secunderabad','Shahdol','Shimla','Siliguri','Sonipat',
 'Surat','Trivandrum','Thane','Thiruvananthapuram','Tirunelvelli','Tirupati','Tornagallu','Trichur','Trichy','Trivandrum',
 'Udaipur','Una','Unnao','Vadodara','Vandavasi','Varanasi','Vellore','Vijayawada','Visakhapatnam','Vizag','Vapi','Yamuna Nagar']

!pip install fuzzywuzzy

!pip install fuzzywuzzy[speedup]

from fuzzywuzzy import process

def correct_spelling_errors(target_word, choices, threshold=80):
    match, score = process.extractOne(target_word, choices)
    if score >= threshold:
        return match
    else:
        return target_word

data['JobCities'] = data['JobCity'].apply(lambda x : correct_spelling_errors(str(x),choice))

len(list(data['JobCities'].unique()))

data['JobCities'].unique()

"""### Univariate Analysis

##### Personality test scores
"""

fig, axes = plt.subplots(2, 3, figsize=(20, 14))

fig.suptitle('Personality Test Scores Distribution')

sns.distplot(ax=axes[0, 0],a=data['conscientiousness'])
sns.distplot(ax=axes[0, 1],a=data['agreeableness'])
sns.distplot(ax=axes[0, 2],a=data['extraversion'])
sns.distplot(ax=axes[1, 0],a=data['nueroticism'])
sns.distplot(ax=axes[1, 1],a=data['openess_to_experience'])

"""Here we can see that the personality test scores are mostly left skewed which means most of the aspirants got high personality test scores.

We can see that Nueroticism scores follow normal distribution which implies that almost half of the aspirants struggle with emotional stability.

##### Specialization
"""

sns.countplot(data['Specialization'])

Domain_replace = {'Chem':'Other','AE':'Other'}
data['Specialization'].replace(Domain_replace,inplace=True)
sns.countplot(data['Specialization'])

"""From this plot we can say that most of the aspirants are from CS and EE specializations."""

sns.countplot(data['Gender'])

tenth = data['10percentage']
twelve = data['12percentage']
clg_gpa = data['collegeGPA']

fig, axes = plt.subplots(1, 3, figsize=(20, 8))

fig.suptitle('Percentages')

sns.distplot(ax=axes[0],a=tenth)
sns.distplot(ax=axes[1],a=twelve)
sns.distplot(ax=axes[2],a=clg_gpa)

"""Most of the aspirants got 50 to 100 percentages in 10th, 12th and College but there are some students who failed in college(i.e.below 20%) ."""

df = pd.DataFrame(data['XBoard'].value_counts())
# The column representing the counts is named 'XBoard' by default.
# Rename 'XBoard' to 'count' for better clarity
df = df.rename(columns={'XBoard': 'count'})

# Use the 'count' column for the pie chart values.
# Also update the labels to use the index
plt.pie(df['count'], labels=df.index)
plt.show()

sns.countplot(x='CollegeTier', data=data, hue='Specialization')

"""Most of the students are from State and CBSE board in 10th class."""

popular_role = list(pd.DataFrame(data['Designation'].value_counts()).head(15).index)

popular_role

temp = data[data['Designation'].isin(popular_role)]
temp

plt.figure(figsize=(14,10))
sns.countplot(x=data.loc[data['Designation'].isin(popular_role),'Designation'],hue=data['Gender'])
plt.xticks(rotation=90)
plt.show()

plt.figure(figsize=(14,10))
sns.countplot(x=data.loc[data['Designation'].isin(popular_role),'Designation'])
plt.xticks(rotation=90)
plt.show()

plt.figure(figsize=(14,10))
sns.countplot(x=data.loc[data['Designation'].isin(popular_role),'Designation'],hue=data['Specialization'])
plt.xticks(rotation=90)
plt.show()

popular_city = list(pd.DataFrame(data['JobCities'].value_counts()).head(15).index)

popular_city

temp_1 = data[data['JobCities'].isin(popular_city)]
temp_1

plt.figure(figsize=(14,10))
sns.countplot(x=data.loc[data['JobCities'].isin(popular_city),'JobCities'])
plt.xticks(rotation=90)
plt.show()

plt.figure(figsize=(14,10))
sns.countplot(x=data.loc[data['JobCities'].isin(popular_city),'JobCities'],hue=data['Gender'])
plt.xticks(rotation=90)
plt.show()

"""### Bivariate analysis"""

plt.figure(figsize=(16,8))
sns.boxplot(x=data['Salary']/100000,y=data['Gender'])

"""The median salary of both Males and Females is almost similar.

The maximum salary of the males category exceeds the maximum salary of female category.

The Highest salary is nearly 40LPA.
"""

temp_data = pd.DataFrame(data[data['Tenure']>=0])
temp_data.shape

temp_data.head()

plt.figure(figsize=(16,10))
sns.scatterplot(x=temp_data['Tenure']/12,y=temp_data['Salary']/100000,hue=temp_data['Specialization'])

"""We can see that the period of tenure doesn't impact the Salary.

People with higher salaries have experience less that 5 years.

The CS Specialization has the highest salary packages.
"""

plt.figure(figsize=(14,10))
sns.barplot(x=data.loc[data['Designation'].isin(popular_role),'Designation'],y=data['Salary'])
plt.xticks(rotation=90)
plt.show()

plt.figure(figsize=(14,10))
sns.boxplot(x=data['Specialization'],y=data['Salary'])

"""The median salary of CS, EE, IT, ME, and other categories are almost similar.

The salary packages of CE specialization are low compared to CS, EE, IT and ME categories.

Salaries of CS and IT are almost same.
"""

sns.countplot(x=data['Specialization'],hue=data['Gender'])

"""The female aspirants are less than half of male aspirants in each specialization.

ME and CE branches has the least number of female aspirants
"""

plt.figure(figsize=(18,10))
sns.barplot(x=data.loc[data['JobCities'].isin(popular_city),'JobCities'],y=data['Salary'],hue=data['Specialization'])
plt.xticks(rotation=90)
plt.show()

plt.figure(figsize=(18,10))
sns.barplot(x=data.loc[data['JobCities'].isin(popular_city),'JobCities'],y=data['Salary'],hue=data['CollegeTier'])
plt.xticks(rotation=90)
plt.show()

sns.barplot(x=data['Gender'],y=data['Salary'],hue=data['CollegeTier'])

plt.figure(figsize=(18,10))
sns.barplot(x=data.loc[data['JobCities'].isin(popular_city),'JobCities'],y=data['Salary'],hue=data['Gender'])
plt.xticks(rotation=90)
plt.show()

"""### Research Questions

##### Claim-1 : Times of India article dated Jan 18, 2019 states that “After doing your Computer Science Engineering if you take up jobs as a Programming Analyst, Software Engineer, Hardware Engineer and Associate Engineer you can earn up to 2.5-3 lakhs as a fresh graduate.”
"""

claim_1_data = data[(data['Designation'].isin(["programmer analyst","software engineer","hardware engineer","associate engineer"])) & (data['Tenure']//12==0)]

claim_1_data

plt.figure(figsize=(14,8))
sns.barplot(x=claim_1_data['Designation'],y=claim_1_data['Salary']/100000,hue=claim_1_data['Specialization'])

"""The above made claim is true irrespective of the specialization.

##### Claim-2:Is there a relationship between gender and specialization? (i.e. Does the preference of Specialisation depend on the Gender?)
"""

sns.countplot(x=data['Specialization'],hue=data['Gender'])

"""There is a slight relationship between gender and specialization as we can see females mostly prefer CS, EE and IT specializations.

##### Claim-3 : According to U.S Bureau of Labor Statistics "Learn more, earn more: Education leads to higher wages, lower unemployment"

Read more at:
https://www.bls.gov/careeroutlook/2020/data-on-display/education-pays.htm
"""

data['Grad_level'] = ['UnderGrad' if x == 'B.Tech/B.E.' else 'PostGrad' for x in data['Degree']]

data['Grad_level'].unique()

#plt.figure(figsize=(10,8))
sns.barplot(x=data['Grad_level'],y=data['Salary'])

from scipy import stats

undergrad_sal = data.loc[data['Grad_level']=='UnderGrad','Salary']
postgrad_sal = data.loc[data['Grad_level']=='PostGrad','Salary']


t_statistic, p = stats.ttest_ind(undergrad_sal, postgrad_sal)
print("p-value:", p)

if p < 0.05:
    print("Reject the null hypothesis. There is a statistically significant difference in the average salary between undergraduates and postgraduates.")
else:
    print("Fail to reject the null hypothesis. There is no statistically significant difference in the average salary between undergraduates and postgraduates.")

print(data['Salary'].groupby(data['Grad_level']).mean())

"""Tha claim that post graduates earn more than under graduates is not true.

It depends on several conditions like market demand, field of study, previous experience etc.,

##### Claim-4: Tier-1 college students tend to get higher salary packages compared to Tier-2 College students.
"""

plt.figure(figsize=(12,8))
sns.barplot(x=data['CollegeTier'],y=data['Salary'],hue=data['Specialization'])

"""## Conclusion

1. Most of the aspirants are male from CS and EE department from Tier-1 Colleges and they work in IT domain,mostly in Mumbai and Bangalore cities with an average salary of 3LPA and maximum salary of 40LPA

2. Most of the female aspirants prefer CS, EE, IT departments rather than mechanical,civil and they also prefer to work in IT domain mostly in Mumbai, Pune cities with an average salary of 2.9LPA and maximum salary around 36-37 LPA.

3. Bangalore is most sought out place for working but most of the students from Tier-1 college work in Greater Noida.

4. Software Engineer and Software developer are the most popular roles among the aspirants whereas senior software engineer and assisatant manager roles have the highest salary package.
"""

