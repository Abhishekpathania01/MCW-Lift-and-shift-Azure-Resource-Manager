
![](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Azure Resource Manager
</div>

<div class="MCWHeader2">
Supplement to the Hands-on Lab: Introduction to DSC
</div>

<div class="MCWHeader3">
July 2018
</div>


Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2018 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

<!-- TOC -->

- [Azure Resource Manager introduction to DSC](#azure-resource-manager-introduction-to-DSC)
    - [Abstract and learning objectives](#abstract-and-learning-objectives)
    - [Exercise 1: Create Powershell Desired State Configuration](#Exercise-1-create-powershell-desired-state-configuration)
        - [Task 1: Write an HTML file](#task-1-write-an-html-file)
        - [Task 2: Write the configuration](#task-2-write-the-configuration)


<!-- /TOC -->

# Azure Resource Manager introduction to DSC

## Abstract and learning objectives

In this hands-on lab, you will learn how to author a Desired State Configuration using Powershell DSC.  This lab will also cover applying DSC to a virtual machine using virtual machine extensions, how to verify DSC has been applied successfully, and how to troubleshoot DSC.

## Exercise 1: Create Powershell Desired State Configuration

Duration: 15 minutes

In this exercise, you will create a powershell Desired State Configuration (DSC) and apply the configuration manually.

### Task 1: Write an HTML file

1. Create the HTML file that we will use as the website content:

In your root folder, create a folder named `test`.

In a text editor, type the following text:

```html
<head></head>
<body>
<p>Hello World!</p>
</body>
```

Save this as `index.htm` in the `test` folder you created earlier.

### Task 2: Write the configuration

1. In the VM created in the 'Before hands on lab...' open PowerShell ISE and type the following:
```powershell
Configuration WebsiteTest {

    # Import the module that contains the resources we're using.
    Import-DscResource -ModuleName PsDesiredStateConfiguration

    # The Node statement specifies which targets this configuration will be applied to.
    Node 'localhost' {

        # The first resource block ensures that the Web-Server (IIS) feature is enabled.
        WindowsFeature WebServer {
            Ensure = "Present"
            Name   = "Web-Server"
        }

        # The second resource block ensures that the website content copied to the website root folder.
        File WebsiteContent {
            Ensure = 'Present'
            SourcePath = 'c:\test\index.htm'
            DestinationPath = 'c:\inetpub\wwwroot'
        }
    }
}
```

Save the file as `WebsiteTest.ps1`.



