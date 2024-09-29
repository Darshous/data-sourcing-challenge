# Data-Sourcing-Challenge
# CME and GST Data Analysis

This project is designed to fetch, process, and analyze data on Coronal Mass Ejections (CMEs) and Geomagnetic Storms (GSTs) using NASA's DONKI API. The goal is to merge CME and GST datasets, calculate the time difference between these events, and export the final processed data into a CSV file.

## How It Works

### Step 1: Fetch CME Data
- The program requests data on CMEs from the NASA DONKI API for a specific date range (2013-05-01 to 2024-05-01).
- The fetched data is stored as a JSON object, converted into a Pandas DataFrame, and then processed to extract relevant fields like `activityID`, `startTime`, and `linkedEvents`.

### Step 2: Fetch GST Data
- Similarly, the program fetches data on GSTs from the NASA DONKI API for the same date range.
- The resulting JSON object is converted into a DataFrame, and fields such as `gstID`, `startTime`, and `linkedEvents` are extracted.

### Step 3: Data Processing
- Both CME and GST data are processed to ensure that only relevant events are kept. For example:
    - Rows where linked events are missing are removed.
    - The `linkedEvents` are expanded and matched between the CME and GST datasets.
- The function `extract_activityID_from_dict` is used to extract the corresponding activity IDs from the `linkedEvents`.

### Step 4: Data Merging
- The CME and GST datasets are merged using corresponding activity IDs (`gstID`, `CME_ActivityID`, `GST_ActivityID`, `cmeID`).
- The merged dataset contains columns for `gstID`, `startTime_GST`, `CME_ActivityID`, `cmeID`, `startTime_CME`, and the time difference between the events.

### Step 5: Time Difference Calculation
- The program computes the time difference between the `startTime_GST` and `startTime_CME` columns and stores the result in a new column called `timeDiff`.
- The `describe()` method is used to compute statistical summaries (mean, median, etc.) for the time difference.

### Step 6: Export to CSV
- The final merged dataset, including the time difference, is exported to a CSV file (`cme_gst_data.csv`) without the DataFrame index.

## File Structure

- `retrieve_data.ipynb`: The main Jupyter notebook where the entire process is implemented.
- `output/cme_gst_data.csv`: The output CSV file containing the final merged data with the computed time differences.

## Dependencies

The project requires the following Python libraries:
- `pandas`: For data manipulation and analysis.
- `requests`: For making API calls to the NASA DONKI API.
- `python-dotenv`: For securely handling API keys.

You can install the required libraries using the following command:

```bash
pip install pandas requests python-dotenv
```

## How to Run

1. Clone the repository to your local machine.
2. Install the required dependencies using `pip`.
3. Create a `.env` file in the root directory and add your NASA API key in the following format:

   ```
   NASA_API_KEY=your_api_key_here
   ```

4. Open the `retrieve_data.ipynb` notebook and run all the cells sequentially to fetch, process, and export the data.
5. The final output will be saved as `cme_gst_data.csv` in the `output` folder.

## Example Output

The final CSV file (`cme_gst_data.csv`) will contain the following columns:
- `gstID`: The Geomagnetic Storm ID
- `startTime_GST`: The start time of the GST event
- `CME_ActivityID`: The CME Activity ID
- `cmeID`: The Coronal Mass Ejection ID
- `startTime_CME`: The start time of the CME event
- `GST_ActivityID`: The corresponding GST Activity ID
- `timeDiff`: The time difference between the CME and GST events (e.g., `4 days 06:36:00`)

## Summary Statistics

Using the `describe()` function, you can compute the mean and median time difference between CMEs and GSTs. For example:

```
mean    2 days 21:35:13
median  2 days 17:48:00
```

## License

This project is intended for educational purposes and is free to use.
