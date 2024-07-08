# Protocol change history
## v26.0.0

*   Introduced **[Roup](?p=/chat-schemas#chat-room-update-roup)** and **[Dubroup](?p=/transaction-schemas#chat-room-update-dubroup)** entities to describe announcement messages about chat room updates (see also **[Payp](?p=/chat-schemas#message-payload-payp)** and **[Dubpayp](?p=/transaction-schemas#message-payload-dubpayp)**). Announcements can also be muted, check the [PUT chat room](?p=/chat-api#put-chat-room) section for the details.
*   Introduced **[chedit](?p=/user-api#roles)** and **[chadmin](?p=/user-api#roles)** roles (see **[Roles](?p=/user-schemas#user-roles-roles)**, **[Usered](?p=/user-schemas#user-response-usered)**, **[Usereu](?p=/user-schemas#user-update-usereu)** and **[Dubuser](?p=/transaction-schemas#user-dubuser)**).
*   Added **[Usermeta](?p=/user-schemas#user-description-usermeta)** to **[Boma](?p=/chat-schemas#chat-room-member-boma)** and **[Dubusermeta](?p=/transaction-schemas#user-description-dubusermeta)** to **[Dubboma](?p=/transaction-schemas#chat-room-member-dubboma)** and **[Dubbomath](?p=/transaction-schemas#chat-room-message-author-dubbomath)**.
*   Chat rooms now belong to organization units (see **[Roomeu](?p=/chat-schemas#chat-room-update-roomeu)**, **[Roomed](?p=/chat-schemas#chat-room-roomed)** and **[Dubroom](?p=/transaction-schemas#chat-room-dubroom)**).
*   Members of chat rooms can be muted (see **[Boma](?p=/chat-schemas#chat-room-member-boma)**, **[Bomaeu](?p=/chat-schemas#chat-room-member-update-bomaeu)** and **[Dubboma](?p=/transaction-schemas#chat-room-member-dubboma)**).
*   Dropped **rguserxtid** field from **[Roomeu](?p=/chat-schemas#chat-room-update-roomeu)**, introduced **rgboma** instead.
*   From now on, users can be members of unlimited chat rooms. For further details, see [Quotas and limits](?p=/chat-api#quotas-and-limits) in chapter [PUT chat room](?p=/chat-api#put-chat-room).

## v25.0.0

*   Added **[Dboxed](?p=/document-storage-schemas#document-storage-contents-dboxed)**, **[Dboxit](?p=/document-storage-schemas#document-storage-item-dboxit)**, **[Dboxsyc](?p=/document-storage-schemas#document-storage-item-delivery-receipt-dboxsyc)** and **[Dboxsub](?p=/document-storage-schemas#document-storage-subscription-dboxsub)** entities to represent driver document storage data.
*   Added [GET document storage contents](?p=/document-storage-api#get-document-storage-contents), [PUT document](?p=/document-storage-api#put-document), [PUT folder](?p=/document-storage-api#put-folder), [DELETE document or folder](?p=/document-storage-api#delete-document-or-folder) and [GET document](?p=/document-storage-api#get-document) requests to manage user document storage data.
*   Added **[Evjson](?p=/custom-menu-schemas#evjson)** syntax sugar to make passing json-formatted string parameters to external applications simpler.
*   Added **[Notd](?p=/custom-menu-schemas#custom-notification-notd)** to allow sending custom notifications and badges to users.

## v24.0.0

*   Added **[roomlist](?p=/custom-menu-schemas#navigate-to-list-of-chat-rooms-roomlist)** [widget action](?p=/custom-menu-schemas#widget-action-buta) for [customizable screens](?p=/custom-menu-schemas#custom-menu-scren).
*   Added **[Dubroom](?p=/transaction-schemas#chat-room-dubroom)**, **[Rovered](?p=/chat-schemas#chat-room-version-rovered)**, **[Dubpost](?p=/transaction-schemas#chat-message-dubpost)**, **[Dubpayp](?p=/transaction-schemas#message-payload-dubpayp)**, **[Dubboma](?p=/transaction-schemas#chat-room-member-dubboma)** and **[Dubbomath](?p=/transaction-schemas#chat-room-message-author-dubbomath)** entities to represent updates in a chatroom.
*   Added [GET chat room](?p=/chat-api#get-chat-room) and [PUT chat room](?p=/chat-api#put-chat-room) requests to retrieve, create and update chat rooms.
*   Added [PUT message](?p=/chat-api#put-message) request to post new message to a chat room.
*   Added [PUT delivery receipt](?p=/chat-api#put-delivery-receipt) request to update the delivery receipt of a member in a chat room.
*   Added [PUT read receipt](?p=/chat-api#put-read-receipt) request to update the read receipt of a member in a chat room.
*   Added **[Roomeu](?p=/chat-schemas#chat-room-update-roomeu)** and **[Roomed](?p=/chat-schemas#chat-room-roomed)** entities to represent chat rooms.
*   Added **[Boma](?p=/chat-schemas#chat-room-member-boma)** entity to represent members of a chat room.
*   Added **[Wetagdtu](?p=/chat-schemas#version-with-date-and-time-wetagdtu)** entity to represent a delivery/read receipt of a member in a chat room.
*   Added **[Posteu](?p=/chat-schemas#new-message-entity-posteu)** and **[Posted](?p=/chat-schemas#message-update-result-posted)** entities to represent a message in a chat room.
*   Added **[Payp](?p=/chat-schemas#message-payload-payp)** entity to represent the payload of a message.

## v22.0.0

*   Added **[Triped](?p=/trip-schemas#trip-response-triped)**.**ruts** and **[Dubtrip](?p=/transaction-schemas#trip-dubtrip).ruts** fields that represent trip attachment storage.
*   Added [Ruts](?p=/trip-schemas#trip-attachment-storage-ruts)/[Dubruts](?p=/transaction-schemas#trip-attachment-storage-dubruts) and [Rut](?p=/trip-schemas#trip-attachment-rut)/[Dubrut](?p=/transaction-schemas#trip-attachment-dubrut) entities to represent trip attachment storage and documents stored in it.
*   Added [PUT rut](?p=/trip-api#put-rut) request to upload documents to trip attachment storage.
*   Added [DELETE rut](?p=/trip-api#delete-rut) request to delete documents from trip attachment storage.
*   Added [GET rut](?p=/trip-api#get-rut) request to retrieve documents from trip attachment storage.
*   Updated trip [Quotas and limits](?p=/trip-api#quotas-and-limits) with limits imposed by the trip attachment storage.
*   Added **[Triped](?p=/trip-schemas#trip-response-triped)**.**cufis**, **[Tripeu](?p=/trip-schemas#trip-update-tripeu)**.**cufis**, **[Dubtrip](?p=/transaction-schemas#trip-dubtrip).cufis** and **[Stan](?p=/trip-schemas#station-stan).cufis** fields to represent custom fields of trips or stations of trips.
*   Added [PUT scren](?p=/custom-menu-api#put-scren) request to change the custom menu of a user.
*   Added [GET scren](?p=/custom-menu-api#get-scren) request to download the custom menu of a user.
*   Added [DELETE scren](?p=/custom-menu-api#delete-scren) request to remove the custom menu of a user.
*   Added documentation for [Scren](?p=/custom-menu-schemas#custom-menu-scren) entity.
*   Updated trip [Station types (Kstan)](?p=/trip-schemas#station-types-kstan) introducing new types of trip stations.

## v21.0.0

*   Document reviews and the **[Dubdosu](?p=/transaction-schemas#document-dubdosu).oredosu** field that used to represent these reviews has been deprecated in favor of individual document image reviews ([Dubredoim](?p=/transaction-schemas#document-image-review-dubredoim)). Document reviews will no longer be returned from the [GET unprocessed updates](?p=/transaction-api#get-unprocessed-updates) API operation and the value of **Dubdosu.oredosu** will be empty.
*   Added the **Dubimg.oredoim** field to represent document image reviews. See also [Dubimg](?p=/transaction-schemas#document-image-dubdosuimg).
*   Added the **Dubimg.odoed** field to represent edits to a document image. See also [Dubimg](?p=/transaction-schemas#document-image-dubdosuimg).