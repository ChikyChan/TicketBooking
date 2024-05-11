# Ticket Booking API design

## View the swagger UI
1. Clone this repository
2. run `npx serve`
3. Launch browser such as `Chrome`, `Microsoft Edge`
4. Enter URL `http://localhost:3000` then you will see the API document in the Swagger UI

## Sample requests and responses
Included in the index.json swagger file, and you should see and review them in the Swagger UI.

## Design Consideration

### API design style
1. There two design style for API: `Restful API` style and `RPC` style, which is `Resource based` design style and `Action based` design style. In this case, I design the API almost in `Restful API` style, and only one endpoint is designed as `RPC` design, I have following considerations:
   1. The requirement of this API is not complex.
   2. All the objects mentioned in the requirement can see them as `Resource`.
   3. Almost all actions are target to resource.
2. Though the API should transfer data between the client side via `https` protocol, a potential `Middle-Man-Attack` may happen, so I designed to hash the password via `SHA512` algorithm while retrieving the access token.
3. As mentioned in the requirement, user can see his own bookings, so authentication is required for the endpoints of the Booking resource.
4. As the user can pay to the booking, and the payment platform has a mechanism about send payment notification in synchronize mode, so needs a endpoint to handle the payment notification request.
5. All unexpected error will log into a error file automatically.
6. For the reason I put all the booking endpoints under `me`, is that user only can see their own bookings, I realized `me` should be a resource and `booking` should be another resource, and `me` should include `booking`. Maybe will have a separate `bookings` resource if have some administration requirement and the administrator can view all booking not only of a specified user.
7. For the endpoint `PUT /me/bookings`, I use http verb `PUT` to present as an create booking action, as the http verb `PUT` means to PUT an entity of the resource to the server, I think it is equals to the create action.
8. For the endpoint `PATCH /me/bookings/{id}`, I use http verb `PATCH` to present as a update booking action, as the http ver `PATCH` means to modify an entity of the resource in the server, I think it is equals to the update action.
9. As the endpoints support pass a booking id to fetch a booking for current user, the system will check the booking with the specified id is belongs to the current authenticated user, if no, the system will respond with `404` status code to tell the API caller this user has no any booking with the specified id, I trust this can avoid the API caller know if the system has this record.
10. The API has not define a base API respond format, as in my design, I hope the status code can cover all cases, and the API caller can determine which response handler they should use according to the status code. And for some specified status code, I can use another response content schema to contains more details about the error to the API caller, then the API caller can use this response to know more details and handle the exception case.