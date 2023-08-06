# aws-cwsyn-canary

## Synopsis
#### Pre-requisites: Ensure you already have the required [service role](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Synthetics_Canaries_Roles.html) created with the appropriate policy. An S3 bucket to store the screenshots and logs of the canary monitoring results and a KMS key that you can use to encrypt/decrypt (you can skip this step if no encryption is necessary).
A simple (YAML) Cloudformation template CloudWatch Synthetic Canary that monitors a web application simulating an end user login. The elements are verified via their XPaths. This synthetic canary performs the following checks:
1. Navigate to a URL (e.g. https://www.example.com)
2. Enter a username on the page
3. Enter the password on the page <br>
   **Note**: In this example, I encrypted/decrypted the password so that it's not displayed in PlainText format
5. Click the Sign-in button
6. Checks the following element(s): <br>
   **Note**: In this example, I have added a try/except logic from the selenium module `from selenium.common.exceptions import NoSuchElementException`
   - If **Element#1** exists on the page upon login, then click 'Done' button. If **Element#1** does not exist
   - Checks for **Element#2** <br>
This is optional; you skip this step if checking for a single element on the page. This logic is very useful if the page contains overlapping element (**e.g.** a maintenance page/banner that gets displayed periodically to notify users of an upcoming maintenance window; with the option to click off the banner within the page).
8. On the page, navigate to the Sign-out menu
9. Click on Sign-out button <br>

I'm passing the URL, CanaryUser and CanaryPassword as Environment Variables; this is useful when you're deploying the canary to various environments (e.g. dev, stage, production) where they have varying URLs, usernames & passwords(involving encryption/decryption). 
If all steps were executed without any error, this completes the test/monitor successfully.
