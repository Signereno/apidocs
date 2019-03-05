# Quickstart

Securely collect digital signatures from your customers for your contracts and agreements, onboarding forms, declarations and more, using Nordic eIDs as mechanisms to sign with for your end users.

**Step 1:**

First, you need to establish a test account and API credentials. You can get a free API account in Idfy test environments by completing our onboarding form [here](https://onboard.idfy.io), and specifying that you want access to our signature services. You will then receive an e-mail with your API credentials, as well as some more information, test users and code examples.

**Step 2:**

When you have received your API credentials, you can use these to make calls towards the Idfy signature APIs. You can either make pure REST calls towards our APIs \(i.e. make your own HTTP client\), or use one of our pre-built SDKs in your preferred programming language. Check out our [SDKs](https://developer.idfy.io/sdk) and choose the SDK or approach that fits your needs best. One benefit of using an SDK is that you don't have to manage the API authentication and wrapping of HTTP calls yourself.

**Step 3:**

To get quickly started with testing the signature services, you can start using your preferred SDK and calling the appropriate methods within that given library, or you can for instance make some initial calls using cURL or the API tool Postman \(see more information and download Postman [here](https://www.getpostman.com/)\). For the sake of simplicity, we here show some initial calls using Postman. The Postman collection for our APIs can be [imported from Swagger](https://learning.getpostman.com/docs/postman/collections/data_formats/#importing-swagger). The Swagger document can be downloaded from our [API reference](https://developer.idfy.io/api) page by clicking the "Download" button at the top of the page.

**Step 4:**

Open Postman and the Idfy API reference collection. Browse to Documents, and "Documents\_Create". The parameters that are required for a minimal test follow below, with some example values that you can try, by pasting the JSON into the body of the request in Postman \(replace the example values that come from the collection\):

JSON:

```text
{
    "signers":[
        {
            "externalSignerId":"123abc",
            "redirectSettings":{
                "redirectMode":"redirect",
                "success":"https://www.idfy.io#success",
                "cancel":"https://www.idfy.io#cancel",
                "error":"https://www.idfy.io#error"
            },
            "signatureType":{
                "signatureMethods":["NO_BANKID"],
                "mechanism":"pkisignature"
            },
            "ui":{
                "language":"en"
            }
        }
    ],
    "title":"Signature test",
    "description":"A small text to sign",
    "contactDetails":{
        "email":"support@idfy.io"
    },
    "dataToSign":{
        "fileName":"shortText.txt",
        "base64Content":"SGVsbG8gd29ybGQhIFNpZ24gdGhpcyBzaG9ydCB0ZXh0IHRvIHRyeSBvdXQgSWRmeSBBUElz"
    },
    "externalId":"myDocumentID-42"
}
```

Explanation of parameters:

| Parameters | Example value | Explanation |
| :--- | :--- | :--- |
| signers \(array - see below\) | \(array of values, see below\) | These are the individual signers that are to sign the document |
| signers\[0\].externalSignerId | "123abc" | Your own reference for the person to sign the document. Could e.g. be the customer number |
| signers.\[0\].redirectSettings                    .redirectMode | "redirect" | Determines how the signing application should behave for the user, e.g. if you are planning on redirecting the user to the signing URL, or if you are planning to iframe the signing process into your webpage. |
| signers\[0\].redirectSettings                     .success | "[https://www.idfy.io\#success](https://www.idfy.io#success)" | The landing page on your end, where you want the user to be redirected after successful signature |
| signers\[0\].redirectSettings                     .cancel | "[https://www.idfy.io\#cancel](https://www.idfy.io#cancel)" | The landing page on your end, where you want the user to be redirected if he/she chooses to abort the signing process |
| signers\[0\].redirectSettings                     .error | "[https://www.idfy.io\#error](https://www.idfy.io#error)" | The landing page on your end, where you want the user to be redirected in case any errors occur during the signing process |
| signers\[0\].signatureType                        .signatureMethods | \["NO\_BANKID"\] | The method for the user to sign with. Could be |
| signers\[0\].signatureType                        .method | "pkisignature" | Determines which type of signature is to be used. pkisignature means that the native signature capability of the signature method is to be used. |
| signers\[0\].ui.language | "en" | Determines the language to load in the signing UI |
| title | "Signature test" | This is the title of the document, which is shown in the GUI for the signer |
| description | "A small test text to sign" | This is the description for the document to sign, which is shown in the GUI for the signer |
| dataToSign.fileName | "shortText.txt" | This is the file name for the document that is sent for signing. Note that the correct extension for the document must be provided, e.g. .txt for text signing, and .pdf for PDF documents. |
| dataToSign.base64Content | "SGVsbG8gd29ybGQhIFN                       pZ24gdGhpcyBzaG9ydCB                     0ZXh0IHRvIHRyeSBvdXQ                      gSWRmeSBBUElz" | This is the base64 encoded file contents of the document to sign. The example value corresponds to the text "Hello world! Sign this short text to try out Idfy APIs." |
| externalId | "myDocumentID-42" | This is your reference for the document to be signed. Could e.g. be your own internal documentID in your system |

Postman: 

![](../../.gitbook/assets/postman-sign-create1.PNG)

**Step 5:**

When we have inserted the example body in Postman, we are almost ready for making an example call, but we need to get an authentication token so that the API recognizes us as a valid client. Postman handles OAuth2 for us, so we browse to the Authorization tab, and press "Get access token". Now we input the credentials that we got in step 1, using the client\_credentials grant type and the required scopes \(`document_read document_write document_file`\).

![](../../.gitbook/assets/quickstart_token.png)

When we have got our access token above, we add it to the headers of the request, and press "Send":

![](../../.gitbook/assets/postman_sign4.png)

In the response you should for each of the signers get a URL for the signer to use. Note that this URL by default is only valid for 7 days, but can be controlled via parameter settings:

![](../../.gitbook/assets/postman-sign12.PNG)

**Step 6:**

Enter the URL received in the response object \(under signers\[0\].url\). Since we specified redirect mode in step 4, the API assumes a full page mode in the GUI:

![](../../.gitbook/assets/postman-sign7.png)

To sign the document, you can use the BankID test user "John Doe", who has social security number 05128938534, one time code "otp" and personal password "qwer1234". After successful signature with these credentials, we can see that we are redirected to the landing page that we specified in step 4, and we are done \(see screenshots below\):

![](../../.gitbook/assets/postman-sign8.PNG)

![](../../.gitbook/assets/postman-sign9.PNG)

![](../../.gitbook/assets/postman-sign10.PNG)

![](../../.gitbook/assets/postman-sign11.PNG)

**Step 7:**

Now when we have signed the example text, we might be interested in getting the signed file back. Since we signed a text file in this example, there is no signed PDF format, but rather the raw cryptographic format, in this case a BankID SDO file. If we had signed a PDF document, there would also be a PAdES PDF \(signed PDF\) format, but for simplicity we used a simple text here. For more examples, also with PDF documents, please see the [examples and tutorials]() section and the packaging section.

To get the signed SDO file back, we now use a GET call to the Files API, where we use the documentId that we got back in the response in step 5. In Postman, go to the Files section and choose the Files\_Get call. Get a new OAuth2 token and insert the documentId you got in step 5, as well as appending the query parameter fileFormat with value "native" at the end of the request URL, indicating that we want to fetch the native cryptographic signature from the signing mechanism. Then press send:

![](../../.gitbook/assets/postman-sign12%20%281%29.PNG)

As we can see in the response, we have the native cryptographic signature for the chosen signature mechanism, in this case a BankID SDO file, which is an XML structure that contains the signature and some other data like the signer's certificate. If we were using another signature mechanism, the native signature format would be different, so this is just an example. If we want, we can also check the validity of the signature by going to BankID's signature validator [here ](https://www.bankid.no/privat/los-mitt-bankid-problem/les-signerte-dokumenter/)and uploading the response that we got in the API call above. First we need to copy the XML contents into a text file, and then give it an .sdo extension before we upload it into the validator. Also, make sure that all of the response contents are included in the file - we can for instance call the file test.sdo , and then upload it into the validator:

![](../../.gitbook/assets/postman-sign13.PNG)

As we can see, that validator confirms that the signature is valid, and that it is indeed "John Doe", our test user, who has signed the document. It is also possible to view the original document that was signed by pressing "View document", and we can confirm that it is the text that we submitted for signing in step 4:

![](../../.gitbook/assets/postman-sign14.PNG)

You can also test right away how this plays out in Node:

If you have tried the above code example in Runkit and completed signing the document with the test user, you can try to download the signed version of the document using the documentId that was outputted in the Runkit console. Copy the documentId from the output console and paste it into the variable documentId below \(remember that you must first complete the signature process in the signing link in order to get a signed file back\):

