

### Pertinent Directions
To use directions in the Google Maps JavaScript API, create an object of type DirectionsService and call DirectionsService.route() to initiate a request to the Directions service, passing it a DirectionsRequest object literal containing the input terms and a callback method to execute upon receipt of the response.

###### Additional Instructions regarding the follow up
Initiating a directions request to the DirectionsService with the route() method requires passing a callback which executes upon completion of the service request. This callback will return a DirectionsResult and a DirectionsStatus code in the response.

```js
$(function(){
  $("#submitRequest").click(function(){
    var theRoute = new DirectionsService();
    theRoute.route( DirectionsRequestObjectLiteral, RouteCallBack(DirectionsResult, DirectionsStatus) );
  });
});

var DirectionsRequestObjectLiteral = {
  origin: 'Chicago, IL',
  destination: 'Los Angeles, CA',
  waypoints: [
    {
      location: 'Joplin, MO',
      stopover: false
    },{
      location: 'Oklahoma City, OK',
      stopover: true
    }],
  provideRouteAlternatives: false,
  travelMode: 'DRIVING',
  unitSystem: google.maps.UnitSystem.IMPERIAL
};

var mapObject; //Is this needed?
function RouteCallBack( DirectionsResult, DirectionsStatus ) {

  if ( DirectionsStatus === "ok") {
      var myOptions = {
        zoom : 16,
        center : DirectionsResult.origin,
        mapTypeId : google.maps.MapTypeId.ROADMAP
      };
      var mapObject = new google.maps.Map(document.getElementById("map"), myOptions);
    }  else {
      console.log(DirectionsStatus);
    }
}
```
