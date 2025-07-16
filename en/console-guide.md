## Network > DNS Plus > Console User Guide

This guide describes how to manage DNS Zones and record sets in the console.

## Manage DNS Zones

You can manage DNS Zones on the **DNS** screen of the menu.

![image_01_20210604](https://static.toastoven.net/prod_dnsplus/image_01_20210604.png)

### Create a DNS Zone

1. A DNS Zone is a container for record sets, which is a domain zone for hosts served by DNS. Click **Create DNS Zone** to create a DNS Zone.

2. Enter **DNS Zone Name** and **Description**, and click **Confirm**.  

    - In **DNS Zone Name**, enter the domain or subdomain you own as [fully qualified domain name (FQDN)](https://en.wikipedia.org/wiki/Fully_qualified_domain_name).
    - **DNS Zone Name** must be unique on the DNS server.
    - The same **DNS Zone Name** can be created as many as the number of DNS servers. There are 3 DNS servers.
    - After the DNS Zone is created, you must set the name server information of the NS record set that is created by default to the domain. For the record sets created by default, see [Manage Record Sets](./console-guide/#manage-record-sets).
        - If the DNS Zone was created with a newly registered domain, you must set the name server information to the corresponding name server in the domain registrar.
        - If the DNS Zone was created with a subdomain of a domain currently in operation, you must create an NS record set in your operating domain with the subdomain name and its name server.

![image_02_20210604](https://static.toastoven.net/prod_dnsplus/image_02_20210604.png)

### Modify a DNS Zone

1. Select the DNS Zone to modify and click **Modify DNS Zone**.

2. Modify **Description** and click **Confirm**.

![image_03_20210604](https://static.toastoven.net/prod_dnsplus/image_03_20210604.png)

### Delete DNS Zones

1. Select all DNS Zones to delete and click **Delete DNS Zone**.

2. Click **Confirm**. All record sets within the DNS Zone will be deleted and it will take some time to finish the job.

![image_04_20210604](https://static.toastoven.net/prod_dnsplus/image_04_20210604.png)


## Manage Record Sets

You can manage record sets of the DNS Zone selected on the **DNS** screen of the menu.

- You can create record sets for **DNS Zone Name** or sub-name by record set type.
- SOA and NS record sets for **DNS Zone Name** are created by default and cannot be modified or deleted.
- SOA record sets cannot be created, modified, or deleted, and NS record sets cannot be created, modified, or deleted using **DNS Zone Name**.

![image_05_20210604](https://static.toastoven.net/prod_dnsplus/image_05_20210604.png)

### Create a Record Set

1. A record set is the information of the host to serve. Click **Create Record Set** to create a record set.

2. Enter the record set information.

    - Record Set Name: Name of the host to serve. Enter in [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) format so that it can be a **DNS Zone Name** or a sub-name.
    - Record Set Type: Type of the host. Select the type according to the purpose of use.
    - TTL (seconds): Time to live, which indicates the validity period of the data. Enter the update cycle of the record set information in the name server (in seconds). You can enter a value easily by clicking the buttons on the right.

3. Enter the record information according to the record set type.

    - A different record value must be entered depending on the record set type. Enter the record value according to the description displayed at the bottom of the screen.
    - Depending on the record set type, multiple records can be entered. You can add a record by clicking the **+** button on the right side of the screen, and you can remove a record by clicking the **-** button of the added record.

4. After completing the configuration, click **Confirm**.

5. There is a limit to the maximum number of record sets that can be created. If you want to raise the limit, please contact us. 

    - Contact: [1:1 Inquiry](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

![image_06_20210604](https://static.toastoven.net/prod_dnsplus/image_06_20210604.png)

### Create Multiple Record Sets

1. Click **Create Multiple Record Sets**.

2. Click **Download Template** to download a template. Enter the record set information on the downloaded template. Click **Select File** to select a template.

3. On the screen, check the information entered on the template and click **Confirm**.

![image_06_2_20210802](https://static.toastoven.net/prod_dnsplus/image_06_2_20210802.png)

### Modify a Record Set

1. Select a record set to modify and click the **Modify Record Set** button.

2. **Record Set Name** cannot be modified. However, **Record Set Type**, **TTL (seconds)**, and **Record Value** can be modified.

3. After completing the configuration, click **Confirm**.

![image_07_20210604](https://static.toastoven.net/prod_dnsplus/image_07_20210604.png)

### Delete Record Sets

1. Select all record sets to delete and click **Delete Record Set**.

2. Click **Confirm**.

![image_08_20210604](https://static.toastoven.net/prod_dnsplus/image_08_20210604.png)

### Record Set Statistics

1. Select the record set to view and click **Record Set Statistics**.

2. You can view the query information and average response time of the record set for the last seven days.

### Search Record Sets and View Results

1. You can view only the record set type selected using the **Type filter**.

2. Enter a search term and press **Enter Key** or click **Search** to search by the record set name.  

    - All values containing the search term are retrieved.

![image_10_20210604](https://static.toastoven.net/prod_dnsplus/image_10_20210604.png)

## Manage GSLB and Connected Pools

You can manage GSLB (global server load balancing) from the **GSLB** screen of the menu, and manage the pool connection of the selected GSLB.

![image_11_20210604](https://static.toastoven.net/prod_dnsplus/image_11_20210604.png)

### Create a GSLB

For a **GSLB domain**, traffic is reliably load balanced according to the **routing rule**. You can create a GSLB as follows:

1. Click **Create GSLB**.

2. Enter information in the **Create GSLB** dialog box.

    - GSLB Name: You can set uppercase and lowercase letters, numbers, '-' and '_'.
    - Routing Rule: You can select FAILOVER, RANDOM, or GEOLOCATION as the load balancing method for the GSLB domain.
        - FAILOVER: Performs routing based on the priorities of connected pools.
        - RANDOM: Performs routing by randomly selecting an available pool among connected pools.
        - GEOLOCATION: Routes the traffic of the configured region to the connected pool. When there is no configured region, performs routing based on the priorities of connected pools.
    - TTL (seconds): Time to live, which indicates the validity period of the data. Enter the update cycle of the GSLB domain (in seconds). You can enter a value easily by clicking the buttons on the right.
    - Status: Choose whether to activate the GSLB domain.
    
3. After completing the configuration, click **Confirm**.

4. There is a limit to the maximum number of GSLBs that can be created. If you want to raise the limit, please contact us. 

    - Contact: [1:1 Inquiry](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

![image_12_20210604](https://static.toastoven.net/prod_dnsplus/image_12_20210604.png)

### Modify a GSLB

1. Selecting the GSLB to modify and click **Modify GSLB**.

2. Modify the **GSLB Name**, **Routing Rule**, **TTL (seconds)**, and **Status**, and click **Confirm**.

![image_13_20210604](https://static.toastoven.net/prod_dnsplus/image_13_20210604.png)

### Delete GSLBs

Selecting all the GSLBs to delete, click **Delete GSLB**, and click **Confirm** in the **Delete GSLB** dialog box.

![image_14_20210604](https://static.toastoven.net/prod_dnsplus/image_14_20210604.png)

### Connect a Pool

Configure a pool to connect to GSLB.

1. Click **Connect Pool**.

2. In the **Connect Pool** dialog box, select a pool to connect to and enter the setting information according to the routing rule.

    - Priority: The smaller the number, the higher the routing order. If the same priority as the previously connected pool is entered, the routing order of the existing pool is lowered. Set when the routing rule is FAILOVER or GEOLOCATION.
    - Region: Routes traffic of the configured region to the connected pool. Set when the routing rule is GEOLOCATION.
Example: If a pool is connected to GSLB as shown in the table below
        - In case of FAILOVER, it will be routed to Pool-A.
        - In case of GEOLOCATION, if the client is in the Northeast Asia region, it will be routed to Pool-B.
        - In case of GEOLOCATION, if the client is in the Western North America region, it is routed to Pool-C.
        - In case of GEOLOCATION, if the client is in the Western Europe region, it is routed to Pool-A by priority because there is no configure region.

    | Pool name | Priority | Region |
    | --- | --- | --- |
    | Pool-A | 1 |  |
    | Pool-B | 2 | Northeast Asia, Southeast Asia |
    | Pool-C | 3 | Western North America |

3. Click **Confirm**.

4. There is a limit to the maximum number of pools that can be connected. If you want to raise the limit, please contact us.

    - Contact: [1:1 Inquiry](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

![image_15_20210604](https://static.toastoven.net/prod_dnsplus/image_15_20210604.png)

### Modify Pool Connection

1. Select the pool to modify and click **Modify Pool Connection**.

2. **Pool** cannot be modified, but the configuration information can be modified according to the routing rule.

3. Click **Confirm**.

![image_16_20210604](https://static.toastoven.net/prod_dnsplus/image_16_20210604.png)

### Disconnect Pools

1. Select all pools to disconnect and click **Disconnect Pools**.

2. Click **Confirm**.

![image_17_20210604](https://static.toastoven.net/prod_dnsplus/image_17_20210604.png)

### Search GSLB and View Results

You can search for the GSLB that you want by name.

1. Enter a search term in the search bar on the upper right side and click **Search** or press **Enter**. All values including the search term are displayed as results.

2. If you enter a search term in the connected pool list, the search is performed within the current list.

    - All pools that contain the search term in **Pool Name**, **Priority**, and **Region** are searched and displayed.

3. The GSLB status is displayed according to the rules below, and the status of the [Pool](./console-guide/#health-check-api) is displayed for the connected pool.

| GSLB Status | GSLB Enabled/Disabled | Pool Status |
| --- | --- | --- |
| Normal (green) | Enabled | 1 or more pools in normal status |
| Error (red) | Enabled | No normal status and 1 or more pools in error status |
| Unknown (gray) | Enabled | All in disabled or unknown status,<br>A status where there is no connected pool |
| Disabled (Gray) | Disabled | Unrelated |

![image_18_20210604](https://static.toastoven.net/prod_dnsplus/image_18_20210604.png)

## Manage Pools and Endpoints

On the **GSLB** screen of the menu, you can manage pools and endpoints of the selected pools.

![image_19_20210604](https://static.toastoven.net/prod_dnsplus/image_19_20210604.png)

### Create a Pool

1. A pool is a component that groups endpoints, which is the smallest unit to which the routing rule is applied. Click **Create Pool** to create a pool.

2. Enter the **Pool** information.

    - Pool Name: Name of the pool to be created. Uppercase and lowercase letters, numbers, and '-' and '_' are allowed.
    - Status: Select whether to activate the pool.
    - Health Check: You can select a health check to check the accessibility of endpoints in the pool.

3. Enter the **Endpoint** information.

    - You can enter multiple endpoints. You can add an endpoint by clicking the **+** button on the right side of the screen, and remove an endpoint by clicking the **-** button of the added endpoint.
    - Endpoint Address: A domain address or IPv4 address can be entered, but a [reserved IP address](https://en.wikipedia.org/wiki/Reserved_IP_addresses) cannot be entered. Endpoint addresses cannot be duplicated within a pool.
    - Weight: 0~1.00 can be entered, and the value is applied relative to other endpoint weights in the pool. Equal weights have the same priority within the pool.
    - Status: Choose whether to activate the endpoint.
    
4. After completing the configuration, click **Confirm**.

5. There are limits to the maximum number of pools that can be created, the maximum number of endpoints in a pool, and the maximum total number of endpoints. If you want to raise the limits, please contact us. 

   - Contact: [1:1 Inquiry](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

![image_20_20210604](https://static.toastoven.net/prod_dnsplus/image_20_20210604.png)

### Modify a Pool

1. Select the pool to modify and click **Modify Pool**.

2. Modify **Pool Name**, **Status**, **Health Check**, and **Endpoint**.

3. After completing the configuration, click **Confirm**.

![image_21_20210604](https://static.toastoven.net/prod_dnsplus/image_21_20210604.png)

### Delete Pools

1. Select all pools to delete and click **Delete Pool**.

2. Click **Confirm**.

![image_22_20210604](https://static.toastoven.net/prod_dnsplus/image_22_20210604.png)

### Search Pools and View Results

1. Enter a search term in the pool list and click **Search** or press **Enter**. All values including the search term are displayed as results.

2. If you enter a search term in the endpoint list, the search is performed within the current list.

    - All endpoints that contain a search term in the **Endpoint Address** and **Weight** fields are searched and displayed.

3. The pool's status is displayed according to the rules below.

| Pool Status | Pool Enabled/Disabled | Health Check Settings | Endpoint Status |
| --- | --- | --- | --- |
| Normal (green) | Enabled | Connected | 1 or more pools in normal status |
| Error (red) | Enabled | Connected | No normal status and 1 or more pools in error status |
| Unknown (gray) | Enabled | Not connected<br>If it is connected, follow the endpoint state | All in disabled or unknown status |
| Disabled (Gray) | Disabled | Unrelated | Unrelated |

![image_23_20210604](https://static.toastoven.net/prod_dnsplus/image_23_20210604.png)


## Manage Health Checks

You can manage health checks from the **GSLB** screen in the menu.

![image_24_20210604](https://static.toastoven.net/prod_dnsplus/image_24_20210604.png)

### Create a Health Check

1. To create a health check, click **Create Health Check**. Depending on the configured value, you can check the accessibility of endpoints in the pool.

2. Enter the **Health Check** information.

    - **Health Check Name**: The name of the health check to be created. Uppercase and lowercase letters, numbers, '-' and '_' are allowed.
    - **Protocol**: The protocol to use when performing a health check, You can select HTTPS, HTTP, or TCP. The information you can enter depends on the protocol you choose.
        - **HTTPS Input Items**: No certificate verification, port, health check interval, maximum response latency (timeout), maximum retries, path, expected status code, expected response body, and request header
        - **HTTP Input Items**: Port, health check interval, maximum response latency (timeout), maximum retries, path, expected status code, expected response body, and request header
        - **TCP Input Items**: Port, health check interval, maximum response latency (timeout), maximum retry count
    - **Disable Certificate Validation**: If you select **Yes**, ignores the endpoint's TLS/SSL certificate being invalid when a health check is performed.
    - **Port**: Enter the port to be used when performing a health check.
    - **Health check interval**: Enter the health check interval. The unit is seconds, and a health check is attempted every specified cycle.
    - **Maximum Response latency (timeout)**: Enter the maximum number of seconds to wait for a healthy response after a health check. The units are seconds, and exceeding the specified wait time is considered a failure.
    - **Maximum Number of Retries**: Enter the maximum number of retries for the health check
    - **Path**: Enter the path to be used when performing a health check. The starting character must be a forward slash (/).
    - **Expected Status Code**: Enter the expected [HTTP Status Code](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) response of the health check. You can enter an 'x' character as a wildcard, and if a match is found, the endpoint is considered healthy. Example: 2xx, 20x, 200
    - **Expected Response Body**: You can enter the expected response body of the health check.
    - When determining **Expected Status Code** and **Expected Response Body**, pages redirected from the endpoint are not supported.
    - **Request Header**: You can enter headers that are set in the health check request.
    
3. After completing the configuration, click **Confirm**.

4. There is a limit to the maximum number of health checks that can be created. If you want to raise the limit, please contact us. 

   - Contact: [1:1 Inquiry](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

![image_25_20210604](https://static.toastoven.net/prod_dnsplus/image_25_20210604.png)

### Modify a Health Check

1. Select the health check to modify and click **Modify Health Check**.

2. Modify **Health Check Name**, **Protocol**, **Disable Certificate Validation**, **Port**, **Health Check Interval**, **Maximum Response latency (timeout)**, **Maximum Number of Retries**, **Path**, **Expected Status Code**, **Expected Response Body**, and **Request Header**.

3. After completing the configuration, click **Confirm**.

![image_26_20210604](https://static.toastoven.net/prod_dnsplus/image_26_20210604.png)

### Delete Health Checks

1. Select all health checks to delete and click **Delete Health Check**.

2. Click **Confirm**.

![image_27_20210604](https://static.toastoven.net/prod_dnsplus/image_27_20210604.png)

### Search Health Checks and Check Basic Information

1. Enter the health check name in the text box at the top right of the health check list and click **Search** or press **Enter**. All values including the search term are displayed as results.

2. If you select the item that you want from the searched health check list, you can check the **basic information** set in the health check.

![image_28_20210604](https://static.toastoven.net/prod_dnsplus/image_28_20210604.png)
