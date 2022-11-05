# PyBer_Analysis
{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# PyBer Challenge\n",
    "## Tasks\n",
    "1. Create a PyBer Summary DataFrame for each city type:\n",
    "    * Total Rides\n",
    "    * Total Drivers\n",
    "    * Total Fares\n",
    "    * Average Fare per Ride\n",
    "    * Average Fare per Driver\n",
    "2. Create a Multiple-Line Plot for the sum of the fares for each city type."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Notebook Setup"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Add Matplotlib inline magic command\n",
    "%matplotlib inline\n",
    "\n",
    "# Setup dependencies\n",
    "import matplotlib.pyplot as plt\n",
    "import pandas as pd\n",
    "import numpy as np\n",
    "import os"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Data files to load\n",
    "city_data_to_load = os.path.join(\"Resources\", \"city_data.csv\")\n",
    "ride_data_to_load = os.path.join(\"Resources\", \"ride_data.csv\")\n",
    "\n",
    "# Read data files and store them in a Pandas DataFrame\n",
    "city_data_df = pd.read_csv(city_data_to_load)\n",
    "ride_data_df = pd.read_csv(ride_data_to_load)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Data Cleaning"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "city            120\n",
       "driver_count    120\n",
       "type            120\n",
       "dtype: int64"
      ]
     },
     "execution_count": 3,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "city_data_df.count()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "city            0\n",
       "driver_count    0\n",
       "type            0\n",
       "dtype: int64"
      ]
     },
     "execution_count": 4,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "city_data_df.isnull().sum()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "city            object\n",
       "driver_count     int64\n",
       "type            object\n",
       "dtype: object"
      ]
     },
     "execution_count": 5,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "city_data_df.dtypes"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array(['Urban', 'Suburban', 'Rural'], dtype=object)"
      ]
     },
     "execution_count": 6,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "city_data_df[\"type\"].unique()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "city       2375\n",
       "date       2375\n",
       "fare       2375\n",
       "ride_id    2375\n",
       "dtype: int64"
      ]
     },
     "execution_count": 7,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "ride_data_df.count()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "city       0\n",
       "date       0\n",
       "fare       0\n",
       "ride_id    0\n",
       "dtype: int64"
      ]
     },
     "execution_count": 8,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "ride_data_df.isnull().sum()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "city        object\n",
       "date        object\n",
       "fare       float64\n",
       "ride_id      int64\n",
       "dtype: object"
      ]
     },
     "execution_count": 9,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "ride_data_df.dtypes"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Merge DataFrames"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "city             object\n",
       "date             object\n",
       "fare            float64\n",
       "ride_id           int64\n",
       "driver_count      int64\n",
       "type             object\n",
       "dtype: object"
      ]
     },
     "execution_count": 10,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Combine data into a single data\n",
    "pyber_data_df = pd.merge(ride_data_df, city_data_df, how=\"left\", on=[\"city\", \"city\"])\n",
    "pyber_data_df.dtypes"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Task 1: Create PyBer Summary DataFrame"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Calculate the total number of rides in each City Type"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "type\n",
       "Rural        125\n",
       "Suburban     625\n",
       "Urban       1625\n",
       "Name: ride_id, dtype: int64"
      ]
     },
     "execution_count": 11,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "total_rides_by_city_type = pyber_data_df.groupby([\"type\"]).count()[\"ride_id\"]\n",
    "total_rides_by_city_type"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Calculate the total number of drivers in each City Type"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "type\n",
       "Rural         78\n",
       "Suburban     490\n",
       "Urban       2405\n",
       "Name: driver_count, dtype: int64"
      ]
     },
     "execution_count": 12,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "total_drivers_by_city_type = city_data_df.groupby([\"type\"]).sum()[\"driver_count\"]\n",
    "total_drivers_by_city_type"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Calculate the total fares in each City Type"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "type\n",
       "Rural        4327.93\n",
       "Suburban    19356.33\n",
       "Urban       39854.38\n",
       "Name: fare, dtype: float64"
      ]
     },
     "execution_count": 13,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "total_fares_by_city_type = pyber_data_df.groupby([\"type\"]).sum()[\"fare\"]\n",
    "total_fares_by_city_type"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Calculate the average fare per ride in each City Type"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "type\n",
       "Rural       34.623440\n",
       "Suburban    30.970128\n",
       "Urban       24.525772\n",
       "dtype: float64"
      ]
     },
     "execution_count": 14,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "avgfares_perride_by_city_type = total_fares_by_city_type / total_rides_by_city_type\n",
    "avgfares_perride_by_city_type"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Calculate the average fare per driver in each City Type"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "type\n",
       "Rural       55.486282\n",
       "Suburban    39.502714\n",
       "Urban       16.571468\n",
       "dtype: float64"
      ]
     },
     "execution_count": 15,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "avgfare_perdriver_by_city_type = total_fares_by_city_type / total_drivers_by_city_type\n",
    "avgfare_perdriver_by_city_type"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Remove the index name "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Rural        125\n",
       "Suburban     625\n",
       "Urban       1625\n",
       "Name: ride_id, dtype: int64"
      ]
     },
     "execution_count": 16,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "total_rides_by_city_type.index.name = None\n",
    "total_rides_by_city_type"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Rural         78\n",
       "Suburban     490\n",
       "Urban       2405\n",
       "Name: driver_count, dtype: int64"
      ]
     },
     "execution_count": 17,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "total_drivers_by_city_type.index.name = None\n",
    "total_drivers_by_city_type"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Rural        4327.93\n",
       "Suburban    19356.33\n",
       "Urban       39854.38\n",
       "Name: fare, dtype: float64"
      ]
     },
     "execution_count": 18,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "total_fares_by_city_type.index.name = None\n",
    "total_fares_by_city_type"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Build the PyBer Summary DataFrame"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Total Rides</th>\n",
       "      <th>Total Drivers</th>\n",
       "      <th>Total Fares</th>\n",
       "      <th>Average Fare per Ride</th>\n",
       "      <th>Average Fare per Driver</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <td>Rural</td>\n",
       "      <td>125</td>\n",
       "      <td>78</td>\n",
       "      <td>4327.93</td>\n",
       "      <td>34.623440</td>\n",
       "      <td>55.486282</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>Suburban</td>\n",
       "      <td>625</td>\n",
       "      <td>490</td>\n",
       "      <td>19356.33</td>\n",
       "      <td>30.970128</td>\n",
       "      <td>39.502714</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>Urban</td>\n",
       "      <td>1625</td>\n",
       "      <td>2405</td>\n",
       "      <td>39854.38</td>\n",
       "      <td>24.525772</td>\n",
       "      <td>16.571468</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "          Total Rides  Total Drivers  Total Fares  Average Fare per Ride  \\\n",
       "Rural             125             78      4327.93              34.623440   \n",
       "Suburban          625            490     19356.33              30.970128   \n",
       "Urban            1625           2405     39854.38              24.525772   \n",
       "\n",
       "          Average Fare per Driver  \n",
       "Rural                   55.486282  \n",
       "Suburban                39.502714  \n",
       "Urban                   16.571468  "
      ]
     },
     "execution_count": 19,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "summary_df = pd.DataFrame()\n",
    "\n",
    "summary_df[\"Total Rides\"] = total_rides_by_city_type\n",
    "summary_df[\"Total Drivers\"] = total_drivers_by_city_type\n",
    "summary_df[\"Total Fares\"] = total_fares_by_city_type\n",
    "summary_df[\"Average Fare per Ride\"] = avgfares_perride_by_city_type\n",
    "summary_df[\"Average Fare per Driver\"] = avgfare_perdriver_by_city_type\n",
    "\n",
    "summary_df"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Total Rides                  int64\n",
       "Total Drivers                int64\n",
       "Total Fares                float64\n",
       "Average Fare per Ride      float64\n",
       "Average Fare per Driver    float64\n",
       "dtype: object"
      ]
     },
     "execution_count": 20,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "summary_df.dtypes"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Format the PyBer Summary DataFrame"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Total Rides</th>\n",
       "      <th>Total Drivers</th>\n",
       "      <th>Total Fares</th>\n",
       "      <th>Average Fare per Ride</th>\n",
       "      <th>Average Fare per Driver</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <td>Rural</td>\n",
       "      <td>125</td>\n",
       "      <td>78</td>\n",
       "      <td>$4,327.93</td>\n",
       "      <td>$34.62</td>\n",
       "      <td>$55.49</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>Suburban</td>\n",
       "      <td>625</td>\n",
       "      <td>490</td>\n",
       "      <td>$19,356.33</td>\n",
       "      <td>$30.97</td>\n",
       "      <td>$39.50</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>Urban</td>\n",
       "      <td>1,625</td>\n",
       "      <td>2,405</td>\n",
       "      <td>$39,854.38</td>\n",
       "      <td>$24.53</td>\n",
       "      <td>$16.57</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "         Total Rides Total Drivers Total Fares Average Fare per Ride  \\\n",
       "Rural            125            78   $4,327.93                $34.62   \n",
       "Suburban         625           490  $19,356.33                $30.97   \n",
       "Urban          1,625         2,405  $39,854.38                $24.53   \n",
       "\n",
       "         Average Fare per Driver  \n",
       "Rural                     $55.49  \n",
       "Suburban                  $39.50  \n",
       "Urban                     $16.57  "
      ]
     },
     "execution_count": 21,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "summary_df[\"Total Rides\"] = summary_df[\"Total Rides\"].map(\"{:,}\".format)\n",
    "summary_df[\"Total Drivers\"] = summary_df[\"Total Drivers\"].map(\"{:,}\".format)\n",
    "summary_df[\"Total Fares\"] = summary_df[\"Total Fares\"].map(\"${:,.2f}\".format)\n",
    "summary_df[\"Average Fare per Ride\"] = summary_df[\"Average Fare per Ride\"].map(\"${:.2f}\".format)\n",
    "summary_df[\"Average Fare per Driver\"] = summary_df[\"Average Fare per Driver\"].map(\"${:.2f}\".format)\n",
    "\n",
    "summary_df"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Task 2: Create a Multiple-Line Plot for the Sum of the Fares for Each City Type"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 1. Rename the columns in the PyBer Data DataFrame."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>City</th>\n",
       "      <th>Date</th>\n",
       "      <th>Fare</th>\n",
       "      <th>Ride Id</th>\n",
       "      <th>No. Drivers</th>\n",
       "      <th>City Type</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <td>0</td>\n",
       "      <td>Lake Jonathanshire</td>\n",
       "      <td>2019-01-14 10:14:22</td>\n",
       "      <td>13.83</td>\n",
       "      <td>5739410935873</td>\n",
       "      <td>5</td>\n",
       "      <td>Urban</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>1</td>\n",
       "      <td>South Michelleport</td>\n",
       "      <td>2019-03-04 18:24:09</td>\n",
       "      <td>30.24</td>\n",
       "      <td>2343912425577</td>\n",
       "      <td>72</td>\n",
       "      <td>Urban</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2</td>\n",
       "      <td>Port Samanthamouth</td>\n",
       "      <td>2019-02-24 04:29:00</td>\n",
       "      <td>33.44</td>\n",
       "      <td>2005065760003</td>\n",
       "      <td>57</td>\n",
       "      <td>Urban</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>3</td>\n",
       "      <td>Rodneyfort</td>\n",
       "      <td>2019-02-10 23:22:03</td>\n",
       "      <td>23.44</td>\n",
       "      <td>5149245426178</td>\n",
       "      <td>34</td>\n",
       "      <td>Urban</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>4</td>\n",
       "      <td>South Jack</td>\n",
       "      <td>2019-03-06 04:28:35</td>\n",
       "      <td>34.58</td>\n",
       "      <td>3908451377344</td>\n",
       "      <td>46</td>\n",
       "      <td>Urban</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>5</td>\n",
       "      <td>South Latoya</td>\n",
       "      <td>2019-03-11 12:26:48</td>\n",
       "      <td>9.52</td>\n",
       "      <td>1994999424437</td>\n",
       "      <td>10</td>\n",
       "      <td>Urban</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>6</td>\n",
       "      <td>New Paulville</td>\n",
       "      <td>2019-02-27 11:17:56</td>\n",
       "      <td>43.25</td>\n",
       "      <td>793208410091</td>\n",
       "      <td>44</td>\n",
       "      <td>Urban</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>7</td>\n",
       "      <td>Simpsonburgh</td>\n",
       "      <td>2019-04-26 00:43:24</td>\n",
       "      <td>35.98</td>\n",
       "      <td>111953927754</td>\n",
       "      <td>21</td>\n",
       "      <td>Urban</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>8</td>\n",
       "      <td>South Karenland</td>\n",
       "      <td>2019-01-08 03:28:48</td>\n",
       "      <td>35.09</td>\n",
       "      <td>7995623208694</td>\n",
       "      <td>4</td>\n",
       "      <td>Urban</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>9</td>\n",
       "      <td>North Jasmine</td>\n",
       "      <td>2019-03-09 06:26:29</td>\n",
       "      <td>42.81</td>\n",
       "      <td>5327642267789</td>\n",
       "      <td>33</td>\n",
       "      <td>Urban</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                 City                 Date   Fare        Ride Id  No. Drivers  \\\n",
       "0  Lake Jonathanshire  2019-01-14 10:14:22  13.83  5739410935873            5   \n",
       "1  South Michelleport  2019-03-04 18:24:09  30.24  2343912425577           72   \n",
       "2  Port Samanthamouth  2019-02-24 04:29:00  33.44  2005065760003           57   \n",
       "3          Rodneyfort  2019-02-10 23:22:03  23.44  5149245426178           34   \n",
       "4          South Jack  2019-03-06 04:28:35  34.58  3908451377344           46   \n",
       "5        South Latoya  2019-03-11 12:26:48   9.52  1994999424437           10   \n",
       "6       New Paulville  2019-02-27 11:17:56  43.25   793208410091           44   \n",
       "7        Simpsonburgh  2019-04-26 00:43:24  35.98   111953927754           21   \n",
       "8     South Karenland  2019-01-08 03:28:48  35.09  7995623208694            4   \n",
       "9       North Jasmine  2019-03-09 06:26:29  42.81  5327642267789           33   \n",
       "\n",
       "  City Type  \n",
       "0     Urban  \n",
       "1     Urban  \n",
       "2     Urban  \n",
       "3     Urban  \n",
       "4     Urban  \n",
       "5     Urban  \n",
       "6     Urban  \n",
       "7     Urban  \n",
       "8     Urban  \n",
       "9     Urban  "
      ]
     },
     "execution_count": 22,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "pyber_data_df = pyber_data_df.rename(columns =\n",
    "                                     {\"city\": \"City\",\n",
    "                                     \"date\": \"Date\", \n",
    "                                     \"fare\": \"Fare\", \n",
    "                                     \"ride_id\": \"Ride Id\",\n",
    "                                     \"driver_count\": \"No. Drivers\",\n",
    "                                     \"type\": \"City Type\"})\n",
    "\n",
    "pyber_data_df.head(10)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 2. Set the index column to the Date Column."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "metadata": {},
   "outputs": [],
   "source": [
    "pyber_data_df.set_index(pyber_data_df[\"Date\"], inplace=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 24,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "Index: 2375 entries, 2019-01-14 10:14:22 to 2019-04-25 10:20:13\n",
      "Data columns (total 6 columns):\n",
      "City           2375 non-null object\n",
      "Date           2375 non-null object\n",
      "Fare           2375 non-null float64\n",
      "Ride Id        2375 non-null int64\n",
      "No. Drivers    2375 non-null int64\n",
      "City Type      2375 non-null object\n",
      "dtypes: float64(1), int64(2), object(3)\n",
      "memory usage: 129.9+ KB\n"
     ]
    }
   ],
   "source": [
    "pyber_data_df.info()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 3. Create a new DataFrame for fares and include only the Date, City Type, and Fare columns using the copy() method on the merged DataFrame."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 25,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Date</th>\n",
       "      <th>City Type</th>\n",
       "      <th>Fare</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Date</th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <td>2019-01-14 10:14:22</td>\n",
       "      <td>2019-01-14 10:14:22</td>\n",
       "      <td>Urban</td>\n",
       "      <td>13.83</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-03-04 18:24:09</td>\n",
       "      <td>2019-03-04 18:24:09</td>\n",
       "      <td>Urban</td>\n",
       "      <td>30.24</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-02-24 04:29:00</td>\n",
       "      <td>2019-02-24 04:29:00</td>\n",
       "      <td>Urban</td>\n",
       "      <td>33.44</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-02-10 23:22:03</td>\n",
       "      <td>2019-02-10 23:22:03</td>\n",
       "      <td>Urban</td>\n",
       "      <td>23.44</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-03-06 04:28:35</td>\n",
       "      <td>2019-03-06 04:28:35</td>\n",
       "      <td>Urban</td>\n",
       "      <td>34.58</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-03-11 12:26:48</td>\n",
       "      <td>2019-03-11 12:26:48</td>\n",
       "      <td>Urban</td>\n",
       "      <td>9.52</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-02-27 11:17:56</td>\n",
       "      <td>2019-02-27 11:17:56</td>\n",
       "      <td>Urban</td>\n",
       "      <td>43.25</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-04-26 00:43:24</td>\n",
       "      <td>2019-04-26 00:43:24</td>\n",
       "      <td>Urban</td>\n",
       "      <td>35.98</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-08 03:28:48</td>\n",
       "      <td>2019-01-08 03:28:48</td>\n",
       "      <td>Urban</td>\n",
       "      <td>35.09</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-03-09 06:26:29</td>\n",
       "      <td>2019-03-09 06:26:29</td>\n",
       "      <td>Urban</td>\n",
       "      <td>42.81</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                                    Date City Type   Fare\n",
       "Date                                                     \n",
       "2019-01-14 10:14:22  2019-01-14 10:14:22     Urban  13.83\n",
       "2019-03-04 18:24:09  2019-03-04 18:24:09     Urban  30.24\n",
       "2019-02-24 04:29:00  2019-02-24 04:29:00     Urban  33.44\n",
       "2019-02-10 23:22:03  2019-02-10 23:22:03     Urban  23.44\n",
       "2019-03-06 04:28:35  2019-03-06 04:28:35     Urban  34.58\n",
       "2019-03-11 12:26:48  2019-03-11 12:26:48     Urban   9.52\n",
       "2019-02-27 11:17:56  2019-02-27 11:17:56     Urban  43.25\n",
       "2019-04-26 00:43:24  2019-04-26 00:43:24     Urban  35.98\n",
       "2019-01-08 03:28:48  2019-01-08 03:28:48     Urban  35.09\n",
       "2019-03-09 06:26:29  2019-03-09 06:26:29     Urban  42.81"
      ]
     },
     "execution_count": 25,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "pyber_cities_fares= pyber_data_df[[\"Date\", \"City Type\", \"Fare\"]].copy()\n",
    "pyber_cities_fares.head(10)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 4. Drop the extra Date column."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 26,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>City Type</th>\n",
       "      <th>Fare</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Date</th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <td>2019-01-14 10:14:22</td>\n",
       "      <td>Urban</td>\n",
       "      <td>13.83</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-03-04 18:24:09</td>\n",
       "      <td>Urban</td>\n",
       "      <td>30.24</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-02-24 04:29:00</td>\n",
       "      <td>Urban</td>\n",
       "      <td>33.44</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-02-10 23:22:03</td>\n",
       "      <td>Urban</td>\n",
       "      <td>23.44</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-03-06 04:28:35</td>\n",
       "      <td>Urban</td>\n",
       "      <td>34.58</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-03-11 12:26:48</td>\n",
       "      <td>Urban</td>\n",
       "      <td>9.52</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-02-27 11:17:56</td>\n",
       "      <td>Urban</td>\n",
       "      <td>43.25</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-04-26 00:43:24</td>\n",
       "      <td>Urban</td>\n",
       "      <td>35.98</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-08 03:28:48</td>\n",
       "      <td>Urban</td>\n",
       "      <td>35.09</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-03-09 06:26:29</td>\n",
       "      <td>Urban</td>\n",
       "      <td>42.81</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                    City Type   Fare\n",
       "Date                                \n",
       "2019-01-14 10:14:22     Urban  13.83\n",
       "2019-03-04 18:24:09     Urban  30.24\n",
       "2019-02-24 04:29:00     Urban  33.44\n",
       "2019-02-10 23:22:03     Urban  23.44\n",
       "2019-03-06 04:28:35     Urban  34.58\n",
       "2019-03-11 12:26:48     Urban   9.52\n",
       "2019-02-27 11:17:56     Urban  43.25\n",
       "2019-04-26 00:43:24     Urban  35.98\n",
       "2019-01-08 03:28:48     Urban  35.09\n",
       "2019-03-09 06:26:29     Urban  42.81"
      ]
     },
     "execution_count": 26,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "pyber_cities_fares.drop([\"Date\"], axis=1, inplace=True)\n",
    "pyber_cities_fares.head(10)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 5. Set the index to the datetime data type."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 27,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>City Type</th>\n",
       "      <th>Fare</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Date</th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <td>2019-01-14 10:14:22</td>\n",
       "      <td>Urban</td>\n",
       "      <td>13.83</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-03-04 18:24:09</td>\n",
       "      <td>Urban</td>\n",
       "      <td>30.24</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-02-24 04:29:00</td>\n",
       "      <td>Urban</td>\n",
       "      <td>33.44</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-02-10 23:22:03</td>\n",
       "      <td>Urban</td>\n",
       "      <td>23.44</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-03-06 04:28:35</td>\n",
       "      <td>Urban</td>\n",
       "      <td>34.58</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-03-11 12:26:48</td>\n",
       "      <td>Urban</td>\n",
       "      <td>9.52</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-02-27 11:17:56</td>\n",
       "      <td>Urban</td>\n",
       "      <td>43.25</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-04-26 00:43:24</td>\n",
       "      <td>Urban</td>\n",
       "      <td>35.98</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-08 03:28:48</td>\n",
       "      <td>Urban</td>\n",
       "      <td>35.09</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-03-09 06:26:29</td>\n",
       "      <td>Urban</td>\n",
       "      <td>42.81</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                    City Type   Fare\n",
       "Date                                \n",
       "2019-01-14 10:14:22     Urban  13.83\n",
       "2019-03-04 18:24:09     Urban  30.24\n",
       "2019-02-24 04:29:00     Urban  33.44\n",
       "2019-02-10 23:22:03     Urban  23.44\n",
       "2019-03-06 04:28:35     Urban  34.58\n",
       "2019-03-11 12:26:48     Urban   9.52\n",
       "2019-02-27 11:17:56     Urban  43.25\n",
       "2019-04-26 00:43:24     Urban  35.98\n",
       "2019-01-08 03:28:48     Urban  35.09\n",
       "2019-03-09 06:26:29     Urban  42.81"
      ]
     },
     "execution_count": 27,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "pyber_cities_fares.index = pd.to_datetime(pyber_data_df.index)\n",
    "pyber_cities_fares.head(10)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 6. Check the DataFrame using the info() method to make sure the index is a datetime data type."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 28,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "DatetimeIndex: 2375 entries, 2019-01-14 10:14:22 to 2019-04-25 10:20:13\n",
      "Data columns (total 2 columns):\n",
      "City Type    2375 non-null object\n",
      "Fare         2375 non-null float64\n",
      "dtypes: float64(1), object(1)\n",
      "memory usage: 55.7+ KB\n"
     ]
    }
   ],
   "source": [
    "pyber_cities_fares.info()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 7. Calculate the sum() of fares by city type and date using groupby() to create a new DataFrame."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 29,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "      <th>Fare</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>City Type</th>\n",
       "      <th>Date</th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <td rowspan=\"5\" valign=\"top\">Rural</td>\n",
       "      <td>2019-01-01 09:45:36</td>\n",
       "      <td>43.69</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-02 11:18:32</td>\n",
       "      <td>52.12</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-03 19:51:01</td>\n",
       "      <td>19.90</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-04 03:31:26</td>\n",
       "      <td>24.88</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-06 07:38:40</td>\n",
       "      <td>47.33</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td rowspan=\"5\" valign=\"top\">Urban</td>\n",
       "      <td>2019-05-08 04:20:00</td>\n",
       "      <td>21.99</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-05-08 04:39:49</td>\n",
       "      <td>18.45</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-05-08 07:29:01</td>\n",
       "      <td>18.55</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-05-08 11:38:35</td>\n",
       "      <td>19.77</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-05-08 13:10:18</td>\n",
       "      <td>18.04</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>2375 rows × 1 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "                                Fare\n",
       "City Type Date                      \n",
       "Rural     2019-01-01 09:45:36  43.69\n",
       "          2019-01-02 11:18:32  52.12\n",
       "          2019-01-03 19:51:01  19.90\n",
       "          2019-01-04 03:31:26  24.88\n",
       "          2019-01-06 07:38:40  47.33\n",
       "...                              ...\n",
       "Urban     2019-05-08 04:20:00  21.99\n",
       "          2019-05-08 04:39:49  18.45\n",
       "          2019-05-08 07:29:01  18.55\n",
       "          2019-05-08 11:38:35  19.77\n",
       "          2019-05-08 13:10:18  18.04\n",
       "\n",
       "[2375 rows x 1 columns]"
      ]
     },
     "execution_count": 29,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "sum_fares = pyber_cities_fares.groupby([\"City Type\", \"Date\"]).sum()[[\"Fare\"]]\n",
    "sum_fares"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 30,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "      <th>Fare</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>City Type</th>\n",
       "      <th>Date</th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <td rowspan=\"5\" valign=\"top\">Rural</td>\n",
       "      <td>2019-01-01 09:45:36</td>\n",
       "      <td>43.69</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-02 11:18:32</td>\n",
       "      <td>52.12</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-03 19:51:01</td>\n",
       "      <td>19.90</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-04 03:31:26</td>\n",
       "      <td>24.88</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-06 07:38:40</td>\n",
       "      <td>47.33</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td rowspan=\"5\" valign=\"top\">Urban</td>\n",
       "      <td>2019-05-08 04:20:00</td>\n",
       "      <td>21.99</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-05-08 04:39:49</td>\n",
       "      <td>18.45</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-05-08 07:29:01</td>\n",
       "      <td>18.55</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-05-08 11:38:35</td>\n",
       "      <td>19.77</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-05-08 13:10:18</td>\n",
       "      <td>18.04</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>2375 rows × 1 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "                                Fare\n",
       "City Type Date                      \n",
       "Rural     2019-01-01 09:45:36  43.69\n",
       "          2019-01-02 11:18:32  52.12\n",
       "          2019-01-03 19:51:01  19.90\n",
       "          2019-01-04 03:31:26  24.88\n",
       "          2019-01-06 07:38:40  47.33\n",
       "...                              ...\n",
       "Urban     2019-05-08 04:20:00  21.99\n",
       "          2019-05-08 04:39:49  18.45\n",
       "          2019-05-08 07:29:01  18.55\n",
       "          2019-05-08 11:38:35  19.77\n",
       "          2019-05-08 13:10:18  18.04\n",
       "\n",
       "[2375 rows x 1 columns]"
      ]
     },
     "execution_count": 30,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "sum_fares_df = pd.DataFrame(sum_fares)\n",
    "sum_fares_df"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 8. Reset the index, which is needed for Step 10."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 31,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>City Type</th>\n",
       "      <th>Date</th>\n",
       "      <th>Fare</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <td>0</td>\n",
       "      <td>Rural</td>\n",
       "      <td>2019-01-01 09:45:36</td>\n",
       "      <td>43.69</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>1</td>\n",
       "      <td>Rural</td>\n",
       "      <td>2019-01-02 11:18:32</td>\n",
       "      <td>52.12</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2</td>\n",
       "      <td>Rural</td>\n",
       "      <td>2019-01-03 19:51:01</td>\n",
       "      <td>19.90</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>3</td>\n",
       "      <td>Rural</td>\n",
       "      <td>2019-01-04 03:31:26</td>\n",
       "      <td>24.88</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>4</td>\n",
       "      <td>Rural</td>\n",
       "      <td>2019-01-06 07:38:40</td>\n",
       "      <td>47.33</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>5</td>\n",
       "      <td>Rural</td>\n",
       "      <td>2019-01-08 06:19:45</td>\n",
       "      <td>19.39</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>6</td>\n",
       "      <td>Rural</td>\n",
       "      <td>2019-01-09 15:30:35</td>\n",
       "      <td>31.84</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>7</td>\n",
       "      <td>Rural</td>\n",
       "      <td>2019-01-11 04:39:27</td>\n",
       "      <td>16.42</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>8</td>\n",
       "      <td>Rural</td>\n",
       "      <td>2019-01-14 07:09:17</td>\n",
       "      <td>18.05</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>9</td>\n",
       "      <td>Rural</td>\n",
       "      <td>2019-01-14 15:58:48</td>\n",
       "      <td>54.10</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "  City Type                Date   Fare\n",
       "0     Rural 2019-01-01 09:45:36  43.69\n",
       "1     Rural 2019-01-02 11:18:32  52.12\n",
       "2     Rural 2019-01-03 19:51:01  19.90\n",
       "3     Rural 2019-01-04 03:31:26  24.88\n",
       "4     Rural 2019-01-06 07:38:40  47.33\n",
       "5     Rural 2019-01-08 06:19:45  19.39\n",
       "6     Rural 2019-01-09 15:30:35  31.84\n",
       "7     Rural 2019-01-11 04:39:27  16.42\n",
       "8     Rural 2019-01-14 07:09:17  18.05\n",
       "9     Rural 2019-01-14 15:58:48  54.10"
      ]
     },
     "execution_count": 31,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "sum_fares_df = sum_fares_df.reset_index()\n",
    "sum_fares_df.head(10)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 9. Create a pivot table DataFrame with the Date as the index and columns = 'City Type' with the Fare for each Date in each row. Note: There will be NaNs in some rows, which will be taken care of when you sum based on the date."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 32,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th>City Type</th>\n",
       "      <th>Rural</th>\n",
       "      <th>Suburban</th>\n",
       "      <th>Urban</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Date</th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <td>2019-01-01 00:08:16</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "      <td>37.91</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-01 00:46:46</td>\n",
       "      <td>NaN</td>\n",
       "      <td>47.74</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-01 02:07:24</td>\n",
       "      <td>NaN</td>\n",
       "      <td>24.07</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-01 03:46:50</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "      <td>7.57</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-01 05:23:21</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "      <td>10.75</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-01 09:45:36</td>\n",
       "      <td>43.69</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-01 12:32:48</td>\n",
       "      <td>NaN</td>\n",
       "      <td>25.56</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-01 14:40:14</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "      <td>5.42</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-01 14:42:25</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "      <td>12.31</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-01 14:52:06</td>\n",
       "      <td>NaN</td>\n",
       "      <td>31.15</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "City Type            Rural  Suburban  Urban\n",
       "Date                                       \n",
       "2019-01-01 00:08:16    NaN       NaN  37.91\n",
       "2019-01-01 00:46:46    NaN     47.74    NaN\n",
       "2019-01-01 02:07:24    NaN     24.07    NaN\n",
       "2019-01-01 03:46:50    NaN       NaN   7.57\n",
       "2019-01-01 05:23:21    NaN       NaN  10.75\n",
       "2019-01-01 09:45:36  43.69       NaN    NaN\n",
       "2019-01-01 12:32:48    NaN     25.56    NaN\n",
       "2019-01-01 14:40:14    NaN       NaN   5.42\n",
       "2019-01-01 14:42:25    NaN       NaN  12.31\n",
       "2019-01-01 14:52:06    NaN     31.15    NaN"
      ]
     },
     "execution_count": 32,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "sum_fares_pivot = sum_fares_df.pivot(index=\"Date\", columns=\"City Type\")[\"Fare\"]\n",
    "sum_fares_pivot.head(10)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 10. Create a new DataFrame from the pivot table DataFrame on the given dates [Jan. 1, 2019 to Apr. 28, 2019] using loc."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 33,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th>City Type</th>\n",
       "      <th>Rural</th>\n",
       "      <th>Suburban</th>\n",
       "      <th>Urban</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Date</th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <td>2019-01-01 00:08:16</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "      <td>37.91</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-01 00:46:46</td>\n",
       "      <td>NaN</td>\n",
       "      <td>47.74</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-01 02:07:24</td>\n",
       "      <td>NaN</td>\n",
       "      <td>24.07</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-01 03:46:50</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "      <td>7.57</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-01 05:23:21</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "      <td>10.75</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-04-28 14:28:36</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "      <td>11.46</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-04-28 16:29:16</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "      <td>36.42</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-04-28 17:26:52</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "      <td>31.43</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-04-28 17:38:09</td>\n",
       "      <td>NaN</td>\n",
       "      <td>34.87</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-04-28 19:35:03</td>\n",
       "      <td>NaN</td>\n",
       "      <td>16.96</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>2196 rows × 3 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "City Type            Rural  Suburban  Urban\n",
       "Date                                       \n",
       "2019-01-01 00:08:16    NaN       NaN  37.91\n",
       "2019-01-01 00:46:46    NaN     47.74    NaN\n",
       "2019-01-01 02:07:24    NaN     24.07    NaN\n",
       "2019-01-01 03:46:50    NaN       NaN   7.57\n",
       "2019-01-01 05:23:21    NaN       NaN  10.75\n",
       "...                    ...       ...    ...\n",
       "2019-04-28 14:28:36    NaN       NaN  11.46\n",
       "2019-04-28 16:29:16    NaN       NaN  36.42\n",
       "2019-04-28 17:26:52    NaN       NaN  31.43\n",
       "2019-04-28 17:38:09    NaN     34.87    NaN\n",
       "2019-04-28 19:35:03    NaN     16.96    NaN\n",
       "\n",
       "[2196 rows x 3 columns]"
      ]
     },
     "execution_count": 33,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "fares_Jan_April_df = sum_fares_pivot.loc['2019-01-01':'2019-04-28']\n",
    "fares_Jan_April_df"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 11. Create a new DataFrame by setting the DataFrame you created in Step 10 with resample() in weekly bins, and calculate the sum() of the fares for each week."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 34,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th>City Type</th>\n",
       "      <th>Rural</th>\n",
       "      <th>Suburban</th>\n",
       "      <th>Urban</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Date</th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <td>2019-01-06</td>\n",
       "      <td>187.92</td>\n",
       "      <td>721.60</td>\n",
       "      <td>1661.68</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-13</td>\n",
       "      <td>67.65</td>\n",
       "      <td>1105.13</td>\n",
       "      <td>2050.43</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-20</td>\n",
       "      <td>306.00</td>\n",
       "      <td>1218.20</td>\n",
       "      <td>1939.02</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-01-27</td>\n",
       "      <td>179.69</td>\n",
       "      <td>1203.28</td>\n",
       "      <td>2129.51</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-02-03</td>\n",
       "      <td>333.08</td>\n",
       "      <td>1042.79</td>\n",
       "      <td>2086.94</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-02-10</td>\n",
       "      <td>115.80</td>\n",
       "      <td>974.34</td>\n",
       "      <td>2162.64</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-02-17</td>\n",
       "      <td>95.82</td>\n",
       "      <td>1045.50</td>\n",
       "      <td>2235.07</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-02-24</td>\n",
       "      <td>419.06</td>\n",
       "      <td>1412.74</td>\n",
       "      <td>2466.29</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-03-03</td>\n",
       "      <td>175.14</td>\n",
       "      <td>858.46</td>\n",
       "      <td>2218.20</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-03-10</td>\n",
       "      <td>303.94</td>\n",
       "      <td>925.27</td>\n",
       "      <td>2470.93</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-03-17</td>\n",
       "      <td>163.39</td>\n",
       "      <td>906.20</td>\n",
       "      <td>2044.42</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-03-24</td>\n",
       "      <td>189.76</td>\n",
       "      <td>1122.20</td>\n",
       "      <td>2368.37</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-03-31</td>\n",
       "      <td>199.42</td>\n",
       "      <td>1045.06</td>\n",
       "      <td>1942.77</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-04-07</td>\n",
       "      <td>501.24</td>\n",
       "      <td>1010.73</td>\n",
       "      <td>2356.70</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-04-14</td>\n",
       "      <td>269.79</td>\n",
       "      <td>784.82</td>\n",
       "      <td>2390.72</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-04-21</td>\n",
       "      <td>214.14</td>\n",
       "      <td>1149.27</td>\n",
       "      <td>2303.80</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2019-04-28</td>\n",
       "      <td>191.85</td>\n",
       "      <td>1357.75</td>\n",
       "      <td>2238.29</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "City Type    Rural  Suburban    Urban\n",
       "Date                                 \n",
       "2019-01-06  187.92    721.60  1661.68\n",
       "2019-01-13   67.65   1105.13  2050.43\n",
       "2019-01-20  306.00   1218.20  1939.02\n",
       "2019-01-27  179.69   1203.28  2129.51\n",
       "2019-02-03  333.08   1042.79  2086.94\n",
       "2019-02-10  115.80    974.34  2162.64\n",
       "2019-02-17   95.82   1045.50  2235.07\n",
       "2019-02-24  419.06   1412.74  2466.29\n",
       "2019-03-03  175.14    858.46  2218.20\n",
       "2019-03-10  303.94    925.27  2470.93\n",
       "2019-03-17  163.39    906.20  2044.42\n",
       "2019-03-24  189.76   1122.20  2368.37\n",
       "2019-03-31  199.42   1045.06  1942.77\n",
       "2019-04-07  501.24   1010.73  2356.70\n",
       "2019-04-14  269.79    784.82  2390.72\n",
       "2019-04-21  214.14   1149.27  2303.80\n",
       "2019-04-28  191.85   1357.75  2238.29"
      ]
     },
     "execution_count": 34,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "weekly_fares_df = fares_Jan_April_df.resample(\"W\").sum()\n",
    "weekly_fares_df"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 12. Using the object-oriented interface method, plot the DataFrame you created in Step 12 using the df.plot() function. "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 35,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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u+DqfwG0b8+H8li219PqLT9BuXsHJLsNkstZOS4kCfY7HoW795BGnjDVFy4vVkP940po/veJ15ic1Bq2++cBerZEICIiIiIKJXLn7rA+/RYcV/0bwieTUlFqguSweY3ZJ8+Ca+DFjTlFqmcMaE9D9fvP0Cxb4jUmR8XCOmsBRHRckGZFRERERETVUmvguPZWWJ98E+72Kaf9NPuk6XBdcHkjTowaAgPaAJR7dkL71lzvjeJaHWz3z4MIsOGciIiIiIhCi5ycCusTb8A+fiqEz1ZB+zVT2KmkieAeWh+KnEzoFj/mnVuvVMJ2zzOQO3YN4syIiIiIiOiMqFRwXvkvuPqcD+2nb0Fx/BCcI8bCOWp8sGdG9YQBbRXSyePQvTgHks3iNW6/dQ7c5/QP0qyIiIiIiKguRJsOsM18PtjToAbAlOMKJSboFzwIRXGh17D9hjvhGjoqSJMiIiIiIiKi02FAW07/8sNQ5B7xGnOMvg7OMTcEaUZERERERERUHQa05ZQH9no9dg68GI4JdwGSFKQZERERERERUXUY0Abg6t4H9tseAhT85yEiIiIiIgpVjNh8uNunwDbjGUCtCfZUiIiIiIiIqBoMaKuQE1vB9sALgN4Y7KkQERERERFRDRjQlhOR0bDOmg8REx/sqRAREREREVEtMKAtZ73/BYiW7YI9DSIiIiIiIqolBrTl5E5pwZ4CERERERERnQEGtERERERERBSWGNASERERERFRWGJAS0RERERERGGJAS0RERERERGFJQa0REREREREFJYY0BIREREREVFYYkBLREREREREYYkBLREREREREYUlBrREREREREQUlhjQEhERERERUVhiQEtERERERERhiQEtERERERERhSUGtERERERERBSWGNASERERERFRWGJAS0RERERERGGJAS0RERERERGFJQa0REREREREFJYY0BIREREREVFYYkBLREREREREYYkBLREREREREYUlBrREREREREQUlhjQEhERERERUVhiQEtERERERERhiQEtERERERERhSUGtERERERERBSWGNASERERERFRWGJAS0RERERERGGJAS0RERERERGFJQa0REREREREFJZCKqCdO3cuYmJivP506dKl8nkhBObOnYu0tDS0bNkSl19+Ofbu3ev1GiaTCbfffjvat2+P9u3b4/bbb4fJZGrs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\n",
      "text/plain": [
       "<Figure size 1008x576 with 1 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "# Use the graph style fivethirtyeight\n",
    "plt.style.use('fivethirtyeight')\n",
    "weekly_fares_df.plot(figsize=(14, 8))\n",
    "plt.gcf().subplots_adjust(bottom=0.15)\n",
    "\n",
    "# Add graph properties\n",
    "plt.title(\"Total Fare by City Type\", fontsize=16)\n",
    "plt.ylabel(\"Fare ($USD)\", fontsize=14)\n",
    "plt.xlabel(\"Month\", fontsize=14)\n",
    "\n",
    "# Create a legend\n",
    "lgnd = plt.legend(fontsize=\"10\", loc=\"best\", title=\"City Type\")\n",
    "lgnd.get_title().set_fontsize(12)\n",
    "\n",
    "# Save figure\n",
    "plt.savefig(\"Analysis/Fig8.png\")\n",
    "\n",
    "# Show figure\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "celltoolbar": "Raw Cell Format",
  "kernelspec": {
   "display_name": "PythonData",
   "language": "python",
   "name": "pythondata"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.7.4"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 4
}
