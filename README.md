# 🎥 Predictive Analytics for Movie Box Office Success

Welcome to the **Predictive Analytics for Movie Box Office Success** project! This repository showcases the implementation of advanced data-driven methodologies to analyze and predict movie box office performance using a variety of cutting-edge techniques and visualizations.

---

## 📜 Project Overview
This project aims to provide actionable insights and predictions about box office success by leveraging movie-related data. By incorporating advanced feature engineering and sophisticated visualization tools, this project presents a comprehensive analysis for stakeholders in the film industry.

---

## 🧩 Key Features
- **Feature Engineering**: Inclusion of relevant attributes to boost predictive capabilities.
- **Global Market Analysis**: Insights into how production countries and genres influence popularity.
- **Influencer Impact Assessment**: Role of key actors, directors, and producers in determining success.
- **Sentiment Analysis**: Deep learning-based sentiment analysis of movie reviews.
- **Interactive Dashboard**: (Upcoming) A Streamlit-based dashboard for enhanced user experience.

---

## 📂 Repository Structure
```
project-root
│
├── data
│   ├── feature_engineered_movie_data.csv  # Processed dataset used for analysis
│
├── notebooks
│   ├── feature_engineering.ipynb          # Notebook for feature engineering
│   ├── data_visualization.ipynb           # Notebook for advanced visualizations
│
├── visualizations
│   ├── top_genres_by_country.png          # Chart showing genre popularity by country
│
├── scripts
│   ├── data_preprocessing.py              # Script for cleaning and preprocessing data
│   ├── visualization_script.py            # Script for generating visualizations
│
└── README.md                              # Project documentation (you are here!)
```

---

## 📊 Insights from Visualizations
- **Box Office Revenue vs Budget with Popularity**:
~~~
import plotly.express as px

# Load your dataset (replace with the appropriate file path)
file_path = '/content/feature_engineered_movie_data.csv'
movie_data = pd.read_csv(file_path)

# Select relevant columns
bubble_data = movie_data[['budget', 'revenue', 'popularity', 'genres', 'original_title']]

# Create the bubble chart
fig = px.scatter(
    bubble_data,
    x='budget',                # X-axis: Budget
    y='revenue',               # Y-axis: Box Office Revenue
    size='popularity',         # Bubble Size: Popularity
    color='genres',             # Bubble Color: Genre
    hover_name='original_title', # Tooltip: Movie Name
    title='Box Office Revenue vs Budget with Popularity',
    labels={'budget': 'Budget', 'revenue': 'Revenue'},
    size_max=60                # Max size of bubbles
)

# Customize the chart layout
fig.update_layout(
    title_font=dict(size=20, family='Arial', color='black'),
    xaxis=dict(title='Budget (in $)', tickprefix='$', showgrid=True),
    yaxis=dict(title='Revenue (in $)', tickprefix='$', showgrid=True),
    paper_bgcolor='rgb(240, 242, 245)'  # Background color
)

# Show the chart
fig.show()

~~~
![image](https://github.com/user-attachments/assets/f39127d9-a314-47a2-97e7-75f7784100cc)

- **Actual vs Predicted Comparison**:
~~~
#  Actual vs Predicted Comparison
plt.figure(figsize=(8, 6))
sns.scatterplot(
    data=predictions,
    x='Actual',
    y='Predicted',
    color='#3498DB',
    alpha=0.8,
    s=80
)
plt.plot(
    [predictions['Actual'].min(), predictions['Actual'].max()],
    [predictions['Actual'].min(), predictions['Actual'].max()],
    color='#E74C3C',
    linestyle='--',
    linewidth=2,
    label='Perfect Prediction Line'
)
plt.title('Actual vs Predicted Revenue', fontsize=16, fontweight='bold', color='#2C3E50')
plt.xlabel('Actual Revenue', fontsize=14, color='#34495E')
plt.ylabel('Predicted Revenue', fontsize=14, color='#34495E')
plt.legend(fontsize=12)
plt.grid(visible=True, linestyle='--', alpha=0.6)
plt.show()
~~~
![image](https://github.com/user-attachments/assets/954f9cec-39fc-4546-8fe5-8dc3450e93fd)

- **Correlation Heatmap of Movie Variables**:
~~~
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# Load your dataset (replace with the appropriate file path)
file_path = '/content/feature_engineered_movie_data.csv'
movie_data = pd.read_csv(file_path)

# Select relevant columns for correlation
correlation_data = movie_data[['budget', 'revenue', 'popularity', 'runtime']]

# Calculate the correlation matrix
correlation_matrix = correlation_data.corr()

# Plot the heatmap
plt.figure(figsize=(10, 8))
sns.heatmap(
    correlation_matrix, 
    annot=True, 
    cmap='coolwarm', 
    fmt='.2f', 
    linewidths=0.5,
    annot_kws={"size": 12}
)

plt.title('Correlation Heatmap of Movie Variables', fontsize=16, fontweight='bold')
plt.xticks(fontsize=12)
plt.yticks(fontsize=12, rotation=0)
plt.tight_layout()
plt.show()
~~~
![image](https://github.com/user-attachments/assets/ed6b7fa3-3a6f-4c34-903b-67076d9c0c8c)


- **Distribution of Movies Across ROI Categories**:
~~~
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load the uploaded dataset
file_path = '/content/cleaned_movie_data.csv'
movie_data = pd.read_csv(file_path)

# Inspect the dataset and ensure ROI column is processed correctly
movie_data['ROI'] = pd.to_numeric(movie_data['ROI'], errors='coerce')

# Create ROI categories
bins = [0, 5, 20, 50, 100, 200, movie_data['ROI'].max()]
labels = ['0-5%', '5-20%', '20-50%', '50-100%', '100-200%', '200%+']
movie_data['ROI_Category'] = pd.cut(movie_data['ROI'], bins=bins, labels=labels, include_lowest=True)

# Count the number of movies in each ROI category
roi_distribution = movie_data['ROI_Category'].value_counts().sort_index()

#  Plot the ROI distribution
plt.figure(figsize=(10, 6))
sns.barplot(x=roi_distribution.index, y=roi_distribution.values, palette='viridis')

# Add titles and labels
plt.title('Distribution of Movies Across ROI Categories', fontsize=16, weight='bold')
plt.xlabel('ROI Categories', fontsize=14)
plt.ylabel('Number of Movies', fontsize=14)
plt.xticks(fontsize=12)
plt.yticks(fontsize=12)
plt.grid(axis='y', linestyle='--', alpha=0.7)

# Show the plot
plt.tight_layout()
plt.show()
~~~
![image](https://github.com/user-attachments/assets/374a0148-3ba7-464a-8168-4dba7bc90548)


- **Top 5 Genre Popularity Across Top 10 Production Countries**:
~~~
import pandas as pd
import plotly.express as px

# Load your dataset (replace with the appropriate file path)
file_path = '/content/feature_engineered_movie_data.csv'
movie_data = pd.read_csv(file_path)

# Filter data for top 10 production countries by total popularity
top_countries = (
    movie_data.groupby('production_countries')['popularity']
    .sum()
    .nlargest(10)
    .index
)
filtered_data = movie_data[movie_data['production_countries'].isin(top_countries)]

# Filter top 5 genres by total popularity
top_genres = (
    filtered_data.groupby('genres')['popularity']
    .sum()
    .nlargest(5)
    .index
)
filtered_data = filtered_data[filtered_data['genres'].isin(top_genres)]

# Group data for visualization and sort production countries by total popularity
grouped_data = (
    filtered_data.groupby(['production_countries', 'genres'], as_index=False)['popularity']
    .sum()
)
grouped_data['total_popularity'] = grouped_data.groupby('production_countries')['popularity'].transform('sum')
grouped_data = grouped_data.sort_values(by='total_popularity', ascending=False)

# Create a grouped bar chart with the new pastel color palette
fig = px.bar(
    grouped_data,
    x='production_countries',
    y='popularity',
    color='genres',
    title='Top 5 Genre Popularity Across Top 10 Production Countries',
    labels={'popularity': 'Total Popularity', 'production_countries': 'Production Countries'},
    barmode='group',  # Grouped bars for better comparison
    color_discrete_sequence=px.colors.qualitative.Pastel  # Soft and elegant pastel color palette
)

# Customize chart layout for better visibility and arrangement
fig.update_layout(
    title_font=dict(size=24, family='Arial', color='black'),
    xaxis=dict(
        title='Production Countries',
        tickangle=45,
        showgrid=False,
        categoryorder='array',
        categoryarray=grouped_data['production_countries'].unique()
    ),
    yaxis=dict(title='Total Popularity', showgrid=True),
    legend=dict(title='Genres', orientation='h', yanchor='bottom', y=1.02, xanchor='center', x=0.5),
    margin=dict(l=40, r=40, t=80, b=150),
    paper_bgcolor='white',  # Minimalistic background
    plot_bgcolor='white'    # No extra shading for plot area
)

# Increase visualization size
fig.update_layout(width=1200, height=700)

# Add hover data for more visual information
fig.update_traces(
    hovertemplate="<b>Production Country:</b> %{x}<br>" +
                  "<b>Genre:</b> %{color}<br>" +
                  "<b>Popularity:</b> %{y}<extra></extra>"
)

# Show the chart
fig.show()
~~~
![image](https://github.com/user-attachments/assets/35a0149f-886e-477e-bbfc-1deae9c9f9b7)


- **Average ROI Across Categories**:
~~~
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Reload the dataset to ensure correctness
file_path = '/content/cleaned_movie_data.csv'
movie_data = pd.read_csv(file_path)

# Define the ROI categories more clearly if not already categorized
movie_data['ROI_Category'] = pd.cut(
    movie_data['ROI'],
    bins=[-float('inf'), 0, 50, 100, 200, float('inf')],
    labels=['Negative ROI', 'Low ROI (0-50%)', 'Moderate ROI (50-100%)', 'High ROI (100-200%)', 'Very High ROI (>200%)']
)

# Group data for line visualization
roi_group = movie_data.groupby('ROI_Category')['ROI'].mean()

# Create a line plot for ROI distribution across categories
plt.figure(figsize=(10, 6))
sns.lineplot(data=roi_group, marker='o', linewidth=2, color='#2a9d8f')

# Set the theme for professional aesthetics
sns.set_theme(style="darkgrid", palette="pastel")
plt.title("Average ROI Across Categories", fontsize=16, fontweight='bold')
plt.xlabel("ROI Categories", fontsize=12)
plt.ylabel("Average ROI (%)", fontsize=12)
plt.xticks(rotation=30, fontsize=10)
plt.grid(visible=True, linestyle='--', alpha=0.5)
plt.tight_layout()
plt.show()
~~~
![image](https://github.com/user-attachments/assets/aa940dcc-00e2-4fa9-94c9-80cc96e3a114)


- **Top 10 Revenue Movies**:
~~~
file_path = '/content/feature_engineered_movie_data.csv'
movie_data = pd.read_csv(file_path)

# Selecting columns relevant for top revenue analysis
revenue_data = movie_data[['title', 'revenue', 'genres', 'budget', 'ROI']]

# Sorting data by revenue to get top 10 movies
top_revenue_movies = revenue_data.sort_values(by='revenue', ascending=False).head(10)

# Setting the style for the visualization
sns.set_style("darkgrid")
plt.figure(figsize=(10, 6))

# Horizontal bar chart for top 10 revenue movies
sns.barplot(
    x=top_revenue_movies['revenue'],
    y=top_revenue_movies['title'],
    palette="cividis"
)

# Adding titles and labels
plt.title("Top 10 Revenue Movies", fontsize=16, weight='bold')
plt.xlabel("Revenue (in billions)", fontsize=12)
plt.ylabel("Movie", fontsize=12)

# Displaying the plot
plt.tight_layout()
plt.show()
~~~
![image](https://github.com/user-attachments/assets/39f6d4a4-62fd-41b1-801d-19eb25fc442a)


- **Average Revenue by Release Month**:
~~~
import seaborn as sns
import matplotlib.pyplot as plt
#  Average revenue by release month
# Extracting release month from the dataset
# Grouping data by release month and calculating average revenue
monthly_revenue = movie_data.groupby('release_month')['revenue'].mean().reset_index()

# Mapping numeric months to their names
import calendar
# Ensure release_month is of integer type before applying lambda function
monthly_revenue['release_month'] = monthly_revenue['release_month'].astype(int)
monthly_revenue['Month Name'] = monthly_revenue['release_month'].apply(lambda x: calendar.month_name[x])

# Sorting months in calendar order
monthly_revenue = monthly_revenue.sort_values(by='release_month')

# Plotting the data
plt.figure(figsize=(14, 8))
sns.barplot(
    x='Month Name',
    y='revenue',
    data=monthly_revenue,
    palette='viridis',
    edgecolor='black'
)
plt.title('Average Revenue by Release Month', fontsize=18, fontweight='bold')
plt.xlabel('Month', fontsize=14)
plt.ylabel('Average Revenue (in Billion $)', fontsize=14)
plt.xticks(rotation=45, fontsize=12)
plt.yticks(fontsize=12)
plt.grid(axis='y', linestyle='--', alpha=0.7)
plt.show()
~~~
![image](https://github.com/user-attachments/assets/403f6839-9377-43fc-ad67-0afa01a1dfa1)


- **Top 5 Blockbuster Movies of the Season!**:
~~~
import pandas as pd
import plotly.express as px

# Load your dataset (replace with the appropriate file path)
file_path = '/content/feature_engineered_movie_data.csv'
movie_data = pd.read_csv(file_path)

# Select relevant columns
popularity_data = movie_data[['original_title', 'popularity']]

#  Sort by popularity and select the top 5 movies
top_5_movies = popularity_data.sort_values(by='popularity', ascending=False).head(5)

# Truncate movie names to a maximum length (e.g., 15 characters)
top_5_movies['Truncated Movie'] = top_5_movies['original_title'].apply(lambda x: x[:15] + '...' if len(x) > 15 else x)

# Create a "pull" column to separate the top movie
top_5_movies['Pull'] = [0.2 if i == 0 else 0 for i in range(len(top_5_movies))]  # Top movie is separated

# Create the pie chart
fig = px.pie(
    top_5_movies, 
    values='popularity', 
    names='Truncated Movie', 
    title='<b>Top 5 Blockbuster Movies of the Season!</b>',  # Bold and catchy title
    color_discrete_sequence=px.colors.qualitative.Vivid  # Vibrant color palette
)

# Apply the pull effect and customize label positions
fig.update_traces(
    textinfo='label+percent',  # Show both label and percentage
    textposition='inside',    # Place labels inside the slices
    insidetextorientation='radial',  # Radial orientation for better readability
    pull=top_5_movies['Pull']  # Separate the top movie slice
)

# Adjust layout to reduce background space and increase pie size
fig.update_layout(
    paper_bgcolor='rgb(245, 255, 250)',  # Light green-tinted background
    title_font=dict(size=24, family='Arial, sans-serif', color='darkblue'),
    margin=dict(l=20, r=20, t=50, b=20),  # Tight margins
    height=600,  # Increased chart height
    width=600,   # Increased chart width
    showlegend=False  # Hide the legend since labels are now inside
)

# Show the chart
fig.show()
~~~
![image](https://github.com/user-attachments/assets/54bccae8-2f5e-4844-9694-7d395e790e9f)


---

## 🛠️ Tech Stack
- **Python**: Primary programming language.
- **Libraries**: 
  - `pandas`, `numpy`: For data manipulation.
  - `plotly.express`: For creating professional and interactive visualizations.
  - `sklearn`, `tensorflow`: For predictive modeling and sentiment analysis.
- **Streamlit**: (Upcoming) For dashboard implementation.

---

## 📈 How to Run the Project
1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/movie-box-office-analytics.git
   ```

2. Navigate to the project directory:
   ```bash
   cd movie-box-office-analytics
   ```

3. Install the required packages:
   ```bash
   pip install -r requirements.txt
   ```

4. Run the scripts in `notebooks` or execute visualizations directly from `scripts`.

---



> "Movies are a window into the culture of the world; let data be the key to understanding them!"
