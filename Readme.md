# Dshield-Honeypot-AWS-setup
Write up on how to setup your Dshield honeypot on an AWS instance

****
ATTENTION: If you are here becuase of your raspberrypi install reports that your webserver is not exposed (like how mine was), please check this [guide](https://github.com/DShield-ISC/dshield/blob/main/docs/dshield-architecture/Architecture.md) first. =D
****

Fellow Sans.edu student or enthusiast, you will first need an AWS account to proceed. Registering for one will not be covered here, but here is a [link](https://docs.aws.amazon.com/lex/latest/dg/gs-account.html) to Amazon's documentation.

You will also need to register a [dshield account](https://dshield.org/) to have an API key and visibility of what you are forwarding to the Storm Center.

I was able to put this together by using instructions/guides from [Dr. Johannes Ullrich](https://www.youtube.com/watch?v=fMqhoNnyvmE) the founder of Internet Storm Center, [Guy Bruneau](https://github.com/bruneaug/DShield-SIEM?tab=readme-ov-file) my ISC handler, and [15HzMonitor](https://github.com/15HzMonitor/Internship-Blog-Post/blob/main/1.%20AWS%20DShield%20Sensor%20Setup.md) a fellow student (if you ever read this please correct me if I'm wrong)

****
1) After logging in and selecting your region, punch in ec2 in the search bar an click on click <span style="color:cyan;">EC2</span> under Services. When re-directed, click <span style="color:cyan;">Launch Instance</span>

![1](screenshots/1.png)
![2](screenshots/2.png)

2) Pick a <span style="color:cyan;">name</span> for your instance. Choose <span style="color:cyan;">Ubuntu</span> for you OS, <span style="color:cyan;">Ubuntu Server 22.04 LTS</span> for the machine image, and <span style="color:cyan;">t2.micro</span> for instance type
![3](screenshots/3.png)
![4](screenshots/4.png)
3) scroll down to see options on creating your ssh key. Click on <span style="color:cyan;"> Create new key pair</span>. Pick a <span style="color:cyan;">Key pair name</span>, <span style="color:cyan;">RSA</span> for key pair type, <span style="color:cyan;">PEM</span> for key file format and then click <span style="color:cyan;">Create key pair</span>. <span style="color:red;">__Save it somewhere safe__</span> we'll use this shortly.
![5](screenshots/5.png)
![6](screenshots/6.png)
4) even further under <span style="color:cyan;">Network settings</span>, choose <span style="color:cyan;">Create securtiy group</span> and <span style="color:cyan;">Allow SSH traffic</span> (yes it says anywhere we'll deal with that in a bit)
![7](screenshots/7.png)
5) For <span style="color:cyan;">storage </span> put in <span style="color:cyan;">30</span>. You should see a message saying that this eligible for free tier customers
![8](screenshots/8.png)
6) check the <span style="color:cyan;">Summary</span> and click <span style="color:cyan;">Launch Instance</span>. You should see a pop-up confimation.
![9](screenshots/9.png)
7) On the left handside click on <span style="color:cyan;">Instances</span> > <span style="color:cyan;">tick box for your instnace</span> >
 <span style="color:cyan;">Public IPv4 address</span>
![10](screenshots/10.png)

8) open a powershell on you pc and type in: 


<div style="margin-bottom: 10px;">
  <pre style="background-color: #f4f4f4; padding: 10px; border-radius: 5px; max-width: 100%; overflow-x: auto;">
    <code id="codeBlock" style="white-space: pre-wrap;">
      ssh -i sshkey user@YourInstanceIP
    </code>
  </pre>
  <button onclick="copyToClipboard()" style="background-color: #4CAF50; color: white; padding: 5px 10px; border: none; border-radius: 3px; cursor: pointer;">
    Copy
  </button>
</div>

<script>
  function copyToClipboard() {
    var copyText = document.getElementById("codeBlock");
    var range = document.createRange();
    range.selectNode(copyText);
    window.getSelection().addRange(range);
    document.execCommand("copy");
  }
</script>

</script>

![11](screenshots/11.png)