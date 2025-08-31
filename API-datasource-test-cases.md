I'll provide you with comprehensive step-by-step instructions for using the API data source integration functionality in the Data Explorer.

## How to Use the API Data Source Integration Feature

### Step 1: Access the API Sources Tab
1. Navigate to **Data Explorer** (you're already there!)
2. Click on the **"API Sources"** tab (4th tab after Search, Upload Data, and Data Sources)
3. You'll see the API Sources grid interface with an **"Add API Source"** button

### Step 2: Adding Your First API Source (FBI Wanted API - No Authentication)

1. **Click "Add API Source"** button
2. In the **Basic Info** tab:
   - **Name**: `FBI Wanted Persons API`
   - **Description**: `Public FBI database of wanted criminals`
   - **Base URL**: `https://api.fbi.gov/wanted/v1`
   - **API Type**: Select `REST`
   - **Tags**: Add tags like `law-enforcement`, `public-data`

3. **Authentication** tab:
   - **Auth Type**: Select `None` (this is a public API)
   - Leave credentials empty

4. **Configuration** tab:
   - **Timeout**: Leave default `30` seconds
   - **Rate Limit**: Leave default `60` per minute
   - **Health Check URL**: Use `https://api.fbi.gov/wanted/v1/list`

5. **Sharing** tab:
   - **Visibility**: Choose `Private` (only you can see it)
   - **Share with Organization**: Toggle ON if you want your team to access it

6. **Click "Create API Source"**

### Step 3: Test the Connection
1. After creating, you'll see your FBI API source in the grid
2. **Click the "Test Connection" button** (play icon)
3. You should see a green success toast: "Connection test successful"
4. The status badge should show "Active" with a green dot

### Step 4: View API Source Details
1. **Click on the API source card** to open details dialog
2. Explore the **4 tabs**:
   - **Overview**: Basic information and metadata
   - **Health**: Real-time health metrics and status
   - **Logs**: Execution history and audit trail
   - **Query Builder**: (Future enhancement for custom queries)

### Step 5: Adding an API Key-Protected Source (OpenWeatherMap)

1. **Click "Add API Source"** again
2. **Basic Info** tab:
   - **Name**: `OpenWeatherMap API`
   - **Description**: `Weather data for incident correlation`
   - **Base URL**: `https://api.openweathermap.org/data/2.5`
   - **API Type**: `REST`

3. **Authentication** tab:
   - **Auth Type**: Select `API Key`
   - **API Key**: Enter your OpenWeatherMap API key
   - **Header Name**: `appid` (this goes in query parameters for OpenWeatherMap)

4. **Configuration** tab:
   - **Health Check URL**: `https://api.openweathermap.org/data/2.5/weather?q=London&appid=YOUR_KEY`

5. **Create and test** the connection

### Step 6: Adding a GraphQL API (SpaceX)

1. **Add new API source**
2. **Basic Info**:
   - **Name**: `SpaceX GraphQL API`
   - **Base URL**: `https://api.spacex.land/graphql`
   - **API Type**: Select `GraphQL`

3. **Authentication**: Select `None`
4. **Create and test**

### Step 7: Adding a SOAP API (Currency Converter)

1. **Add new API source**
2. **Basic Info**:
   - **Name**: `Currency Converter SOAP`
   - **Base URL**: `http://currencyconverter.kowabunga.net/converter.asmx`
   - **API Type**: Select `SOAP`

3. **Authentication**: Select `None`
4. **Create and test**

### Step 8: Monitor Your API Sources

1. **Grid View Features**:
   - **Status Indicators**: Green (Active), Red (Error), Yellow (Testing)
   - **API Type Badges**: Different colors for REST, GraphQL, SOAP
   - **Auth Type Badges**: Shows authentication method
   - **Last Tested**: Timestamp of last connection test

2. **Health Monitoring**:
   - Click any API source to view health metrics
   - Check **response times**, **availability percentage**
   - View **error messages** if connections fail

3. **Execution Logs**:
   - See detailed logs of all API calls
   - Track **request/response data**, **status codes**
   - Monitor **performance metrics**

### Step 9: Organization Sharing

1. **For shared sources**:
   - Toggle "Share with Organization" when creating
   - Team members can see and use shared sources
   - **Visibility levels**: Private, Organization, Public

2. **Role-based access**:
   - Regular users: Can create and manage their own sources
   - Organization admins: Can see all org sources
   - Aurora admins: Can manage all sources

### Step 10: Error Handling and Troubleshooting

1. **Connection Failures**:
   - Check the **error message** in the toast notification
   - Verify **URL format** and **authentication credentials**
   - Check **network connectivity** and **API status**

2. **Health Monitoring**:
   - Failed sources show **red status** with error details
   - Use **Health tab** to see specific error messages
   - Check **Logs tab** for detailed failure information

### Step 11: Advanced Features

1. **Custom Headers**:
   - Add custom headers in the Configuration tab
   - Useful for additional authentication or API requirements

2. **Query Parameters**:
   - Set default query parameters for API calls
   - Helpful for common filters or API versions

3. **Rate Limiting**:
   - Configure per-minute limits to respect API quotas
   - Prevents overwhelming external services

### Testing Workflow Summary:

1. **Start Simple**: Begin with public APIs (FBI, JSONPlaceholder)
2. **Add Authentication**: Test API key sources (OpenWeatherMap)
3. **Try Different Types**: GraphQL (SpaceX), SOAP (Currency)
4. **Monitor Health**: Check status, logs, and performance
5. **Share with Team**: Use organization sharing features
6. **Handle Errors**: Practice troubleshooting failed connections

The interface is designed to be intuitive - just follow the guided tabs when creating sources, and use the grid view to manage and monitor all your API connections in one place!
