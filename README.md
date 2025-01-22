# 30 Days DevOps Challenge - NBADataLake


## **Project Overview**
This repository contains the setup_nba_data_lake.py script, which automates the creation of a data lake for NBA analytics using AWS services. The script integrates Amazon S3, AWS Glue, and Amazon Athena, and sets up the infrastructure needed to store and query NBA-related data. This project demonstrates principles and leverages technologies such as:

- **External API Integration** (SportsData.io API)
- **Cloud Storage** (AWS S3)
- **Data Lake Architecture** (AWS Glue)
- **Serverless Analytics** (Amazon Athena)
- **Infrastructure as Code** (AWS CloudFormation or Terraform)
- **Version Control** (Git)
- **Python Development**
- **Error Handling**
- **Environment Management** (dotenv, AWS CLI)
- **Data Serialization** (JSON and Line-Delimited JSON)


## **Features**
- Creates an Amazon S3 bucket (`sports-analytics-data-lake`) to store raw and processed NBA data.
- Fetches NBA player data from SportsData.io API and stores it in the S3 bucket in line-delimited JSON format.
- Uses AWS Glue to create a database (`glue_nba_data_lake`) and an external table for structured querying of the data.
- Configures Amazon Athena for serverless querying of data stored in S3, enabling SQL-based analytics.
- Automates the ETL process by integrating external data sources, transforming data formats, and storing results in S3 for analysis.

## **Prerequisites**
Before running the script, ensure you have the following:

1. Go to Sportsdata.io and create a free account
2. At the top left, you should see "Developers", if you hover over it you should see "API Resources"
3. Click on "Introduction & Testing"

4. Click on "SportsDataIO API Free Trial" and fill out the information & be sure to select NBA for this tutorial

5. You will get an email and at the bottom it says "Launch Developer Portal"

6. By default it takes you to the NFL, on the left click on NBA
7. Scroll down until you see "Standings"
8. You'll see "Query String Parameters", the value in the drop down box is your API key. 
9. Copy this string because you will need to paste it later in the script

**IAM Role/Permissions | Ensure the user or role running the script has the following permissions:**

- **S3**: ```s3:CreateBucket, s3:PutObject, s3:DeleteBucket, s3:ListBucket```
- **Athena**: ```athena:StartQueryExecution, athena:GetQueryResults```
- **Glue**: ```glue:CreateDatabase, glue:CreateTable, glue:DeleteDatabase, glue:DeleteTable```

AWS Glue is a fully managed ETL (extract, transform, load) service that allows you to easily move data between different data sources and targets. The typical workflow in AWS Glue involves:

1. Define data sources and targets in the Data Catalog.

2. Use Crawlers to populate the Data Catalog with table metadata from data sources.

3. Define ETL jobs with transformation scripts to move and process data.

4. Run jobs on-demand or based on triggers.

5. Monitor job performance using dashboards.

The following diagram shows the architecture of an AWS Glue environment.
![AWS Glue System Diagram](https://github.com/joesghub/sports-data-lake/blob/main/screenshots/aws-glue.png?raw=true)



## **Technical Architecture**
![Gameday notification system diagram](https://github.com/joesghub/sports-data-lake/blob/main/screenshots/sports%20data%20lake%20system%20diagram.png?raw=true)


## **Project Structure**
```
sports-data-lake/
├── src/
│   ├── setup_nba_data_lake.py       # Main function code
├── policies/
│   ├── IAM-Role.json                # S3, Glue, Athena Permissions
├── .gitignore
└── README.md                        # Project documentation
```

## **Setup Instructions**
This journey outlines the step-by-step process of setting up a cloud-based sports analytics data lake using AWS services, including S3, IAM, and Athena. From initializing the environment to dynamically generating S3 bucket names and securely managing credentials, the project demonstrates a structured approach to cloud infrastructure setup and data querying. It highlights key cloud DevOps practices such as automation, security, and scalability, providing a robust foundation for managing and analyzing sports data.

**Impact from a Cloud DevOps Perspective**

- **Scalability**  
  - Dynamically generated S3 bucket names ensure unique and conflict-free resource allocation, supporting future growth and multi-tenant environments.  
  - Amazon Athena allows querying large datasets efficiently without provisioning dedicated compute resources.

- **Automation**  
  - The setup script automates the environment creation, S3 bucket setup, and data ingestion, minimizing manual intervention.  
  - IAM roles and policies streamline secure access management for AWS resources, aligning permissions with resource requirements.

- **Security and Best Practices**  
  - The `.gitignore` implementation ensures sensitive `.env` files remain local, mitigating the risk of exposed credentials.  
  - Adherence to the principle of least privilege in IAM policies restricts permissions to only necessary resources, enhancing security.  
  - Use of IMDSv2 for EC2 metadata access reduces attack vectors for unauthorized access.

- **Cost Efficiency**  
  - Leveraging serverless services like Athena and S3's pay-as-you-go model keeps operational costs low while offering high reliability and availability.  
  - Eliminating over-provisioning of resources ensures optimal utilization and budget-friendly scaling.

- **Resilience and Monitoring**  
  - Integration of IAM roles with EC2 ensures secure and uninterrupted operation of scripts.  
  - Use of metadata queries validates the role assignment and avoids runtime permission errors.  
  - Amazon S3 provides versioning and lifecycle management options for data integrity and long-term storage resilience.

---

This project effectively demonstrates the integration of DevOps principles into the design and deployment of a scalable, secure, and cost-effective sports analytics data platform. It sets the stage for future customization, advanced data analytics, and seamless integration of additional data sources.

### **Setup your environment and clone the repository**

1. **Update environment** 
```
sudo apt update
```

2. **Clone Repo**
```
git clone https://github.com/alahl1/NBADataLake.git
```

3. **Create Blank Repo**
```
https://github.com/joesghub/sports-data-lake.git
```

4. **Update remote origin**
```
git remote rename origin upstream
git remote add origin https://github.com/joesghub/sports-data-lake.git
```

5. **Push Cloned Repo**
```
git push -u origin main
```

6. **Create .gitignore and remove .env file from Git index**

	When I cloned the repository the .env file was pushed before I created a .gitignore file. The problem is that I need to put sensitive keys in the .env file locally without them getting pushed to Github. Currently, if I put my credentials in the .env file, they will be uploaded to the web creating a security risk for the application. 
    
	To remedy this I created a .gitignore file, pushed it to GH, removed the .env file from Git's local index, committed and pushed my changes. Now I can work on the .env file locally without worrying about my keys or credentials being exposed to the Internet! 
	
```
cd <you-local-repository>
echo "src/.env" >> .gitignore
```
```
git rm --cached src/.env
```
```
git commit -m 'Remove src/.env file from repository, keep locally'
```
```
git push
```

7. **Install AWS CLI**

```bash

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

sudo apt install -y unzip

unzip awscliv2.zip

sudo ./aws/install

aws --version
```

8. **Configure AWS CLI**

```
aws configure
```

9. **Install Python Venv and Create Requirements.txt**

To proceed, I needed to download python3 venv, create the environment, activate it, and install my requirements

```
sudo apt install -y python3-venv

python3 -m venv venv

source venv/bin/activate 

pip install boto3 requests python-dotenv

pip freeze > requirements.txt
```

![Pip Freeze in Venv](https://github.com/joesghub/sports-data-lake/blob/main/screenshots/pip%20freeze%20in%20venv.png?raw=true)
    
### **Dynamically Generate Globally Unique S3 Bucket Name**

In generating the unique bucket name, I also need to update the policy script. The policy grants permissions for the EC2 based on specific resource names. In our case the policy needs to be able to accept the names we generate otherwise the permissions will not be applied properly and our application will fail.  Otherwise, I would need to grant more permissions than necessary to account for the bucket name that isn't created yet. That approach is not as secure, so I updated the policy accordingly!

```python
prefix = 'sports-analytics-data-lake' 
random_suffix = ''.join(random.choices(string.ascii_lowercase + string.digits, k=6)) 
#Dynamically generate a unique bucket name
bucket_name = f"{prefix}-{random_suffix}"
```
- A prefix (sports-analytics-data-lake) is defined for the bucket name.
- A random 6-character alphanumeric suffix is appended to create a unique bucket name dynamically.
- **Result**: The bucket name takes the form ```sports-analytics-data-lake-*```, ensuring uniqueness to avoid name conflicts.

```python
S3 Bucket Policy
"Resource": [
"arn:aws:s3:::sports-analytics-data-lake-*",
"arn:aws:s3:::sports-analytics-data-lake-*/*"]

Athena Policy S3 Bucket Resource 
"Resource": [
"arn:aws:s3:::sports-analytics-data-lake/athena-results/*",
"arn:aws:s3:::sports-analytics-data-lake"]
```
- The S3 bucket policy is defined to grant access to all buckets matching the prefix sports-analytics-data-lake-* and their objects (/*).
- **Result**: Enables controlled access to all dynamically created buckets and their contents.


- ```sports-analytics-data-lake/athena-results/*```: Grants access to a specific folder for query results.
- ```sports-analytics-data-lake```: Grants access to the parent bucket.
- **Result**: Restricts Athena's permissions to specific resources required for operations.

        

### **Create IAM Policy for EC2**

Now that policy is updated we can add it to custom policy we create in AWS IAM. 

1. Open the IAM service in the AWS Management Console.
2. Navigate to Policies → Create Policy.
3. Click JSON and paste the JSON policy from IAM_Role.json file
5. Click Next: Tags (you can skip adding tags).
6. Click Next: Review.
7. Enter a name for the policy (e.g., sports_data_lake_policy).
8. Review and click Create Policy.

![Creating policy for ec2 role](https://github.com/joesghub/sports-data-lake/blob/main/screenshots/iam%20role%20policy.png?raw=true)

### **Create an IAM Role for EC2**

1. Open the IAM service in the AWS Management Console.
2. Click Roles → Create Role.
3. Select AWS Service and choose EC2.
4. Attach the custom policy we created.
5. Click Next: Tags (you can skip adding tags).
6. Click Next: Review.
7. Enter a name for the role (e.g., sports_data_ec2_role).
8. Review and click Create Role.

![Creating role for ec2 and attaching policy](https://github.com/joesghub/sports-data-lake/blob/main/screenshots/iam%20role%20ec2.png?raw=true)

### **Create an IAM Role for EC2**

I ran into some issues with the difference between IAM Users and Roles. Below is a quick overview of the two and their use cases before we discuss further:

**IAM User**

*Purpose*: Represents a person or an application that interacts directly with AWS services.

*Key Characteristics*:
- Has long-term access keys and secret keys for authentication.
- Used to configure the AWS CLI or SDKs for programmatic access.
- Permissions are directly attached via IAM policies.

**IAM Role**

*Purpose*: Intended for temporary access by AWS services or entities, like an EC2 instance.

*Key Characteristics*:

- Does not have permanent credentials.
- Requires an entity (like an EC2 instance) to "assume" the role.
- Permissions are scoped to the role's policies.

Initially I created an IAM User. I did not attach our custom policy to the user's policies. The user's policies govern what the AWS CLI can do. Then I configured my AWS CLI on my EC2 with the IAM User's keys. The script was using the IAM User's credentials (configured in the AWS CLI). Since the policy was not attached to the user, the script lacked permissions and failed to run.

Upon realization, I looked further into the use cases for roles vs users because a role seemed more applicable to my use case. Now I needed to backtrack and make some changes so my policies, roles, and access all aligned. My first change was to remove the IAM User as the credentials for the CLI. Then, I created an IAM Role for an EC2. Next, I attached the policy to the role. After, I went to my EC2 console and attached the IAM Role. 

- Go to the AWS EC2 Console.
- Select your EC2 instance.
- Choose Actions > Security > Modify IAM Role.
- Attach the appropriate IAM role.

Finally I was able to use metadata queries to confirm the EC2 was using the intended role. By default, AWS now enforces IMDSv2, which requires tokens for metadata queries. I used the following commands to test with IMDSv2:

``` bash

 Get a Token: TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    56  100    56    0     0  17948      0 --:--:-- --:--:-- --:--:-- 18666
Query IAM Role Information: curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/iam/info
{
  "Code" : "Success",
  "LastUpdated" : "2025-01-20T12:12:51Z",
  "InstanceProfileArn" : "arn:aws:iam::494141675542:instance-profile/sports_lake_ec2",
  "InstanceProfileId" : "AIPAXGDJDTALE5EWZRRGY"
  }
```
### **Update Environment Variables**

1. In the CLI (Command Line Interface), type
```bash
nano .env
```
2. Paste the following line of code into your file, ensure you swap out with your API key
```bash
SPORTS_DATA_API_KEY=your_sportsdata_api_key
NBA_ENDPOINT=https://api.sportsdata.io/v3/nba/scores/json/Players
```

3. Press ^X to exit, press Y to save the file, press enter to confirm the file name 

### **Run the Script**
1. In the CLI type
```bash
python3 setup_nba_data_lake.py
```
- You should see the resources were successfully created, the sample data was uploaded successfully and the Data Lake Setup Completed

![Application Successfully Run](https://github.com/joesghub/sports-data-lake/blob/main/screenshots/succesful%20script%20except%20athena%20location.png?raw=true)

### **Check S3 Bucket for Data**
1. In the Search Bar, type S3 and click blue hyper link name

- You should see 2 General purpose bucket named "Sports-analytics-data-lake"
- When you click the bucket name you will see 3 objects are in the bucket

2. Click on raw-data and you will see it contains "nba_player_data.json"

3. Click the file name and at the top you will see the option to Open the file

4. You'll see a long string of various NBA data:
![JSON Bucket Data](https://github.com/joesghub/sports-data-lake/blob/main/screenshots/player%20json%20data.png?raw=true)



### **Query Data with Amazon Athena**
1. Head over to Amazon Athena and you could paste the following sample query:
```bash
SELECT FirstName, LastName, Position, Team
FROM nba_players
WHERE Position = 'PG';

```
2. Click Run
3. You should see an output if you scroll down under "Query Results"


You may need to configure the Athena query result location manually if it's not set. You can do this via the AWS Console
After I was able to query the table but the data was limited. 

The original script only created columns for name, team, position, and points. I wanted to do some cool queries based on points but the column was empty. 

First I manually edited the Table Schema in the AWS Glue console. Based on the existing schema, I looked at the actual JSON data in the S3 bucket to confirm the columns pulled. By looking at the JSON you can see many other data fields. Learning this I added the other data fields to the schema in the same synatx as the previous fields. 

![Updated Schema](https://github.com/joesghub/sports-data-lake/blob/main/screenshots/updated%20table%20schema.png?raw=true)

Then I was able to view more columns in my AWS Athena query editor. Finally I updated the application code to match the working schema.


![Expanded Player Data](https://github.com/joesghub/sports-data-lake/blob/main/screenshots/player%20data%20explanded.png?raw=true)

## Data Analysis

```
SELECT firstname, lastname, team, birthcity, birthstate, birthcountry, salary, position
FROM nba_players
ORDER BY salary desc, team
limit 20;

#	firstname	lastname	team	birthcity	birthstate	birthcountry	salary	position
1	Stephen	Curry	GS	Akron	OH	USA	55761216	PG
2	Nikola	Jokic	DEN	Sombor		Serbia	51415938	C
3	Joel	Embiid	PHI	Yaounde		Cameroon	51415938	C
4	Kevin	Durant	PHO	Washington	D.C.	USA	51179021	PF
5	Bradley	Beal	PHO	St. Louis	MO	USA	50203930	SG
6	Kawhi	Leonard	LAC	Riverside	CA	USA	49350000	SF
7	Jaylen	Brown	BOS	Marietta	GA	USA	49205800	SG
8	Karl-Anthony	Towns	NY	Piscataway	NJ	USA	49205800	PF
9	Paul	George	PHI	Palmdale	CA	USA	49205800	SF
10	Devin	Booker	PHO	Grand Rapids	MI	USA	49205800	SG
11	Jimmy	Butler	MIA	Houston	TX	USA	48798677	SF
12	Giannis	Antetokounmpo	MIL	Athens		Greece	48787676	PF
13	Damian	Lillard	MIL	Oakland	CA	USA	48787676	PG
14	LeBron	James	LAL	Akron	OH	USA	48728845	SF
15	Rudy	Gobert	MIN	Saint-Quentin		France	43827587	C
16	Anthony	Davis	LAL	Chicago	IL	USA	43219440	C
17	Trae	Young	ATL	Lubbock	TX	USA	43031940	PG
18	Zach	LaVine	CHI	Renton	WA	USA	43031940	SG
19	Luka	Doncic	DAL	Ljubljana		Slovenia	43031940	PG
20	Fred	VanVleet	HOU	Rockford	IL	USA	42846615	PG
```

![Top 20 Player Data](https://github.com/joesghub/sports-data-lake/blob/main/screenshots/top%2020%20salary%20players.png?raw=true)

**Teams with Multiple Top Paid Players**:

Phoenix Suns, Los Angeles Lakers, and Philadelphia 76ers are the teams with the most players on the top 20 highest-paid list, which indicates their heavy investments in building star-studded rosters.
Other teams like the Milwaukee Bucks and Los Angeles Clippers also have multiple top-paid players, reflecting their title aspirations.

**Competitive Teams**:

Many of the top-paid players are on teams with championship aspirations (e.g., Suns, Lakers, Bucks, Sixers), reflecting how salary distribution often correlates with success in the league.

**Geographical Trends**:

There’s a clear concentration of high-paid players on teams located in larger, more prominent markets, such as Los Angeles, Phoenix, and Philadelphia. These teams are able to afford such high salaries, partly due to larger revenue and fanbases.
Milwaukee is somewhat of an outlier with Giannis and Lillard, showing that teams can still build around one or two stars, even in smaller markets.

**Western Conference Teams**:

- Phoenix Suns (PHO) has 3 players: Kevin Durant, Bradley Beal, and Devin Booker.
- Los Angeles Clippers (LAC) and Los Angeles Lakers (LAL) have 4 players combined: Kawhi Leonard, LeBron James, and Anthony Davis (LAL); and Paul George (LAC).
- Golden State Warriors (GS) has 1 player: Stephen Curry.
- Denver Nuggets (DEN) has 1 player: Nikola Jokic.
- Dallas Mavericks (DAL) has 1 player: Luka Doncic.
- Houston Rockets (HOU) has 1 player: Fred VanVleet.

**Eastern Conference Teams**:

- Philadelphia 76ers (PHI) has 2 players: Joel Embiid and Paul George.
- Miami Heat (MIA) has 1 player: Jimmy Butler.
- Boston Celtics (BOS) has 1 player: Jaylen Brown.
- Milwaukee Bucks (MIL) has 2 players: Giannis Antetokounmpo and Damian Lillard.
- Minnesota Timberwolves (MIN) has 1 player: Rudy Gobert.
- Chicago Bulls (CHI) has 1 player: Zach LaVine.
- Atlanta Hawks (ATL) has 1 player: Trae Young.

**Birth City Distribution**

California and Texas appear as common birth states for players with high salaries. These states have large basketball talent pools and strong developmental programs, which could explain the presence of top earners like Kawhi Leonard, Paul George, Damian Lillard, Jimmy Butler, and Trae Young.

Ohio also contributes two top-paid players (LeBron James and Stephen Curry), which could indicate the importance of the state's influence on talent development, particularly with strong high school and college basketball programs in the region.

Chicago, New Jersey, and Michigan are other birth cities that are home to players on the top 20 list, suggesting that the Midwest also has strong basketball talent.

U.S. states like California, Ohio, Texas, and Illinois are strong contributors to the top-paid players, suggesting that regions with strong youth development programs and professional team infrastructure are more likely to produce high-earning athletes.



## **What We Learned**  
1. Securely managing credentials and sensitive data with `.gitignore` and IAM best practices.  
2. Applying the principle of least privilege in IAM policies to limit resource access effectively.  
3. Automating infrastructure setup, including S3 bucket creation and data ingestion, with Python scripts.  
4. Dynamically generating globally unique S3 bucket names to avoid naming conflicts in multi-tenant environments.  
5. Efficiently querying structured data stored in S3 with Amazon Athena for data analysis.  
6. Configuring EC2 instances with IAM roles for seamless and secure interaction with AWS services.  
7. Utilizing Amazon S3 as a scalable storage solution for raw and processed data, ensuring data organization and accessibility.


## **Future Enhancements**  
1. Automate periodic data ingestion from external APIs using AWS Lambda and EventBridge.  
2. Introduce a data transformation and cleaning layer with AWS Glue ETL for improved data quality.  
3. Implement fine-grained access control to S3 buckets and objects for enhanced security and compliance.  
4. Incorporate advanced analytics and data visualizations using AWS QuickSight to provide actionable insights.  
5. Leverage Amazon Athena's federated query capabilities to integrate and analyze data from multiple sources.  
6. Add monitoring and logging mechanisms with Amazon CloudWatch for improved system reliability and issue detection.  
7. Explore scalable machine learning models using Amazon SageMaker for predictive analytics on sports data.  
