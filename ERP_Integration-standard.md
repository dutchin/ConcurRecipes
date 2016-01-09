### Apps for my business
This series of API Recipes describes API tasks associated with Apps for my business. Apps for my business are applications designed for financial integration companies who want to streamline their process of systematically creating journal entries relating to travel and entertainment expenses. The benefits of doing so result in increased compliance with spending guidelines, the control of unnecessary spend and reduction of costs and risk. This API Recipe explains the process for obtaining client expense report data from Concur through Enterprise Resource Planning (ERP) integration.

Before a partner user can obtain expense report data from Concur, ensure that you have completed the following:

- Ensure your app has been certified by Concur
- Ensure your organization has obtained a signed letter of Agreement from a client stating that you can obtain that client's expense report data using the extract file created by Concur's service.
- Ensure your app supports Concur's App Center flow.
- Ensure your app can authenticate with Concur using either [Native Flow](https://developer.concur.com/api-reference/authentication/authentication.html#native) (preferred) or Web Flow
- Ensure your app includes the ability to [refresh the Concur authentication token](https://developer.concur.com/api-reference/authentication/authentication.html#refreshing-access-token)
- Ensure your app includes the ability to respond appropriately to [revoked token messages](https://developer.concur.com/api-reference/authentication/authentication.html#revoke-all-access-tokens)

#### Apps for my business: Standard Edition ERP Integration
Standard Edition ERP Integration assumes that you are working for or on behalf of a Financial Integration Company. Once your app is certified, and you have obtained a signed letter of agreement from a client, you can obtain that client's expense report data through the extract file produced by Concur's service.

API�s used to obtain extract files for your client

Before you begin, you need to have the client close the Payment Manager batch using the appropriate APIs in order for you to retrieve the files using the API below. Ask your client not to close the batch manually through the User Interface.

1. Navigate to [Payment Batches](https://developer.concur.com/api-reference/expense/payment-batch/payment-batches.html).

2. Obtain a list of your client's batches by using [Get list of batches]( https://developer.concur.com/api-reference/expense/payment-batch/payment-batches.html#getpaymentbatches)

   #### Example

    ```
    GET /expense/paymentbatch/v1.1/batch/_{BatchID}_/file
    ```

3. Close the desired batch by using [Close a payment batch]( https://developer.concur.com/api-reference/expense/payment-batch/payment-batches.html#closepaymentbatch)
   #### Example
POST /expense/paymentbatch/v1.2/batch/{BatchID}/close
4. Retrieve the file you want by using [Retrieve a payment batch file](https://developer.concur.com/api-reference/expense/payment-batch/payment-batches.html#getbatchfile)
  #### Example
GET /expense/paymentbatch/v1.1/batch/_{BatchID}_/file

##### Make us better at making your experience easier.
Share a Concur API process issue we can do better. Provide us with an explanation, screen shots and your recommendation [here](http://forum.developer.concur.com/).
