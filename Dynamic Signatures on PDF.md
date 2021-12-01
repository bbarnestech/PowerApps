REQUIREMENTS
1. HTML template stored in Power Apps HTML TEXT object.  The HTML template must have unique variables where dynamic data or images need to be added to the document.
  ```
  <img src='replaceMe_LogoVariable' height='80'>
  ```
2. Onedrive to handle the pdf conversion

In this example the replaceMe_LogoVariable is replaced with a company logo image.  This can be applied to any image file such as the signatures obtained through [PowerApps Signature Processing](https://github.com/bbarnestech/PowerApps/blob/main/Process_Signature) 

A Power Automate flow must be created.  In the example case this is an instant cloud flow that is triggered by Power Apps. The Power Apps trigger passes the HTML template to the flow for processing

Flow Steps:
1. Create steps to gather necessary dynamic data.  In the example case a company logo is retrieved from Sharepoint by utilizing the Get File Content step.  
2. Use the Initialize Variable step to create a variable that will hold the dynamic data information to be inserted into the template.  the variable used in this case is 'logo' 
   The variable value should equal the dataUri of the dynamic data gathered in step 1.  
   Ex. dataUri(body('Get_Logo_step'))
3. Use the Compose step to replace the necessary data in the HTML template
   ```
   replace(triggerBody()['ComposeHTML_Inputs'],'replaceMe_LogoVariable',variables('logo'))
   ```
4. Use the OneDrive Create File step to complete the formatting of the HTML template.  File Content should be the output of the Compose step.
5. Use the OneDrive Convert File step to convert the HTML template to a PDF document.  File should be the ID output from the Create File step.

The PDF file can now be sent or stored based upon your needs.  
