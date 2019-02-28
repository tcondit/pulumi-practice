# pulumi-practice

### Learning Pulumi by doing. Mostly Python.

This repo contains infrastructure code for AWS, built with Python-flavored Pulumi. Each subdirectory contains a standalone stack. As an experiment, each of these stacks is a separate release. We'll see if that works, or if not, where it breaks.

## Get ready

If not already available, you'll need to install a few things. I'm assuming you're on Mac or Linux. Windows may work too.

The instructions at [aws-py-webserver](https://github.com/pulumi/examples/tree/master/aws-py-webserver) should get you most of the way there, while walking you thru your first AWS infrastructure deployment. Bonus!

If you skipped that, here's the full list:

* An AWS account. ([sign up](https://aws.amazon.com/))
* Python v3.6 or later. ([option 1 (recommended)](https://docs.python-guide.org/starting/installation/) or [option 2](https://www.python.org/downloads/))
  * Note: If you go with option one, you can stop after doing the initial install. Since v3.3, Python has shipped with [venv](https://docs.python.org/3/library/venv.html) which is what we'll use to create and manage virtual environments.
* pip3, but note that it should have been included with Python v3.6 or later. Link provided just in case, but chances are good you won't need it. ([install](https://pip.pypa.io/en/stable/installing/))
* A Pulumi account ([sign up](https://app.pulumi.com/welcome))
* The Pulumi CLI ([install](https://pulumi.io/quickstart/))
* AWS credentials ([configure](https://pulumi.io/quickstart/aws/setup.html))
* An SSH key pair ([create](https://help.github.com/en/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key))

## Get set

The action is in the project subdirectories. Each subdirectory is intended to be self-contained. If you find that's not the case, feel free to open an issue.

Let's use the infrastructure project `vpc-with-ec2` to walk thru the rest of the steps. You'll set up a virtual environment with `venv`, fetch `Pulumi` dependency libraries, then deploy the stack.

### Set up a virtual environment

    ➜ cd pulumi-practiceinfrastructure/vpc-with-ec2/
    ➜ python3 -m venv venv
    ➜ source venv/bin/activate

### Fetch `Pulumi` dependency libraries

Here's everything we just created, including `requirements.txt`.

    ➜ ls
    Pulumi.dev.yaml  Pulumi.yaml      __main__.py      requirements.txt venv

`pip install` is all we need. Also `pip` is nagging me to upgrade.

    ➜ pip install -r requirements.txt
    Collecting pulumi>=0.16.4 (from -r requirements.txt (line 1))
      Downloading https://files.pythonhosted.org/packages/bd/60/93682c12996d2aca11081cb88473562d14b860ce0aa820f2de7f7414d2e7/pulumi-0.16.17-py2.py3-none-any.whl (130kB)
        100% |████████████████████████████████| 133kB 1.2MB/s
    Collecting pulumi_aws>=0.16.2 (from -r requirements.txt (line 2))
      Using cached https://files.pythonhosted.org/packages/0c/24/fee18eba45725b711cb3566e164a7d77dea6949c28b526c8c91b389151d7/pulumi_aws-0.16.10.tar.gz
    Collecting protobuf>=3.6.0 (from pulumi>=0.16.4->-r requirements.txt (line 1))
      Using cached https://files.pythonhosted.org/packages/18/e7/785830a65d1f1faba7dccfa8314f7afded1db8cc2860218226ba4b3f6150/protobuf-3.6.1-cp37-cp37m-macosx_10_6_intel.macosx_10_9_intel.macosx_10_9_x86_64.macosx_10_10_intel.macosx_10_10_x86_64.whl
    Collecting grpcio>=1.9.1 (from pulumi>=0.16.4->-r requirements.txt (line 1))
      Using cached https://files.pythonhosted.org/packages/df/33/c0561fe7c5e235325255f46c08bd3d07f2c80824feb22d057328eff1f8b7/grpcio-1.19.0-cp37-cp37m-macosx_10_9_x86_64.whl
    Requirement already satisfied: setuptools in ./venv/lib/python3.7/site-packages (from protobuf>=3.6.0->pulumi>=0.16.4->-r requirements.txt (line 1)) (40.6.2)
    Collecting six>=1.9 (from protobuf>=3.6.0->pulumi>=0.16.4->-r requirements.txt (line 1))
      Using cached https://files.pythonhosted.org/packages/73/fb/00a976f728d0d1fecfe898238ce23f502a721c0ac0ecfedb80e0d88c64e9/six-1.12.0-py2.py3-none-any.whl
    Installing collected packages: six, protobuf, grpcio, pulumi, pulumi-aws
      Running setup.py install for pulumi-aws ... done
    Successfully installed grpcio-1.19.0 protobuf-3.6.1 pulumi-0.16.17 pulumi-aws-0.16.10 six-1.12.0
    You are using pip version 18.1, however version 19.0.3 is available.
    You should consider upgrading via the 'pip install --upgrade pip' command.

### Deploy a stack

Make sure we're in the right place.

    ➜ pwd
    /Users/timc/z/src/github.com/tcondit/pulumi-practice/infrastructure/vpc-with-ec2

Start the update. Note we're deploying a [Pulumi stack](https://pulumi.io/reference/stack.html) called `dev`.

    ➜ pulumi up
    Previewing update (dev):

         Type                              Name              Plan
     +   pulumi:pulumi:Stack               vpc-with-ec2-dev  create
     +   ├─ aws:ec2:Vpc                    new-vpc           create
     +   ├─ aws:s3:Bucket                  new-bucket        create
     +   ├─ aws:ec2:InternetGateway        new-igw           create
     +   ├─ aws:ec2:Subnet                 new-subnet        create
     +   ├─ aws:ec2:SecurityGroup          new-sg            create
     +   ├─ aws:ec2:RouteTable             new-rt            create
     +   ├─ aws:ec2:Instance               new-ec2           create
     +   ├─ aws:ec2:RouteTableAssociation  new-rta           create
     +   ├─ aws:ec2:Route                  default-route     create
     +   └─ aws:ec2:Eip                    new-eip           create

    Resources:
        + 11 to create


    Do you want to perform this update?
    > yes
      no
      details

You can see we've got options (`yes`, `no`, `details`) on how to proceed. There's a lot here, so check out `details` at some point. For now, let's perform this update. This step will take a minute.

    Do you want to perform this update? yes
    Updating (dev):

         Type                              Name              Status
     +   pulumi:pulumi:Stack               vpc-with-ec2-dev  created
     +   ├─ aws:s3:Bucket                  new-bucket        created
     +   ├─ aws:ec2:Vpc                    new-vpc           created
     +   ├─ aws:ec2:InternetGateway        new-igw           created
     +   ├─ aws:ec2:Subnet                 new-subnet        created
     +   ├─ aws:ec2:SecurityGroup          new-sg            created
     +   ├─ aws:ec2:RouteTable             new-rt            created
     +   ├─ aws:ec2:Instance               new-ec2           created
     +   ├─ aws:ec2:RouteTableAssociation  new-rta           created
     +   ├─ aws:ec2:Route                  default-route     created
     +   └─ aws:ec2:Eip                    new-eip           created

    Outputs:
        AMI              : "ami-032509850cf9ee54e"
        bucket_name      : "new-bucket-5d4716d.s3.amazonaws.com"
        elasticIP        : "35.161.22.167"
        instanceID       : "i-0019a7f0311a54a27"
        internetGatewayID: "igw-0ad633ffd2ccc0ec1"
        privateIP        : "10.0.15.105"
        routeID          : "r-rtb-0765dc7e5e980dedf1080289494"
        routeTableID     : "rtb-0765dc7e5e980dedf"
        securityGroupID  : "sg-0b9cb0fe692920ebb"
        subnetID         : "subnet-0df76499eb12b2f53"
        vpcID            : "vpc-0cd8288ab71785033"

    Resources:
        + 11 created

    Duration: 53s

    Permalink: https://app.pulumi.com/tcondit/vpc-with-ec2/dev/updates/41

Correction: 53 seconds.

#### Things to note:

* Near the top is this status.

> `+   pulumi:pulumi:Stack               vpc-with-ec2-dev  created`

`vpc-with-ec2` is the name of the project, and the stack short name `dev` is appended to it, for a fully qualified stack name.

* `Pulumi` created 11 [Resources](https://pulumi.io/tour/programs-resources.html), based on Python objects.

* The `elasticIP` shown above is long gone, but yours should work! We'll `SSH` to your instance next.

## Go!

Time to make sure we can `SSH` to your brand new instance. It'll look like this.

    ➜ ssh -l ec2-user -i ~/Downloads/sl-us-west-2.pem $(pulumi stack output elasticIP)
    The authenticity of host '54.244.65.81 (54.244.65.81)' can't be established.
    ECDSA key fingerprint is SHA256:apdlxOZpKMFhflqI0qF6+3V2zsxfZ1QBMP4LIjTua6k.
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added '54.244.65.81' (ECDSA) to the list of known hosts.

           __|  __|_  )
           _|  (     /   Amazon Linux 2 AMI
          ___|\___|___|

    https://aws.amazon.com/amazon-linux-2/

    [ec2-user@ip-10-0-12-250 ~]$ exit
    logout
    Connection to 54.244.65.81 closed.

<!--

1. move my key ; show that we can get to the instance but can't get in
2. show `pulumi destroy`
3. update `key_name`
4. update `pulumi config` with secret
5. describe the problem with having Pulumi encrypt your private key ; move on anyway
6. `pulumi up` ; this time check the details ; ensure the user's `key_name` is pulled in
7. `ssh` for real this time (Amazon Linux: `ec2-user`)

-->

