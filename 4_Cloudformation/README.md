<img src="https://welastic.pl/wp-content/uploads/2020/05/cropped-welastic_logo-300x259.png" alt="Welastic logo" width="100" align="left">
<br><br>
<br><br>
<br><br> 

<h2 id="step1">Lab overview</h2>

<p>Deploying infrastructure in a consistent, reliable manner is difficult—it requires people to follow documented procedures without taking any undocumented shortcuts. In addition, it can be difficult to deploy infrastructure outside of normal business hours when less staff are available. AWS CloudFormation changes this by defining infrastructure in a template that can be automatically deployed—even on an automated schedule.</p>

<p>In this lab, you will learn how to deploy multiple layers of infrastructure with AWS CloudFormation. You will also learn how to update a stack, explore templates with AWS CloudFormation Designer, and delete a stack.</p>

<p><strong>Objectives</strong></p>

<p>After completing this lab, you will be able to:</p>
<ul>
<li>Use AWS CloudFormation to deploy a networking layer</li>
<li>Use AWS CloudFormation to deploy an application layer that references the networking layer</li>
<li>Use AWS CloudFormation to update resources in a stack</li>
<li>Explore templates with AWS CloudFormation Designer</li>
<li>Delete an AWS CloudFormation stack that has a deletion policy</li>
</ul>
<p><strong>Duration</strong></p>

<h2 id="step3">Task 1: Deploying a networking layer</h2>

<p>A best practice is to deploy infrastructure in <em>layers</em>. Common layers include the following:</p>
<ul>
<li>Networking</li>
<li>Application</li>
<li>Database</li>
</ul>
<p>When using layers, you can re-use infrastructure templates between systems. For example, you can deploy a common network topology between Dev/Test/Production or deploy a standard database for multiple applications.</p>

<p>In this task, you deploy an AWS CloudFormation template that creates a <em>networking layer</em> using Amazon Virtual Private Cloud (Amazon VPC).</p>
<ol start="4">
<li>Right-click the following link and download the template to your computer: <a href="lab-network.yaml" target="_blank">lab-network.yaml</a>
</li>
</ol>
<p><i class="fas fa-comment"></i> <strong>Note</strong> If you want to see how resources are defined, you can open the template in a text editor.</p>

<p>AWS CloudFormation templates can be written in JSON or YAML. YAML is similar to JSON, but it is easier to read and edit.</p>
<ol start="5">
<li><p>In the AWS Management Console, on the <span style="background-color:#232f3e;font-weight:bold;font-size:90%;color:white;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;">Services <i class="fas fa-angle-down"></i></span> menu, click <strong>CloudFormation</strong>.</p></li>
<li><p>Click <span style="background-color:#ec7211;font-weight:bold;font-size:90%;color:white;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;white-space: nowrap;">Create stack</span> and configure:</p></li>
</ol>
<p><strong>Step 1: Specify template</strong></p>
<ul>
<li>
<strong>Template source:</strong> Select <span style="font-weight:bold;background-color:#f1faff;font-size:90%;border-color:#00A1C9;border-width:1px;border-style:solid;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;"><i class="far fa-dot-circle" style="color:#007dbc;"></i> Upload a template file</span>
</li>
<li>
<strong>Upload a template file:</strong> Click <span style="background-color:white;font-weight:bold;font-size:90%;color:#545b64;border-color:#545b64;border-radius:2px;border-width:1px;border-style:solid;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;">Choose file</span> and select the <strong>lab-network.yaml</strong> file you downloaded</li>
<li>Click <span style="background-color:#ec7211;font-weight:bold;font-size:90%;color:white;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;white-space: nowrap;">Next</span>
</li>
</ul>
<p><strong>Step 2: Specify stack details</strong></p>
<ul>
<li>
<strong>Stack name: </strong>lab-network-x where x is your student ID
</li>
<li>Click <span style="background-color:#ec7211;font-weight:bold;font-size:90%;color:white;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;white-space: nowrap;">Next</span>
</li>
</ul>
<p><strong>Step 3: Configure stack options</strong></p>
<ul>
<li>Tags:
<ul>
<li>
<strong>Key: </strong>application
</li>
<li>
<strong>Value: </strong> inventory
</li>
</ul>
</li>
<li>Click <span style="background-color:#ec7211;font-weight:bold;font-size:90%;color:white;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;white-space: nowrap;">Next</span>
</li>
</ul>
<p><strong>Step 4: Review</strong></p>
<ul>
<li>Click <span style="background-color:#ec7211;font-weight:bold;font-size:90%;color:white;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;white-space: nowrap;">Create stack</span>
</li>
</ul>
<p>AWS CloudFormation now uses the template to generate a <em>stack</em> of resources.</p>

<p>The specified <em>tags</em> will be automatically propagated to the resources that are created, making it easier to identify resources that a particular application uses.</p>
<ol start="7">
<li><p>Click the <strong>Stack info</strong> tab.</p></li>
<li><p>Wait for the <strong>Status</strong> to change to <span style="color: green;"><i class="far fa-check-circle"></i> CREATE_COMPLETE</span>.</p></li>
</ol>
<p><i class="fas fa-comment"></i> <strong>Note</strong> Click the refresh <i class="fas fa-redo"></i> icon every 15 seconds to update the display if necessary.</p>

<p>Now, examine the resources that were created.</p>
<ol start="9">
<li>Click the <strong>Resources</strong> tab.</li>
</ol>
<p>You see a list of the resources created by the template.</p>

<p><i class="fas fa-comment"></i> <strong>Note</strong> If the list is empty, click the refresh <i class="fas fa-redo"></i> icon to update the list.</p>
<ol start="10">
<li>Click the <strong>Events</strong> tab and scroll through the listing.</li>
</ol>
<p>The listing shows (in reverse chronological order) the activities that AWS CloudFormation performed, such as starting to create a resource and then completing the resource creation. Any errors encountered during the creation of the stack will be listed in this tab.</p>
<ol start="11">
<li>Click the <strong>Outputs</strong> tab.</li>
</ol>
<p>An AWS CloudFormation stack can provide <em>output information</em>, such as the ID of specific resources and links to resources.</p>

<p>You see two outputs:</p>
<ul>
<li>
<strong>PublicSubnet:</strong> The value is the ID of the public subnet that was created (for example, <em>subnet-08aafd57f745035f1</em>)</li>
<li>
<strong>VPC:</strong> The value is the ID of the VPC that was created (for example, <em>vpc-08e2b7d1272ee9fb4</em>)</li>
</ul>
<p>The <strong>Outputs</strong> tab can also provide values that other stacks will use. The <strong>Export name</strong> column shows these values. In this case, the VPC and subnet IDs are given an export name so that other stacks can retrieve the values and build resources inside the VPC and subnet. You will use these values in the next task.</p>
<ol start="12">
<li>Click the <strong>Template</strong> tab.</li>
</ol>
<p>This tab shows the template that was used to create the stack. In this case, it shows the template that you uploaded while creating the stack. Feel free to examine the template and see the resources that were created. The <strong>Outputs</strong> section at the end of the template defined the values to export.</p>



<h2 id="step4">Task 2: Deploying an application layer</h2>

<p>Now that the network layering is deployed, it is time to deploy an <em>application layer</em> that contains an Amazon Elastic Compute Cloud (Amazon EC2) instance and a security group.</p>

<p>The AWS CloudFormation template will import the VPC and subnet IDs from the outputs of the existing AWS CloudFormation stack. The template will then use this information to create the security group in the VPC and the EC2 instance in the subnet.</p>
<ol start="13">
<li>Right-click the following link and download the template to your computer: <a href="lab-application.yaml" target="_blank">lab-application.yaml</a>
</li>
</ol>
<p><i class="fas fa-comment"></i> <strong>Note</strong> If you want to see how resources are defined, you can open the template in a text editor.</p>
<ol start="14">
<li>In the left navigation pane, click <strong>Stacks</strong>.</li>
</ol>
<p><i class="fas fa-comment"></i> <strong>Note</strong> You may need to expand the navigation pane by clicking the menu <i class="fas fa-bars"></i> icon.</p>
<ol start="15">
<li><p>Click <span style="background-color:#DEDEDE;font-weight:bold;font-size:90%;color:#444;border-radius:1px;border-width:1px;border-style:solid;border-color:#444;padding-top:2px;padding-bottom:2px;padding-left:10px;padding-right:10px;white-space: nowrap;">Create stack</span> and then <strong>With new resources (standard)</strong></p></li>
<li><p>Configure the following:</p></li>
</ol>
<p><strong>Step 1: Specify template</strong></p>
<ul>
<li>
<strong>Template source:</strong> Click <span style="font-weight:bold;background-color:#f1faff;font-size:90%;border-color:#00A1C9;border-width:1px;border-style:solid;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;"><i class="far fa-dot-circle" style="color:#007dbc;"></i> Upload a template file</span>
</li>
<li>
<strong>Upload a template file:</strong> Click <span style="background-color:white;font-weight:bold;font-size:90%;color:#545b64;border-color:#545b64;border-radius:2px;border-width:1px;border-style:solid;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;">Choose file</span> and select the <strong>lab-application.yaml</strong> file you downloaded</li>
<li>Click <span style="background-color:#ec7211;font-weight:bold;font-size:90%;color:white;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;white-space: nowrap;">Next</span>
</li>
</ul>
<p><strong>Step 2: Specify stack details</strong></p>
<ul>
<li>
<strong>Stack name: </strong> lab-application-x
</li>
<li>
<strong>NetworkStackName: </strong> lab-network-x (x same as step 2 in task 1)
</li>
<li>Click <span style="background-color:#ec7211;font-weight:bold;font-size:90%;color:white;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;white-space: nowrap;">Next</span>
</li>
</ul>
<p><i class="fas fa-comment"></i> <strong>Note</strong> The <strong>NetworkStackName</strong> parameter tells the template the name of the first stack you created (<em>lab-network-x</em>) so that the template can retrieve values from that stack's outputs.</p>

<p><strong>Step 3: Configure stack options</strong></p>
<ul>
<li>Tags:
<ul>
<li>
<strong>Key: </strong> application
</li>
<li>
<strong>Value: </strong>inventory
</li>
</ul>
</li>
<li>Click <span style="background-color:#ec7211;font-weight:bold;font-size:90%;color:white;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;white-space: nowrap;">Next</span>
</li>
</ul>
<p><strong>Step 4: Review</strong></p>
<ul>
<li>Click <span style="background-color:#ec7211;font-weight:bold;font-size:90%;color:white;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;white-space: nowrap;">Create stack</span>
</li>
</ul>
<p>While the stack is being created, examine the <strong>Events</strong> and <strong>Resources</strong> tabs to view the resources that are being created.</p>
<ol start="17">
<li>Wait for the <strong>Status</strong> (on the <strong>Stack info</strong> tab) to change to <span style="color: green;"><i class="far fa-check-circle"></i> CREATE_COMPLETE</span>.</li>
</ol>
<p>Your application is now ready!</p>
<ol start="18">
<li><p>Click the <strong>Outputs</strong> tab.</p></li>
<li><p>Copy the <strong>URL</strong> that is displayed, open a new web browser tab, paste the URL, and press ENTER.</p></li>
</ol>
<p>A new browser tab opens, taking you to the application running on the web server.</p>

<p>An AWS CloudFormation stack can also reference values from another stack. For example, the following is a portion of the <em>lab-application</em> template that references the <em>lab-network</em> template:</p>
<pre class="highlight yaml"><code>  <span class="na">WebServerSecurityGroup</span><span class="pi">:</span>&#x000A;    <span class="na">Type</span><span class="pi">:</span> <span class="s">AWS::EC2::SecurityGroup</span>&#x000A;    <span class="na">Properties</span><span class="pi">:</span>&#x000A;      <span class="na">GroupDescription</span><span class="pi">:</span> <span class="s">Enable HTTP ingress</span>&#x000A;      <span class="na">VpcId</span><span class="pi">:</span>&#x000A;        <span class="s">Fn::ImportValue</span><span class="pi">:</span>&#x000A;          <span class="kt">!Sub</span> <span class="s">${NetworkStackName}-VPCID</span>&#x000A;</code></pre>
<p>The last line uses the NetworkStackName (<em>lab-network</em>), which you provided when the stack was created. The template then imports the value of <em>lab-network-VPCID</em> from the outputs of the first stack and inserts the value into the VPC ID field of the security group definition. The result is that the security group is created in the VPC that the first stack created.</p>

<p>In another example, the following is the code that places the Amazon EC2 instance into the correct subnet:</p>
<pre class="highlight yaml"><code>  <span class="na">SubnetId</span><span class="pi">:</span>&#x000A;    <span class="s">Fn::ImportValue</span><span class="pi">:</span>&#x000A;    <span class="kt">!Sub</span> <span class="s">${NetworkStackName}-SubnetID</span>&#x000A;</code></pre>
<p>The template takes the subnet ID from the <em>lab-network</em> stack and uses it in the <em>lab-application-x</em> stack to launch the instance into the public subnet that the first stack created.</p>

<p>This demonstrates how you can use multiple AWS CloudFormation stacks to deploy infrastructure in multiple layers.</p>



<h2 id="step5">Task 3: Updating a stack</h2>

<p>AWS CloudFormation can also update a stack that has been deployed. When updating a stack, AWS CloudFormation will only modify or replace the resources that are being changed. Any resources that are not being changed are left as-is.</p>

<p>In this task, you update the <em>lab-application-x</em> stack to modify a setting in the security group. AWS CloudFormation will not modify any of the other resources.</p>

<p>First, you examine the current settings on the security group.</p>
<ol start="20">
<li><p>In the AWS Management Console, on the <span style="background-color:#232f3e;font-weight:bold;font-size:90%;color:white;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;">Services <i class="fas fa-angle-down"></i></span> menu, click <strong>EC2</strong>.</p></li>
<li><p>In the left navigation pane, click <strong>Security Groups</strong>.</p></li>
<li><p>Select <i class="far fa-check-square"></i> <strong>Web Server Security Group</strong>.</p></li>
<li><p>On the lower half of the page, click the <strong>Inbound rules</strong> tab.</p></li>
</ol>
<p>The security group currently only has one rule, which permits HTTP traffic.</p>

<p>Now, return to AWS CloudFormation to update the stack.</p>
<ol start="24">
<li><p>On the <span style="background-color:#232f3e;font-weight:bold;font-size:90%;color:white;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;">Services <i class="fas fa-angle-down"></i></span> menu, click <strong>CloudFormation</strong>.</p></li>
<li><p>Right-click this link and download the updated template to your computer: <a href="lab-application2.yaml" target="_blank">lab-application2.yaml</a></p></li>
</ol>
<p>This template has an additional configuration to permit inbound SSH traffic on port 22:</p>
<pre class="highlight yaml"><code>  <span class="pi">-</span> <span class="na">IpProtocol</span><span class="pi">:</span> <span class="s">tcp</span>&#x000A;    <span class="na">FromPort</span><span class="pi">:</span> <span class="s">22</span>&#x000A;    <span class="na">ToPort</span><span class="pi">:</span> <span class="s">22</span>&#x000A;    <span class="na">CidrIp</span><span class="pi">:</span> <span class="s">0.0.0.0/0</span>&#x000A;</code></pre><ol start="26">
<li><p>Click the <strong>lab-application-x</strong> stack name.</p></li>
<li><p>Click <span style="background-color:white;font-weight:bold;font-size:90%;color:#545b64;border-color:#545b64;border-radius:2px;border-width:1px;border-style:solid;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;">Update</span> and configure:</p></li>
</ol><ul>
<li>Click <span style="font-weight:bold;background-color:#f1faff;font-size:90%;border-color:#00A1C9;border-width:1px;border-style:solid;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;"><i class="far fa-dot-circle" style="color:#007dbc;"></i> Replace current template</span>
</li>
<li>
<strong>Template source:</strong> Click <span style="font-weight:bold;background-color:#f1faff;font-size:90%;border-color:#00A1C9;border-width:1px;border-style:solid;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;"><i class="far fa-dot-circle" style="color:#007dbc;"></i> Upload a template file</span>
</li>
<li>
<strong>Upload a template file:</strong> Click <span style="background-color:white;font-weight:bold;font-size:90%;color:#545b64;border-color:#545b64;border-radius:2px;border-width:1px;border-style:solid;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;">Choose file</span> and select the <strong>lab-application2.yaml</strong> file you downloaded</li>
</ul><ol start="28">
<li>Click <span style="background-color:#ec7211;font-weight:bold;font-size:90%;color:white;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;white-space: nowrap;">Next</span> <strong>three times</strong> to advance to the <strong>Review</strong> page.</li>
</ol>
<p>In the <strong>Change set preview</strong> section at the bottom of the page, AWS CloudFormation displays the resources that will be updated, as shown in the following image:</p>

<p><img src="img/change-set-preview.png" alt="Change set preview"></p>

<p>AWS CloudFormation will <em>modify</em> the WebServerSecurityGroup without needing to replace it (<em>Replacement = False</em>). This means there will be a minor change to the security group, and no references to the security group will need to change.</p>
<ol start="29">
<li><p>Click <span style="background-color:#ec7211;font-weight:bold;font-size:90%;color:white;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;white-space: nowrap;">Update stack</span></p></li>
<li><p>Wait for the <strong>Status</strong> (in the <strong>Stack info</strong> tab) to change to <span style="color: green;"><i class="far fa-check-circle"></i> UPDATE_COMPLETE</span>.</p></li>
</ol>
<p><i class="fas fa-comment"></i> <strong>Note</strong> Click the refresh <i class="fas fa-redo"></i> icon every 15 seconds to update the display if necessary.</p>

<p>You can now verify the change.</p>
<ol start="31">
<li><p>Return to the <strong>EC2</strong> console. In the left navigation pane, click <strong>Security Groups</strong>.</p></li>
<li><p>Select the <strong>WebServerSecurityGroup</strong>.</p></li>
</ol>
<p>The <strong>Inbound rules</strong> tab displays an additional rule for SSH traffic.</p>

<p>This demonstrates how you can deploy changes in a repeatable, documented process. You can store the AWS CloudFormation template in a source code repository, such as AWS CodeCommit, to maintain a history of the template and the infrastructure that has been deployed.</p>



<h2 id="step6">Task 4: Exploring templates with AWS CloudFormation Designer</h2>

<p><em>AWS CloudFormation Designer</em> is a graphic tool for creating, viewing, and modifying AWS CloudFormation templates. With Designer, you can diagram your template resources using a drag-and-drop interface. Then, edit resource details with the integrated JSON and YAML editor. Whether you are a new or experienced AWS CloudFormation user, Designer can help you quickly see the interrelationship between a template's resources and easily modify templates.</p>

<p>In this task, you gain some hands-on experience with AWS CloudFormation Designer.</p>
<ol start="33">
<li><p>On the <span style="background-color:#232f3e;font-weight:bold;font-size:90%;color:white;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;">Services <i class="fas fa-angle-down"></i></span> menu, click <strong>CloudFormation</strong>.</p></li>
<li><p>In the left navigation pane, click <strong>Designer</strong>.</p></li>
</ol>
<p><i class="fas fa-comment"></i> <strong>Note</strong> You may need to expand the navigation pane by clicking the menu <i class="fas fa-bars"></i> icon.</p>
<ol start="35">
<li>Use the file menu <i class="fas fa-file"></i> to open a <strong>local file</strong> and select the <strong>lab-application2.yaml</strong> template you downloaded previously.</li>
</ol>
<p>Designer displays a graphical representation of the template, as shown in the following image:</p>

<p><img src="img/designer.png" alt="CloudFormation Designer"></p>

<p>Rather than drawing a typical architecture diagram, Designer is a visual editor for AWS CloudFormation templates, so Designer draws the resources defined in a template and their relationships to each other.</p>
<ol start="36">
<li>Experiment with the features of the Designer. The following are a few things to try:</li>
</ol><ul>
<li>Click a displayed resource. The lower pane then displays the portion of the template that defines the resource.</li>
<li>From the <strong>Resource types</strong> pane on the left, drag a new resource into the design area. The definition of the resource is automatically inserted into the template.</li>
<li>Drag the resource connector circles to create relationships between resources.</li>
<li>Open the <strong>lab-network.yaml</strong> template you downloaded earlier in the lab, and explore its resources too.</li>
</ul>


<h2 id="step7">Task 5: Deleting the stack</h2>

<p>When resources are no longer required, AWS CloudFormation can delete the resources built for the stack.</p>

<p>You can specify a <em>deletion policy</em> for resources. Such a policy can preserve or (in some cases) back up a resource when its stack is deleted. This is useful for retaining databases, disk volumes, or any resource that might be required after stack deletion.</p>

<p>The <em>lab-application-x</em> stack has been configured to take a snapshot of an Amazon Elastic Block Store (Amazon EBS) disk volume before it is deleted. The following code block shows this section of the AWS CloudFormation template:</p>
<pre class="highlight yaml"><code>  <span class="na">DiskVolume</span><span class="pi">:</span>&#x000A;    <span class="na">Type</span><span class="pi">:</span> <span class="s">AWS::EC2::Volume</span>&#x000A;    <span class="na">Properties</span><span class="pi">:</span>&#x000A;      <span class="na">Size</span><span class="pi">:</span> <span class="s">100</span>&#x000A;      <span class="na">AvailabilityZone</span><span class="pi">:</span> <span class="kt">!GetAtt</span> <span class="s">WebServerInstance.AvailabilityZone</span>&#x000A;      <span class="na">Tags</span><span class="pi">:</span>&#x000A;        <span class="pi">-</span> <span class="na">Key</span><span class="pi">:</span> <span class="s">Name</span>&#x000A;          <span class="na">Value</span><span class="pi">:</span> <span class="s">Web Data</span>&#x000A;    <span class="na">DeletionPolicy</span><span class="pi">:</span> <span class="s">Snapshot</span>&#x000A;</code></pre>
<p>The <em>DeletionPolicy</em> in the final line directs AWS CloudFormation to create a snapshot of the disk volume before it is deleted.</p>

<p>You will now delete the <em>lab-application-x</em> stack and see the results of this deletion policy.</p>
<ol start="37">
<li><p>To close Designer and return to the main AS CloudFormation console, click the <span style="color:#1166bb;">Close</span> link at the top-left of the page.</p></li>
<li><p>Click the name of the <strong>lab-application-x</strong> stack.</p></li>
<li><p>Click <span style="background-color:white;font-weight:bold;font-size:90%;color:#545b64;border-color:#545b64;border-radius:2px;border-width:1px;border-style:solid;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;">Delete</span></p></li>
<li><p>Click <span style="background-color:#ec7211;font-weight:bold;font-size:90%;color:white;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;white-space: nowrap;">Delete stack</span></p></li>
</ol>
<p>You can monitor the deletion process in the <strong>Events</strong> tab and update the screen by clicking the refresh <i class="fas fa-redo"></i> icon occasionally. You might also see a reference to the Amazon EBS snapshot being created.</p>
<ol start="41">
<li>Wait for the stack to be deleted. It will disappear from the <strong>Stacks</strong> list.</li>
</ol>
<p>The application stack has been removed, but the network stack is untouched. This reinforces the idea that different teams (for example, the network team or application team) can manage their own stacks.</p>

<p>Now, check that a snapshot was created of the EBS volume before it was deleted.</p>
<ol start="42">
<li><p>On the <span style="background-color:#232f3e;font-weight:bold;font-size:90%;color:white;padding-top:3px;padding-bottom:3px;padding-left:10px;padding-right:10px;">Services <i class="fas fa-angle-down"></i></span> menu, click <strong>EC2</strong>.</p></li>
<li><p>In the left navigation pane, click <strong>Snapshots</strong>.</p></li>
</ol>
<p>You should see a snapshot with a <strong>Started</strong> time in the last few minutes.</p>

<h2 id="step8">Conclusion</h2>

<p>Congratulations! You now have successfully:</p>
<ul>
<li>Used AWS CloudFormation to deploy a networking layer</li>
<li>Used AWS CloudFormation to deploy an application layer that references the networking layer</li>
<li>Used AWS CloudFormation to update resources in a stack</li>
<li>Explored templates with AWS CloudFormation Designer</li>
<li>Deleted an AWS CloudFormation stack that has a deletion policy</li>
</ul>
<h2 id="step9">End Lab</h2>

<br><br>

<p align="right">&copy; 2020 Welastic Sp. z o.o.<p>
