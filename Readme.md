# Dshield-Honeypot-AWS-setup
Write up on how to setup your Dshield honeypot on an AWS instance

****
ATTENTION: If you are here becuase of your raspberrypi install reports that your webserver is not exposed (like how mine was), please check this [guide](https://github.com/DShield-ISC/dshield/blob/main/docs/dshield-architecture/Architecture.md) first. =D
****

Fellow Sans.edu student or enthusiast, you will first need an AWS account to proceed. Registering for one will not be covered here, but here is a [link](https://docs.aws.amazon.com/lex/latest/dg/gs-account.html) to Amazon's documentation.

You will also need to register a [dshield account](https://dshield.org/) to have an API key and visibility of what you are forwarding to the Storm Center.

I was able to put this together by using instructions/guides from [Dr. Johannes Ullrich](https://www.youtube.com/watch?v=fMqhoNnyvmE) the founder of Internet Storm Center, [Guy Bruneau](https://github.com/bruneaug/DShield-SIEM?tab=readme-ov-file) my ISC handler, and [15HzMonitor](https://github.com/15HzMonitor/Internship-Blog-Post/blob/main/1.%20AWS%20DShield%20Sensor%20Setup.md) a fellow student (if you ever read this please correct me if I'm wrong)

****
1) After logging in and selecting your region, punch in ec2 in the search bar an click on EC2 under Services.

![1](screenshots/1.png)

