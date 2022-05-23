# EC2

We'll do everything under the EC2 page

## Network & Security

First thing create a security group for the EC2 instances to use:

- Give it a name to the s.g and a description
- Using the default VPC
- Add some inbound rules
  - SSH to anywhere
  - HTTP to anywhere
  - HTTPS to anywhere
  - Custom to 3000 bc we're hosting our app in that port

## Launch Templates

Create a blueprint for the autoscaling group to use, we can tell the type of Os, Image, EC2 Instance to use, what code to download:

- Give it a name
- For the Image we use Free Tier Eligible Amazon Linux 2
- For the Instance type we use t2.micro Free Tier Eligible
- Select Key Pair (create one if you don't have it)
- Select our S.G created in 1st Step
- In Advanced Details
  - In the User data here is the command that EC2 is gonna run after they get spun up, also we'll specify the command for downloading the code from our repo and then run a command to start it (the script it's in the sh file)

## Auto Scaling Groups

Now we can create the auto scaling groups:

- Give it a name
- Select the template we just created
- Select latest version
- For network select default VPC where we create our SG in, for subnets select first 3 defaults
- For load balancer:
  - We create a new balancer
    - We select Application Load Balancer
    - We choose Internet-facing bc we're gonna access it directly
    - For port number we use the 3000, and create a new target group
- Configuring group size and scaling policies
  - Desire capacity we want 2 instances, with a minimum of 1 and a max of 3 (for this example)
  - We define a scaling policy (whenever avg cpu utilization across diff ec2 instances it's above 20% > scale out to create additional one)
    - We use avg cpu
    - Target value for testing purposes to 20

## EC2 Dashboard

Here we can see our 2 EC2 instances spin up after 5m, if we copy the address and add the port we should have the same content on both instances.

To fix typing the URL without the port we go to load balancers of our app > listeners, we check the HTTP 3000 and Edit > Change port to 80
