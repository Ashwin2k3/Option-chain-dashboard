<img width="1279" alt="image" src="https://github.com/user-attachments/assets/37a1f765-85ee-4546-9cff-fdea7f5955db">
<img width="1280" alt="image" src="https://github.com/user-attachments/assets/243fe180-8507-4756-8897-b901e6cccd62">
<img width="1278" alt="image" src="https://github.com/user-attachments/assets/97777ef5-613f-4495-9ca8-55d5493fb65a">




* [Libraries Used](#libraries-used)
   * [1. **Streamlit (st)**](#1-streamlit-st)
   * [2. **Requests**](#2-requests)
   * [3. **Pandas (pd)**](#3-pandas-pd)
   * [4. **Streamlit Autorefresh**](#4-streamlit-autorefresh)
   * [5. **Plotly Express (px)**](#5-plotly-express-px)
* [Page Configuration](#page-configuration)
* [Custom CSS Styling](#custom-css-styling)
* [Key Functions](#key-functions)
   * [1. **fetch_nse_data()**](#1-fetch_nse_data)
   * [2. **find_atm_strike(data)**](#2-find_atm_strikedata)
   * [3. **display_atm_otm_data(data-selected_strike)**](#3-display_atm_otm_datadata-selected_strike)
* [Main App Structure (**main()**)](#main-app-structure-main)
   * [Sidebar Configuration](#sidebar-configuration)
   * [Data Fetching and Visualization](#data-fetching-and-visualization)
* [Final Notes](#final-notes)

    
### Libraries Used
```python
import streamlit as st
import requests
import pandas as pd
import time
from streamlit_autorefresh import st_autorefresh
import plotly.express as px
```

#### 1. **Streamlit (`st`)**
   - **Purpose**: Streamlit is used to build the interactive dashboard.
   - **Usage**: Provides the core UI components like sidebar, buttons, and various layout elements.

#### 2. **Requests**
   - **Purpose**: Handles the HTTP request to fetch data from the NSE Option Chain API.
   - **Usage**: Makes a GET request to NSE API and parses the response.

#### 3. **Pandas (`pd`)**
   - **Purpose**: Pandas is used for data manipulation and transformation.
   - **Usage**: Creates a DataFrame to store and format option chain data.

#### 4. **Streamlit Autorefresh**
   - **Purpose**: Adds an auto-refresh mechanism to the app.
   - **Usage**: Automatically refreshes the app at specified intervals to fetch updated data.

#### 5. **Plotly Express (`px`)**
   - **Purpose**: Used for data visualization.
   - **Usage**: Plots bar charts to represent Call and Put Open Interest at different strike prices.

---

### Page Configuration

The first command in the app sets the page configuration using `st.set_page_config()`:

```python
st.set_page_config(page_title="NSE Option Chain Dashboard", layout="wide", page_icon="📈")
```

- **page_title**: The title displayed on the browser tab.
- **layout**: Sets the layout to wide for a broader view.
- **page_icon**: Displays a stock chart icon next to the page title.

---

### Custom CSS Styling

The app uses custom CSS to style the title, subtitle, and data tables, enhancing the UI:

```python
st.markdown("""
    <style>
    .title { ... }
    .subtitle { ... }
    .metrics { ... }
    .dataframe { ... }
    </style>
""", unsafe_allow_html=True)
```

---

### Key Functions

#### 1. **`fetch_nse_data()`**
   - **Purpose**: Fetches option chain data from NSE.
   - **Usage**: 
     - Makes a request to NSE’s option chain API for NIFTY index data.
     - Handles headers and returns the JSON response or displays an error.
   - **Output**: Returns JSON data from the API.
   
#### 2. **`find_atm_strike(data)`**
   - **Purpose**: Determines the At-the-Money (ATM) strike price based on the current NIFTY index value.
   - **Input**: The option chain data from the NSE API.
   - **Logic**:
     - Extracts the NIFTY index value from the data.
     - Identifies the strike price closest to the current index value.
   - **Output**: Returns ATM strike, NIFTY price, and all strikes available.

#### 3. **`display_atm_otm_data(data, selected_strike)`**
   - **Purpose**: Calculates the Put-Call Ratio (PCR) and displays data for selected and nearby strikes.
   - **Input**: 
     - `data`: Option chain data.
     - `selected_strike`: The ATM strike price.
   - **Logic**:
     - Iterates through all available strikes.
     - Calculates the total open interest for calls and puts.
     - Computes PCR and displays nearby strikes (10 ATM + 10 OTM).
   - **Output**: Returns the overall PCR and a DataFrame containing nearby strikes.

---

### Main App Structure (`main()`)

#### Sidebar Configuration

The app includes a sidebar to adjust the auto-refresh interval and trigger the data fetching process:

```python
refresh_time = st.sidebar.slider("Auto-refresh interval (minutes):", min_value=1, max_value=60, value=5, step=1)
if st.sidebar.button('Fetch NSE Data'):
    # Fetch data when button is pressed
```

- **Auto-refresh interval**: Allows users to select how often the app should refresh the data.
- **Fetch NSE Data**: Triggers the data fetching function when clicked.

#### Data Fetching and Visualization

When the "Fetch NSE Data" button is clicked, the following steps occur:
1. **Data Fetching**:
   - The app calls `fetch_nse_data()` to retrieve data from the NSE API.
   
2. **ATM and Strike Calculation**:
   - It uses `find_atm_strike()` to determine the ATM strike price based on NIFTY index data.
   
3. **Display Key Metrics**:
   - The NIFTY Index value, ATM strike price, and PCR are displayed using `st.metric()`.

4. **Data Visualization**:
   - Plots a bar chart of Call and Put Open Interest for selected strikes using Plotly:
   ```python
   fig = px.bar(strike_data, x="Strike Price", y=["Call Open Interest", "Put Open Interest"], barmode="group")
   ```

5. **Auto-refresh Setup**:
   - Sets the app to refresh at intervals defined by the user using `st_autorefresh()`.

---

### Final Notes

- **Error Handling**: The app includes error handling to display meaningful error messages in case of failed API requests or exceptions.
- **Responsive Layout**: The dashboard is optimized for wide screen layouts, making it suitable for tracking large datasets.
- **Customization**: Users can customize refresh intervals, and the styling can be adjusted to fit specific design needs.


