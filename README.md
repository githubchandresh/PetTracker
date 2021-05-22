# PetTracker

In simple words, this is for tracking of pets. One has to purchase a :
(1) cellular transmitting device and affix it to the pet’s collar. 
(2) Subscription plan from a service provider to track the signals (and hence the location of the pet). 
It’s a use-case for IOT (Internet of Things).

Once the locations are stored in the database, a user can at-will request for location history of the pet.
A subscription allows to determine how long back in the past can a user source the location history. Such as -
(1) Basic subscription: 24 hours old data
(2) Premium subscription: 30 days old data

The presentation layer here would be built in the mobile app. Inputs via the app could be captured via JQuery in the JSON format and relayed to the server (Expressjs). Expressjs could capture the input, html methods (PUT, POST, GET etc) and provide the route to perform I/O operations to/from the data layer (mongoDB).

It is suggested that the following be used:
a)	The customer would use the mobile app to create his/her profile, select subscription, create the pet(s) profile. These 3 activities from the presentation tier should exchange the data with the data tier preferably in JSON format. The logic tier will consist of the APIs.
b)	Store the ‘Effective Start Date’ and ‘Effective End Date’ in Customer, Subscription and Pet(s) profiles to maintain historical data based on updates to them.
c)	Location History is not created/updated via the mobile app. It’s the information which would be updated based on the ping signal transmitted from the pet’s collar (cellular transmitter) to the cellular tower(s).
d)	Opt-In consent, which a user agrees for specifically, is for allowing to be contacted by fellow pet owners within a 5-mile radius. This is a premium subscription. Hence it is not stored at customer level, but is stored at Subscription level.

API
These are the six API’s suggested:
1.	Manage_Customer_Profile
a)	This API is to Create, Update or Inactivate customer profile.
2.	Manage_Customer_Subscription
a)	This API is to Create, Update or Inactivate subscription.
3.	Manage_Pet_Profile
a)	This API is to Create, Update or Inactivate profile of the pet(s) wrt to the customer.
4.	Store_Location_Data
a)	This API is to Update location coordinates of the pet at regular intervals.
b)	Since the locations are to be sent from the pet’s collar, it will be integrated into/with the cellular transmitter on it. This API cannot be integrated to a customer’s app.
c)	Assumptions: transmitters attached to the pet collar sends an update every 5 minutes to the cell towers. Advice to keep this configurable.
5.	Return_Location_Data
a)	This API is to Get location data for a given pet of a customer
b)	If the customer’s subscription is:
i.	Basic: then return last 24 hours of data.
ii.	Premium: then return last 30 days of data.
c)	Assumptions: Irrespective of the number of pet(s) a customer has registered for this service, the same subscription applies to all the pets. This is because the subscription is at the customer’s level, not at the pet’s level.
6.	Provide_Other_PetOwners_Contact
a)	This API is only for customers with Premium subscription. 
b)	It will take the customer’s zip code/geocode to identify and create a set of all the possible zip codes/geocodes that fall within its 5-mile radius. 
c)	Using this set, it will then identify all the other customers on the platform who have ‘opted-in’ (to be contacted by other pet-owners). Then it will fetch their contact information and return the result.
d)	Assumptions: If the returned dataset is, say more than 3, then there should be a provision to limit it to 3 (configurable) and a click link to navigate to the next page. This will be handled by the presentation layer though, not by the API.  
