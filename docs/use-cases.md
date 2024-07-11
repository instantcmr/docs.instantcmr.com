# Use cases

In the following section, we will outline several scenarios where integrating your backend with instantCMR would be beneficial. We will start by examining the simplest case and progress to more advanced scenarios, demonstrating the increasing benefits of this integration.

## Receiving document submissions

Drivers scan various documents: CMRs, delivery notes, pallet receipts, photos of accidents or damages. These documents are automatically synchronized with the instantcmr backend. Users with access to the instantcmr hub can review and manually download these.

As your company grows, manual processes can become cumbersome, creating a need for automation. With minimal effort, you can automate the export of these submissions using the instantcmr API.

Here’s how the process works:

1. **Document Submission:** when a new document is submitted, the instantCMR backend creates a [Dubdosu](?p=/transaction-schemas#document-dubdosu) entity. This entity contains metadata about the driver, notes and links to attached images. Dubdosu is then added to a message queue, which has been set up by our backend team for you.

2. **Queue Management:** the entity remains in the queue until your backend makes a poll request to the provided endpoint. The instantcmr backend then retrieves the first few items from the message queue and returns them in the response.

3. **Processing Entries:** your backend should process the entities, download the images, and register them in your system.

4. **Acknowledgment:** after processing, your backend needs to acknowledge the transmission by sending another request to the instantcmr backend. The instantcmr system will then mark the entities as processed and remove them from the message queue.

By following this process, you can efficiently manage document submissions, ensuring that they are seamlessly integrated into your transport management system.


## Trip management

Dealing with many tours and optimizing vehicle usage, processing the flow of incoming documents becomes increasingly challenging. This necessitates a higher level of organization. In instantcmr terminology, *trips* consist of a list of stations that need to be visited by the driver and document requests to be fulfilled. While our hub provides many features to work with data arriving from trips, you cannot currently create them within the UI itself; this functionality is unlocked by implementing deeper integration with our system.

Here’s how you can manage trips through integration:

1. **Define a Trip:** specify the stations to be visited and document requests that need to be submitted by the driver. Attach additional guides, documents, or route plans to the trip.
   
2. **Create the Trip:** your system sends a PUT request to the instantcmr backend to create the [trip](?p=/trip-api) in our system. Instantcmr will synchronize the trip information with the corresponding driver's mobile phone, and the driver will be immediately notified about the tasks to be completed.

3. **Trip Execution:** as the driver follows the steps within the trip, documents continuously flow into the instantcmr backend.

Getting notified about trip updates is similar to the case of handling arbitrary document submissions.

1. **Document Submitted:**
   when the submission arrives, the instantcmr backend creates a [Dubtrip](?p=/transaction-schemas#trip-dubtrip) entity. This entity corresponds to the trip you created and contains the current status of the trip with all the submitted items and metadata. The Dubtrip entity is added to your message queue, the same way as we discussed at arbitrary document submissions above. 
   
   The process is exactly the same when the trip is modified by a hub operator. For example, when he or she fixes a mislabeled submission and associates it with the document request it really belongs to, the trip entity gets updated and a corresponding Dubtrip message is dispatched.

2. **Queue Management:**
   the entity remains in the queue until your backend makes a poll request to the provided endpoint. The instantcmr backend then retrieves the first few items from the message queue and returns them in the response.

3. **Processing Entries:** your backend should process these entities, making updates to the trip in your system, download the images, and register them on your side.

4. **Acknowledgment:** after processing, your backend needs to acknowledge the transmission by sending another request to the instantcmr backend. The instantcmr system will then mark the entities processed and remove them from the message queue.
