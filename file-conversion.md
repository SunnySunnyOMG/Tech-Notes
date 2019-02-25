# How Do I Implement the "Preview Documents Online" Feature in Our Product?

## Description

A need in our app is to allow users to upload their documents/notes and then let others view them online.

The documents here could be 

* `.pdf` 
* Microsoft Office file ( like `.doc`, `.docx`, `.ppt`, `.pptx`).
* some other file types that can easily handle with, e.g. `.txt`


## Original Idea
At first, I thought it easy to implement because I know there are some Microsoft Office APIs to view the file online and as for the `.pdf`, the modern browser can already handle them.

However, after carefully thinking, I gave up implementing the feature in this way. The reason was in future we would probably make the file download as a charged service for other people who did't own this file. On the other hand, using those third-party APIs in front-end usually means you must make your download URL public to get the APIs working. Users can easily get your download URL from the Chrome dev tools or even worse from the location bar.

## First Try

Then I decided to make our own server to handle this requirement. The basic idea was when a user uploaded a file, if it is a Microsoft document, the server would also manage to make a "preview-version" copy, which could be a pdf/image file with 1 or 2 pages only for preview, in our storage database. Free users can preview the file and even download it but can never touch the original file.

The idea might be work, but soon I found the file conversion is very difficult to realize and some open source libraries can do nothing with a non-open XML file (like `.doc`, `.ppt`, `.xls`). In fact, the online file conversion system itself could be another project and some companies run business based on this service. As a small, start-up company, we cannot afford this.

This feature bothered me several days and I couldn't give a good solution.

## Google Drive!

One day at night when I was going to sleep, I suddenly remembered when you upload a document to Google Drive, even it is a `.doc`, you can view/edit it in a very short time as a Google Doc file. This means,

**1. Google has already implemented such a feature that can convert `.doc/.ppt/.xls` to their own Google Doc types;**

Moreover, I know,

**2. Google Drive has some RESTful APIs for developers to upload files to the cloud;**

after I scanned their tech document quickly, I confirmed,

**3. they do have an API to DOWNLOAD GOOGLE DOC FILE AS PDF.**


Thus, finally, I implement the file conversion in this way,

> **1. upload the Microsoft document to Google Drive**
> 
> **2. Google automatically convert those files to Google Doc types**
> 
> **3. I download the Google Doc as pdf**
> 
> **4. use an open source document process tool, e.g. pdf.js, to get the preview version of the document**

Then I can save the "preview version" to our cloud storage which can feed the front-end in the future.

I knew there must be a better practice, but for a small company, this already works like a charm and more important, **it's totally free**.

## Summary

This happened when I just graduated from university and joined the Wooko team.

Although I was new there and had plenty of things to learn, I manage to implement the "big" feature in our product with limited money and resources.

This still makes me feel proud :)
