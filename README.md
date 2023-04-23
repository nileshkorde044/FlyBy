## Info 

Publish the subgraph schemas to Apollo Studio
1. Run `rover config auth` to set the `APOLLO_KEY` value 
1. Navigate to `/subgraph-locations`.
1. Run `rover subgraph publish <APOLLO_GRAPH_REF> --name locations --schema locations.graphql --routing-url http://localhost:4001`
1. Navigate to `/subgraph-reviews`.
1. Run `rover subgraph publish <APOLLO_GRAPH_REF> --name reviews --schema locations.reviews --routing-url http://localhost:4001`


1. Open a new terminal window, and navigate to `/router`.
1. Run `APOLLO_KEY=<APOLLO_KEY> APOLLO_GRAPH_REF=<APOLLO_GRAPH_REF> ./router --config config.yaml`. This will start the router at `http://127.0.0.1:4000/` and allow access from `localhost:3000` for the client.
1. Open another new terminal window, and navigate to `/subgraph-locations`.
1. Run `npm install && npm start` again. This will install all packages for the `locations` subgraph, then start the subgraph at `http://localhost:4001`.
1. Open a third new terminal window, and navigate to `/subgraph-reviews`.
1. Run `npm install && npm start` again. This will install all packages for the `reviews` subgraph, then start the subgraph at `http://localhost:4002`.
1. In a web browser, open Apollo Studio Sandbox for `http://localhost:4000`. You should be able to run queries against your gateway server. Some test queries are included in the following section.

### Queries

1. Get all locations for the homepage.

   ```graphql
   query getAllLocations {
     locations {
       id
       name
       photo
       description
       overallRating
     }
   }
   ```

1. Get the latest reviews for the homepage.

   ```graphql
   query LatestReviews {
     latestReviews {
       comment
       rating
       location {
         name
         description
       }
     }
   }
   ```

1. Get details for a specific location.

   ```graphql
   query getLocationDetails {
     location(id: "loc-1") {
       id
       name
       description
       photo
       overallRating
       reviews {
         comment
         rating
       }
     }
   }
   ```

1. Submit a review for a location.
   ```graphql
   mutation submitReview {
     submitReview(review: { comment: "Wow, such a great planet!", rating: 5, locationId: "1" }) {
       code
       success
       message
       review {
         id
         comment
         rating
       }
     }
   }
   ```