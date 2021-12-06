<img src="https://welastic.pl/wp-content/uploads/2020/05/cropped-welastic_logo-300x259.png" alt="Welastic logo" width="100" align="left">
<br><br>
<br><br>
<br><br>

## Task 1: Generate key for User
In this task you download keys for your user

1. In the AWS Management Console, on the **Services** menu, click **IAM**.
2. In the navigation pane on the left, click **Users**.
3. Click on your username.

This will bring to a summary page for user. The **Permissions** tab will be displayed.

4. Review your permissions.
5. Click the **Security credentials** tab.

6. Click in section **Access keys** the **Create access key**

When new window appear, click **Show** and write down you Access key ID and Secret access key

7. Click **Close**

<h2 id="step4">Task 2 - Configure AWS CLI</h2>

<p>In this task, you will configure the AWS CLI in the Cloud9 console. In the subsequent tasks in this lab, you will gather information about the application stack using various AWS CLI commands.</p>
<ol start="1">
</ol><pre class="highlight plaintext"><code>aws configure&#x000A;</code></pre>

Put **AWS Access Key ID** and **AWS Secret Access Key**

as default region set **eu-west-1**


<h2 id="step5">Task 2 - Gather Information About the Instances in the Application Stack</h2>
Turn off the Cloud9 temporary IAM credentials.
![iam off](img/iam_credentials_off.png)
<p>In this task, you will query the application stack using the AWS CLI to find out information about the Amazon EC2 instances being used in the application stack.</p>
<ol start="1">
<li>Query the AWS account provided for you in this lab for all the Amazon EC2 instances that are available by using the command below:</li>
</ol><pre class="highlight plaintext"><code>aws ec2 describe-instances&#x000A;</code></pre><ol start="2">
<li>Determine how many instances are deployed by using the <input readonly class="copyable-inline-input" size="7" type="text" value="--query"> command. To pull out only the instance IDs, you will use the following command: <input readonly class="copyable-inline-input" size="47" type="text" value="--query 'Reservations[].Instances[].InstanceId'">. Execute the command below:</li>
</ol><pre class="highlight plaintext"><code>aws ec2 describe-instances --query 'Reservations[].Instances[].InstanceId'&#x000A;</code></pre>
<p><strong>Example Output</strong></p>
<pre class="highlight plaintext"><code> [&#x000A;    "i-04c5ce1d34f815527"&#x000A; ]&#x000A;</code></pre>
<p>You can see that there all instances deployed in the application stack.</p>
<ol start="3">
<li>Now, determine if the previous admin tagged everything appropriately. Execute the command below. Explore the output of this query to learn more about the application environment.</li>
</ol><pre class="highlight plaintext"><code>aws ec2 describe-instances --query 'Reservations[].Instances[].[InstanceId,Tags]'&#x000A;</code></pre>
<p>Here you can determine a range of information about the environment. There is a tag with the <strong>Name</strong> of the instance.</p>

<p><strong>Example Output</strong></p>
<pre class="highlight plaintext"><code>{&#x000A;   "Value": "jaroslaw@welastic.pl",&#x000A;   "Key": "Creator"&#x000A;}&#x000A;</code></pre>
<ol start="4">
<li>Format the output into a table with <input readonly class="copyable-inline-input" size="14" type="text" value="--output table"> as an alternative to JSON. Execute the command below:</li>
</ol><pre class="highlight plaintext"><code>aws ec2 describe-instances --query 'Reservations[].Instances[].[InstanceId, Tags[?Key==`Name`].Value | [0], IamInstanceProfile.Arn]' --output table&#x000A;</code></pre><ol start="5">
<li>Filter out the Command Host, with the addition of a <input readonly class="copyable-inline-input" size="8" type="text" value="--filter">. In this case, use <input readonly class="copyable-inline-input" size="39" type="text" value='--filter "Name=tag:Name,Values=wetest*"'>. For the command <input readonly class="copyable-inline-input" size="22" type="text" value="ec2 describe-instances">, there is a list of filters that can be passed. These are described in the AWS CLI documentation -  <a href="https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-instances.html#options" target="_blank">https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-instances.html#options</a>
</li>
</ol>
<p>Execute the command below:</p>
<pre class="highlight plaintext"><code>aws ec2 describe-instances --filter "Name=tag:Name,Values=wetest*" --query 'Reservations[].Instances[].[InstanceId, Tags[?Key==`Name`].Value | [0], IamInstanceProfile.Arn]' --output table&#x000A;</code></pre>
<p>More information on manipulating the output can be found here: <a href="https://docs.aws.amazon.com/cli/latest/userguide/controlling-output.html" target="_blank">https://docs.aws.amazon.com/cli/latest/userguide/controlling-output.html</a><br>
and on using filtering here: <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Filtering.html" target="_blank">https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Filtering.html</a></p>


<h2 id="step7">Task 3 - Determine the Deployment Method</h2>

<p>In this task you will query the AWS deployment services to check if any are being used in the environment. Let's start with <strong>AWS Elastic Beanstalk</strong>, and then check if <strong>AWS OpsWorks</strong> or <strong>AWS CodeDeploy</strong> is being used.</p>

<p>AWS CLI documentation for each service can be found here:</p>
<ul>
<li>AWS Elastic Beanstalk - <a href="http://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/index.html" target="_blank">http://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/index.html</a>
</li>
<li>AWS OpsWorks -          <a href="http://docs.aws.amazon.com/cli/latest/reference/opsworks/index.html#available-commands" target="_blank">http://docs.aws.amazon.com/cli/latest/reference/opsworks/index.html#available-commands</a>
</li>
<li>AWS CodeDeploy -        <a href="https://docs.aws.amazon.com/cli/latest/reference/deploy/index.html#cli-aws-deploy" target="_blank">https://docs.aws.amazon.com/cli/latest/reference/deploy/index.html#cli-aws-deploy</a>
</li>
</ul><ol start="2">
<li>Explore the AWS CLI help menu and AWS Elastic Beanstalk with the <input readonly class="copyable-inline-input" size="4" type="text" value="help"> command. Execute the command below:</li>
</ol><pre class="highlight plaintext"><code>aws elasticbeanstalk help&#x000A;</code></pre>
<p>Once executed, take note of the <input readonly class="copyable-inline-input" size="9" type="text" value="describe-"> commands.</p>

<p><strong>Note</strong>: For those unfamiliar with working in a Linux shell, type <input readonly class="copyable-inline-input" size="1" type="text" value="q"> to drop out of the man pages.</p>
<ol start="3">
<li>You can bring up <input readonly class="copyable-inline-input" size="4" type="text" value="help"> on any command. Let's bring up the <input readonly class="copyable-inline-input" size="4" type="text" value="help"> section of the top level entity in AWS ElasticBeanstalk, the application. Execute the command below:</li>
</ol><pre class="highlight plaintext"><code>aws elasticbeanstalk describe-applications help&#x000A;</code></pre><ol start="4">
<li>Now let's make a call to see if AWS Elastic Beanstalk is being used in this environment. Execute the command below:</li>
</ol><pre class="highlight plaintext"><code>aws elasticbeanstalk describe-applications&#x000A;</code></pre>
<p>Once executed, an empty <input readonly class="copyable-inline-input" size="17" type="text" value="Applications: [ ]"> array is returned indicating that AWS Elastic Beanstalk is not being used as a deployment service.</p>

<p><strong>Example Output</strong></p>
<pre class="highlight plaintext"><code>{&#x000A;       "Applications": []&#x000A;}&#x000A;</code></pre>
<ol start="5">
<li>Now that you have determined the correct command, let's see if AWS OpsWorks is being used in this AWS account. Execute the command below:</li>
</ol><pre class="highlight plaintext"><code>aws opsworks describe-stacks&#x000A;</code></pre>
<p>Once executed, an empty <strong>Stacks: [ ]</strong> array is returned indicating that AWS OpsWorks is not being used as a deployment service.</p>

<p><strong>Example Output</strong></p>
<pre class="highlight plaintext"><code>{&#x000A;        "Stacks": []&#x000A;}&#x000A;</code></pre><ol start="6">
<li>Let's dive into <strong>AWS CodeDeploy</strong> to see if it is being used in our environment. AWS CodeDeploy is a deployment service that automates application deployments to Amazon EC2 instances or on-premise instances running in your own facility.</li>
</ol>
<p>Check for top level entities such as <strong>Applications</strong> of the AWS CodeDeploy service by executing the command below:</p>
<pre class="highlight plaintext"><code>aws deploy list-applications&#x000A;</code></pre>
<p>Once executed, you will note that there are three applications managed by AWS CodeDeploy.</p>

<p><strong>Example Output</strong></p>
<pre class="highlight plaintext"><code>{&#x000A;        "applications": [&#x000A;            "LABS-IAM-LAB-BASIC",&#x000A;            "GOV-CHAOS-MONKEY"&#x000A;        ]&#x000A;}&#x000A;</code></pre><ol start="7">
<li>Next, determine if the <strong>applications</strong> are related to any of our instances. To do this, query for the list of deployments. Execute the command below:</li>
</ol><pre class="highlight plaintext"><code>aws deploy list-deployments&#x000A;</code></pre>
<p>Once executed you will note that there are three deployments, each with a unique identifier.</p>

<p><strong>Example Output</strong></p>
<pre class="highlight plaintext"><code>{&#x000A;        "deployments": [&#x000A;            "d-sdasdAsON",&#x000A;            "d-AK4AAR7ON",&#x000A;            "d-0NRED6WNN"&#x000A;        ]&#x000A;}&#x000A;</code></pre>

<h2 id="step11">CheatSheet - Command Dictionary</h2>

<p>Below is a cheat-sheet containing all of the commands you executed in this lab:</p>
<pre class="highlight plaintext"><code>aws ec2 describe-instances&#x000A;aws ec2 describe-instances --query 'Reservations[].Instances[].InstanceId'&#x000A;aws ec2 describe-instances --query 'Reservations[].Instances[].[InstanceId,Tags]'&#x000A;aws ec2 describe-instances --query 'Reservations[].Instances[].[InstanceId, Tags[?Key==`Name`].Value, IamInstanceProfile.Arn]'&#x000A;aws ec2 describe-instances --query 'Reservations[].Instances[].[InstanceId, Tags[?Key==`Name`].Value | [0], IamInstanceProfile.Arn]'&#x000A;aws ec2 describe-instances --query 'Reservations[].Instances[].[InstanceId, Tags[?Key==`Name`].Value | [0], IamInstanceProfile.Arn]' --output table&#x000A;aws ec2 describe-instances --filter "Name=tag:Name,Values=MadLib*" --query 'Reservations[].Instances[].[InstanceId, Tags[?Key==`Name`].Value | [0], IamInstanceProfile.Arn]' --output table&#x000A;appROLEARN=$(aws ec2 describe-instances --filter "Name=tag:Name,Values=MadLib Web*" --query 'Reservations[0].Instances[0].IamInstanceProfile.Arn' --output text)&#x000A;aws iam list-instance-profiles --query "InstanceProfiles[?Arn=='$appROLEARN'].Roles[0].RoleName"&#x000A;appROLENAME=$(aws iam list-instance-profiles --query "InstanceProfiles[?Arn=='$appROLEARN'].Roles[0].RoleName" --output text)&#x000A;aws iam list-role-policies --role-name $appROLENAME&#x000A;appPOLNAME=$(aws iam list-role-policies --role-name $appROLENAME --query PolicyNames[] --output text)&#x000A;aws iam get-role-policy --role-name $appROLENAME --policy-name $appPOLNAME&#x000A;apiROLEARN=$(aws ec2 describe-instances --filter "Name=tag:Name,Values=MadLib API*" --query 'Reservations[0].Instances[0].IamInstanceProfile.Arn' --output text)&#x000A;apiROLENAME=$(aws iam list-instance-profiles --query "InstanceProfiles[?Arn=='$apiROLEARN'].Roles[0].RoleName" --output text)&#x000A;apiPOLNAME=$(aws iam list-role-policies --role-name $apiROLENAME --query PolicyNames[] --output text)&#x000A;aws iam get-role-policy --role-name $apiROLENAME --policy-name $apiPOLNAME&#x000A;saveROLEARN=$(aws ec2 describe-instances --filter "Name=tag:Name,Values=MadLib Save*" --query 'Reservations[0].Instances[0].IamInstanceProfile.Arn' --output text)&#x000A;saveROLENAME=$(aws iam list-instance-profiles --query "InstanceProfiles[?Arn=='$saveROLEARN'].Roles[0].RoleName" --output text)&#x000A;savePOLNAME=$(aws iam list-role-policies --role-name $saveROLENAME --query PolicyNames[] --output text)&#x000A;aws iam get-role-policy --role-name $saveROLENAME --policy-name $savePOLNAME&#x000A;aws elasticbeanstalk describe-applications&#x000A;aws opsworks describe-stacks&#x000A;aws deploy list-applications&#x000A;aws deploy list-deployments&#x000A;DEPLOYARRAY=$(aws deploy list-deployments --output text)&#x000A;IFS=' ' read -r -a DEPLOYID &lt;&lt;&lt; $DEPLOYARRAY&#x000A;echo "${DEPLOYID[1]}"&#x000A;echo "${DEPLOYID[3]}"&#x000A;echo "${DEPLOYID[5]}"&#x000A;aws deploy list-deployment-instances --deployment-id ${DEPLOYID[1]}&#x000A;aws ec2 describe-instances --filter "Name=tag:Name,Values=MadLib*" --query 'Reservations[].Instances[].[InstanceId, Tags[?Key==`Name`].Value | [0]]' --output table&#x000A;aws deploy get-deployment --deployment-id ${DEPLOYID[1]}&#x000A;&#x000A;</code></pre>


<br><br>

<p align="right">&copy; 2021 Welastic Sp. z o.o.<p>
