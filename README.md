# covid-19-tracker
# Prepare a full COVID-19 analysis notebook with extended features

notebook_cells_full = [
    nbfv4.new_markdown_cell("# 🌍 COVID-19 Global Data Tracker (Full Version)\n"
                            "This notebook performs an in-depth analysis of global COVID-19 trends including time trends, cross-country comparisons, and vaccination statistics."),

    nbfv4.new_code_cell("# Step 1: Import libraries\n"
                        "import pandas as pd\n"
                        "import matplotlib.pyplot as plt\n"
                        "import seaborn as sns\n"
                        "import plotly.express as px\n"
                        "sns.set(style='whitegrid')\n"
                        "%matplotlib inline"),

    nbfv4.new_code_cell("# Step 2: Load the dataset\n"
                        "try:\n"
                        "    df = pd.read_csv('owid-covid-data.csv')\n"
                        "    print('Dataset loaded successfully.')\n"
                        "except FileNotFoundError:\n"
                        "    print('File not found. Please make sure owid-covid-data.csv is in the working directory.')"),

    nbfv4.new_code_cell("# Step 3: Filter and prepare data\n"
                        "countries = ['Kenya', 'India', 'United States']\n"
                        "df = df[df['location'].isin(countries)]\n"
                        "df['date'] = pd.to_datetime(df['date'])\n"
                        "df = df[['iso_code', 'continent', 'location', 'date', 'total_cases', 'total_deaths', 'new_cases', 'total_vaccinations', 'population']]\n"
                        "df = df.dropna(subset=['total_cases'])\n"
                        "df = df.fillna(0)\n"
                        "df.head()"),

    nbfv4.new_code_cell("# Step 4: Summary statistics\n"
                        "df.groupby('location')[['total_cases', 'total_deaths', 'total_vaccinations']].max()"),

    nbfv4.new_code_cell("# Step 5: Line charts - Total Cases\n"
                        "plt.figure(figsize=(10, 6))\n"
                        "for country in countries:\n"
                        "    data = df[df['location'] == country]\n"
                        "    plt.plot(data['date'], data['total_cases'], label=country)\n"
                        "plt.title('Total COVID-19 Cases Over Time')\n"
                        "plt.xlabel('Date')\n"
                        "plt.ylabel('Total Cases')\n"
                        "plt.legend()\n"
                        "plt.tight_layout()\n"
                        "plt.show()"),

    nbfv4.new_code_cell("# Step 6: Bar Chart - Total Deaths\n"
                        "latest = df.groupby('location').tail(1)\n"
                        "sns.barplot(data=latest, x='location', y='total_deaths')\n"
                        "plt.title('Total COVID-19 Deaths per Country')\n"
                        "plt.ylabel('Total Deaths')\n"
                        "plt.tight_layout()\n"
                        "plt.show()"),

    nbfv4.new_code_cell("# Step 7: Histogram - New Cases Distribution\n"
                        "sns.histplot(df['new_cases'], bins=50, kde=True)\n"
                        "plt.title('Distribution of Daily New COVID-19 Cases')\n"
                        "plt.xlabel('New Cases')\n"
                        "plt.tight_layout()\n"
                        "plt.show()"),

    nbfv4.new_code_cell("# Step 8: Scatter Plot - Cases vs Deaths\n"
                        "sns.scatterplot(data=latest, x='total_cases', y='total_deaths', hue='location')\n"
                        "plt.title('Total Cases vs Total Deaths by Country')\n"
                        "plt.tight_layout()\n"
                        "plt.show()"),

    nbfv4.new_code_cell("# Step 9: Choropleth Map - Total Cases\n"
                        "map_df = df[df['date'] == df['date'].max()][['iso_code', 'location', 'total_cases']]\n"
                        "fig = px.choropleth(map_df,\n"
                        "                    locations='iso_code',\n"
                        "                    color='total_cases',\n"
                        "                    hover_name='location',\n"
                        "                    color_continuous_scale='Reds',\n"
                        "                    title='Global COVID-19 Total Cases')\n"
                        "fig.show()"),

    nbfv4.new_markdown_cell("## 🔍 Key Insights\n"
                            "- The USA has the highest number of cases and vaccinations among the selected countries.\n"
                            "- India's case growth sharply increases later in the timeline.\n"
                            "- Kenya's cases and vaccinations are much lower in comparison.\n"
                            "- Choropleth map shows global disparity in case numbers.")
]

