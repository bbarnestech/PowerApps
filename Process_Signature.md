**REQUIREMENTS**
1. PowerApps Pen Input to capture signatures
2. Mailbox to process signatures as email attachments
3. Power Automate flow to manipulate signature for your needs

Collect signature from pen input
```
ClearCollect(
  Signature, {
    Image:PenInput1.Image, imageName:"nameYourFile.jpg"
  }
);
```

Add signature to email attachments collection for processing
```
Clear(
  EmailAttachments
);
ForAll(
  Signature, Collect(
    EmailAttachments, {
      ContentBytes: Image, Name: imageName
    }
  )
);
```
Send signature to processing mailbox.  Power Automate flow triggered on new email arrival which can process and store the signature in your desired location.
```
Office365.SendEmail(
  "signatures@yourdomain.com", Subject, Body, {
    Attachments:EmailAttachments, Importance:"Normal", IsHtml:true
  }
);
```

Once the signature is sent to the processing email address it can be manipulated in a number of ways 
including upload to sharepoint list or other data storage compatible with Power Automate.
An example case would be to store the signature with a unique name and recall the signature from PowerApps
to add to a form via HTML template

