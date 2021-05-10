# eGain-address-book-import-utility

This example deals with the architecture involved in importing contacts from an external address book (CSV) for a customer into eGain, such that agent would get contact suggestions while looking for a contact in eGain Advisor desktop.

# Architecture Diagram
![address-book-architecture](https://user-images.githubusercontent.com/18159903/117599809-7621ae80-b0ff-11eb-9bc7-5421dcdd9c30.png)

# Architecture Overview
When a CSV file (refer to address-book.csv for the structure) is uploaded to the created s3 bucket, then an object create event is generated which in turn is being sent to the lambda for processing. This lambda will process the contacts in the CSV to JSON and call the eGain login API to authenticate and in turn calls eGain create individual customer API to create the entries in the eGain database.

# Pre-requisites

1. Git installed on your system
2. AWS account
3. aws-cli installed on your system
4. sam-cli installed on your system
5. npm installed on your system
6. eGain system with valid agent credentials

# Installation Steps:

1. Go to your terminal and do git clone https://github.com/naveenmano/eGain-address-book.git
2. Move to the root of the cloned repository in your system and do npm install
3. Fetch the eGain server host name and the agent credentials for your system
4. Add these details into the config.js inside the src folder and save it
5. Go to your AWS account and select the region where you want to deploy this app
6. In that region create a S3 bucket. This bucket is for deployment. (If you already have a bucket you can omit this step)
7. Go back to the terminal and move to the root of the cloned repository
8. Run the below command which will generate serverless-output.yml file

sam package --template-file template.yml --output-template-file serverless-output.yml --s3-bucket <Pass the deployment bucket which you created in Step 6> --profile <This is you AWS profile which you have configured. If you have only one profile then pass "default" or the name of the profile>

9. Run the below command to deploy the app to AWS

sam deploy --s3-bucket <deployment bucket> --template-file serverless-output.yml --stack-name import-address-book --capabilities CAPABILITY_IAM CAPABILITY_AUTO_EXPAND --profile <AWS profile> --region <AWS region>

10. Open the address-book.csv from the cloned repository and add the contact details by replacing the example contacts already present in the file and save it
11. Go to AWS account and navigate to S3 and search for a bucket named "import-address-book"
12. Upload the address-book.csv into that bucket. Once the upload is complete, then all the contacts should be imported in to your eGain system

# Verification

The below steps helps to verify if the email has been added for the customers in the csv
1. Login to eGain advisor desktop
2. Click on New from the Chat window
3. Select Outbound Email for New Case
4. Now when you type the email on the pop up field, you should be seeing suggestions of all the emails which matches the text

# Note

1. This sample app is only for creating contacts
2. Always replace the contacts from address-book.csv with new contacts and upload it to s3
3. Duplicate contacts will not created
4. This address-book.csv is just an example structure. You can add lot more fields while creating contacts. A complete set of available attributes is available in the requestBody.json file
5. When you are planning to add more fields to contacts, then add those fields to the address-book.csv and also modify the requestBody present inside the processor.js file and deploy the updated package to AWS




