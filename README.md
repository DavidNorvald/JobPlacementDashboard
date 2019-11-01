# Live Project

## Introduction
For my first 2 week sprint at the Tech Academy, I participated in the Live Project course for Python were I worked with fellow students on a Django Web Application. While the Web Application was already constructed, we were tasked with implimenting new apps and updating existing apps. While some of these new applications were adding new features to the website for the ease of the consumers use, other existing apps needed to be fixed and updated. Over the course of this Live Project sprint, we got to see how it is to work as a team with fellow developers and work on the storyboards to make the ease of the work load more manageable and improve our [skills](#skills-learned-from-course) as developers. 


## Python Stories
* [Restaurant Application](#restaurant-application)
* [Hotel Search cleanup](#hotel-search-cleanup)

## C# Stories
* [Preloader](#preloader)
* [Save Button Bug](#save-button-bug)

## Front-End Stories
* [Site Tour](#site-tour)

## Back-End Stories
* [Anchor Button Cleanup](#anchor-button-cleanup)
* [JobSite directions](#jobsite-directions)

## Skills learned
* [Skills](#skills-learned-from-course)

## Summary of Live Project Course
* [Summary of Live Project](#summary-of-live-project)


### Restaurant Application
I worked on implimenting a "search by cuisine type" feature to the Restaurant Application along side the City and Sate search feature already in place. The application used the Zomato API, so I had to research how to impliment the URL parameters to use the existing search results to include the resulting cuisine ID numbers that the Zomato API uses in its search query. The returning results from the API needed to be parsed through to only retrieve the cuisine name and cuisine ID, and then make sure that the cuisine name matched what the user input into the form fields on the web application.


         # getting Cuisine ID
        url = "https://developers.zomato.com/api/v2.1/cuisines?"

        urlParam = {'city_id': cityId} # using cityId from above to plug into 'city_id' in url search for cuisineId below

        url = url + urllib.parse.urlencode(urlParam)

        header = {"User-agent": "curl/7.43.0", "Accept": "application/json", "user_key": "932f5d060ad2588af1928fcef440282a"}

        #Receiving json response and extracting the data    
        results = requests.get(url, headers=header)
        
        jsonResults = results.json() # storing the results
        
        lenJsonDicts = len(jsonResults["cuisines"]) # Storing the json dictionary of main search result response body
        
        # searching for the correct cuisine ID
        cuisineId = 0 # initial cuisineId before iterations, same as above for cities
        count = 0
        msg =''
        while  count < lenJsonDicts:
            cuisinetype = jsonResults["cuisines"][count]# grabbing main response body from above to sift through

            cuisineName = cuisinetype["cuisine"] # grabs the cuisine name from 'cuisine sub-section'

            if cuisineName["cuisine_name"] == userCuisine: # checks the 'cuisine_name' sub-section of search result against the user input of cuisine preference
                cuisineId = cuisineName["cuisine_id"] #getting the cuisine id
            else:
                msg = 'No cuisine type match for the location. Please refine search.' #message to display if no city/cuisine match
            count +=1
       
       
Along with implimenting the new search feature, the Restaurant Application needed some cleanup on the front end so that it would not return plain forms to the user.


    #RestWrapper {
        display: flex;
        justify-content: space-between;
    }

    #RestTitle {
        font-family: "Give You Glory", cursive;
        font-size: 3.7em;
        font-weight: 700;
        position: relative;
        bottom: 25px;
        color: #000;
        text-align: center;
    }

    #RestTitle2 {
        position: relative;
        bottom: 25px;
        color: #000;
        text-align: center;
    }

    #RestInputFormContainer,
    #RestSearchResult {
        display: block;  
        position: relative;
        font-weight: bold;
        text-align: center;
        /* For a 3D raised effect */
        line-height: 1em;
        margin: 1em 2em;
        border: 6px outset #9a9;
        box-shadow: 5px 5px 15px rgba(0,0,0,0.4);
        -webkit-box-shadow: 5px 5px 15px rgba(0,0,0,0.4);
        -moz-box-shadow: 5px 5px 15px rgba(0,0,0,0.4);
    }

    #ZomatoWidget {
        position: -webkit-sticky;
        position: sticky;
        top: 0;
    }

    .RestSteps-item {
        display: inline-block;
        margin: 0 33px 0;
        position: relative;
    }

    .banner-form {
        margin-bottom: 60px;
        display: block;
        margin-top: 20px;
    }

    .RestSteps-item h4 {
        font-family: "Give You Glory", cursive;
        font-weight: bold;
        color: #000;
        font-size: 21px;
    }

    .RestSteps-item h4 span {
        color: #f30;
    }


### Hotel Search cleanup
Along with updating the Restaurant Applications search features, I also worked on cleaning up the Hotel Search Application to be more user friendly. Over the course of this project, I cleaned up the user interface and styled the front end portion of the application so the user could read the data being returned fromt eh search. I kept the data returned to a minimun and added a modal that housed additional info when clicked. 


  {% block search_results %}
  <div id="results">
      {% for result in results %}
      <div id="HotelResults" class="container results_container"> <!-- Main results container -->
          <p><b>Name:</b> {{result.hotel.name}}</p> <!-- Hotel Name -->
          <p><b>Rating:</b> {{result.hotel.rating}}-star rating</p> <!-- Hotel Rating -->
          <p><b>Price:</b> {{result.offers.0.price.total}} {{result.offers.0.price.currency}} per night</p> <!-- Hotel Price/currency type -->
          <button type="button" class="btn btn-primary" data-toggle="modal" data-target=".bd-example-modal-lg">Details</button> <!-- Button to invoke modal -->

          <div class="modal fade bd-example-modal-lg" tabindex="-1" role="dialog" aria-labelledby="myLargeModalLabel" aria-hidden="true"> <!-- The modal -->
              <div class="modal-dialog modal-lg">
                  <div class="modal-content"> <!-- Modal content to display once clicked -->
                      <div class="modal-header">
                          <h4 class="modal-title">Additional Detail</h4>
                      </div>
                          <!--  <span class="close">&times;</span> Close button -->
                          <!-- Content inside the modal -->
                      <div class="modal-body">
                          <p><b>Phone:</b>{{result.hotel.contact.phone}}</p> <!-- Phone Number -->
                          <p><b>Address:</b>{{result.hotel.address.lines}}, {{result.hotel.address.cityName}}, {{result.hotel.address.stateCode}}. {{result.hotel.address.postalCode}}</p> <!-- Hotel Address -->
                          <p><b>Country:</b>{{result.hotel.address.countryCode}}</p> <!-- Country -->
                          <p><b>Description:</b>{{result.hotel.description.text}}</p> <!-- Description about the Hotel -->
                      </div>
                      <div class="modal-footer">
                          <button type="button" class="btn btn-primary" data-dismiss="modal">Close</button>
                      </div>
                  </div>
              </div>
          </div>
      </div>
  {% endfor %}
  </div>
  {% endblock %}
  
  
The CSS stylings for the Hotal Search were kept in line with the overall stylings of the Web Application as a whole so that everything looked like it fit together.
  

    #HotelDestination {
        font-size: 1.5em;
        font-weight: 700;
        color: #000;
        text-shadow: 5px 5px 15px rgba(0,0,0,0.4);
    }

    form .row{
        justify-content: center;
        margin-top: 30px;
        margin-bottom: 30px;
    }

    .HotelSelection {
        text-align:center;
        list-style-position: inside;
        font-size: 18px;
        font: bold;
        font-size: 1.5em;
        font-weight: 700;
    }

    #HotelResults {
        overflow: auto;
        padding: 30px 50px;
        margin: 30px;
        background-color: azure;
        border: 6px outset #9a9;
        box-shadow: 5px 5px 15px rgba(0,0,0,0.4);
        -webkit-box-shadow: 5px 5px 15px rgba(0,0,0,0.4);
        -moz-box-shadow: 5px 5px 15px rgba(0,0,0,0.4);
    }

    .modal-backdrop {
        opacity:0.5 !important;
        position: absolute !important;
    }

    .modal-header,
    .modal-footer {
        background-color: #17a2b8;
    }

    .modal-body {
        background-color: azure;
    }


### Preloader
While working on the C# Live Project, I had to impliment a Preloader gif to the button classes. When clicked upon, the preloader gif would trigger, displaying the gif to show the user that the page was loading.
Below is teh code snip of the Preloader script. It starts by targeting the .btnSubmit class, and when the button is clicked, it shows the loading gif of #divLoader by targeting its id. The gif appears when submitting data using the POST method so the user knows that their data is being posted to the db.

         // PRELOADER script
         $(".btnSubmit").click(function () {
             $("#divLoader").show();
             $.ajax({
                 type: "POST",
                 url: '',
                 dataType: "",
                 contentType: 'application/json; charset=utf-8',
                 alert: "Submitted Successfully",
                 data: {},

                 success: function (data) {
                     $("#divLoader").hide();
                     //alert(data);
                 },
                 error: function (xhr) {
                     $("#divLoader").hide();
                     //alert('error');
                 }
             })
         });


### Save Button Bug
The next story that I worked on in the C# Live Project was fixing the save button on the Edit section of the Jobs view. When the user tried to update a pre-existing job, the changes would not be recorded in the db and when the user clicked the save button, nothing would happen. There was a conflicting script on the Jobs/Edit view that was causing the button to not function, soa fter removing the script that did not do anything anymore, the button started functioning, but would only save some of the changes and not all of them.
I had to edit the controller to include the changes made to the jobType, Notes, Active, and WeeklyShifts tables, so those changes would be updated in the db whent he user hit the Save button. Below is the code for the Jobs Controller for the Save button to fully function.

The below method uses the TryUpdateModel method and targets the job type, notes, active, and weekly shifts fields specifically as the other fields are updated in the prior methods of the Controller.

            if (TryUpdateModel(job, "",
                new string[] { "JobType", "Notes", "Active", "WeeklyShifts" }))
            {
                try
                {
                    job.Location = db.JobSites.Find(Int32.Parse(LocationId));           // used to find location site in db by ID so if changed, it will be saved
                    job.Manager = db.Users.Find(ManagerId);                             // used to find manager from db by ID so if changed, it will be saved
                    db.SaveChanges();
                    return RedirectToAction("Index");
                }
                catch (DataMisalignedException)
                {
                    ModelState.AddModelError("", "Unable to save changes.Try again, and if the problem persists, see your system                             administrator.");
                }
            }
            

### Site Tour
For my front-end design sprint, I chose the story that implimented a site tour for the whole project to guide users around the site, showing them the different features that are available. The first attempt at the site tour was done with Bootstrap Tour. After a lot of research about how to implement the tour on the project, we found out that the current version of Bootstrap4 does not work with Bootstrap Tour as it has not been updated to reflect the new changes. Bootstrap Tour is a very fun way to guide a user around your site and is overall a very clean approach to a site tour, but it was just not compatible with the Live Project's other features as it is dependent on tooltip.js and popover.js for it to work. 
After many failed attempts on trying to get it working (I tried both the full version and the minified versions of Bootstrap Tour to no success) it was discussed at trying to use another site tour plugin. I ended up going with intro.js as it did not have any dependencies that would conflict with anything else on the Live Project. Intro.js is also a very clean approach at guiding your users around your site and has a lot of its features built into its own JavaScript and CSS files, making it easier to implement. There were a few bugs when trying to get the site tour to work, like not covering up the element that the tour was trying to show the user, but was able to resolve that issue by reading about another persons experience with the same thing and the fix to get intro.js to not cover up teh elements. I also styled the tooltip containers to reflect the color pallete of teh Live Project, so that it looked like it fit in with the rest of the project. The tour was started when the user would click on the site tour button that was added to the navigation bar upon clicking it.

         <script type="text/javascript">
             function startIntro() {
                 var intro = introJs();
                 intro.setOptions({
                     showStepNumbers: false,
                     steps: [
                         {
                             element: '#step1',
                             intro: 'This is the Nav Bar. This is what you use to navigate the site and get to the many sections that make up this webpage.'
                         },
                         {
                             element: '#step2',
                             intro: 'This is the Users section. From here, you can see all users.',
                             position: 'right'
                         },
                         {
                             element: '#step3',
                             intro: 'This is the Jobs section. This is where you can create new jobs and see what is going on right now.',
                             position: 'right'
                         },
                         {
                             element: '#step4',
                             intro: 'This is the Job Sites section. This is a list of the current sites for jobs. You can create new job sites here.',
                             position: 'right'
                         },
                         {
                             element: '#step5',
                             intro: 'This is the Schedule section. You can see current schedules for employees. This is also where you go to request time off.',
                             position: 'right'
                         },
                         {
                             element: '#step6',
                             intro: 'This is the company news section. This is where you will find all the latest company postings.',
                             position: 'right'
                         },
                         {
                             element: '#step7',
                             intro: 'This is the company chat module. You can use this to talk to other members of the company.',
                             position: 'right'
                         }
                     ]
                 });
        intro.start();
    }
</script>

I also had to make sure that the site tour button was not able to be triggered while on any of the other pages of the Live Project, with the exception of the dashboard. To achieve this I had to make the button show on the dashboard, but hidden on all other sites by using an if else statement. Below is the code to achieve that functionality.

         <!-- hides Site Tour button on all pages but dashboard -->
         <script type="text/javascript">
         $(function(){
               if (window.location.pathname == "/Home/Dashboard") {
                     $('#Site-Tour').show();
               } else {
                     $('#Site-Tour').hide();
               }
             });
         </script>
         

### Anchor Button Cleanup
For one of the back-end stories, I had to cleanup the Anchor Buttons that were implimented in a prior story and change over all the buttons on the various pages to reflect the new styling of the Anchor Buttons. The buttons that needed to be changed looked like this:

         @Html.ActionLink("Back to List", "Index")
         
and needed to be changed to this:

         @Html.Partial(AnchorButtonGroupHelper.PartialView, AnchorButtonGroupHelper.GetBack())


### JobSite directions
For my back-end story, I was tasked with implementing the addition of directions to the Job Site locations through the Leaflet map that was already deployed in the project. I had to first get Leaflet-Routing-machine added to the project along with the appropriate files to help the plugin function, like the addition of a Geocoder for address verification. For the project, I used Nominatim, as it was what was recommended on the documentation for Leaflet-Routing-Machine.
Through the addition of the plugin, I was able to add a popup on the Leaflet map that the user could input starting and ending locations for directions, it also could be minimized if the user wanted to look at the map in greater detail. The API's documentation also allowed for the direction route to be draggable, in case the user wanted to take another direction instead of what Leaflet-Routing-Machine recommended, and would update the directions on screen to match the new route in real time. 

         //Creates a leaflet map to display on the user's screen
                 function setLeafletMap(mapId, lat, long, popupText) {
                     var initialZoom = 13;

            var apiToken = "pk.eyJ1IjoiYXMtdHRhY3NscCIsImEiOiJjanlwMmxhc3UwMTlzM2hxcTBjaDN2MHE2In0.CQq8cphDoOacxDkn7sdNtg";

            var mapboxRequest = "https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token=";

            var leafletMap = L.map(mapId).setView([lat, long], initialZoom);

            L.tileLayer(mapboxRequest + apiToken, {
                maxZoom: 18,
                attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, ' +
                    '<a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, ' +
                    'Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
                id: 'mapbox.streets'
            }).addTo(leafletMap);
            
            // creates button for start and ending waypoints for user's input
            function button(label, container) {
                var btn = L.DomUtil.create('button', '', container);
                btn.setAttribute('type', 'button');
                btn.innerHTML = label;vg
                return btn;
            }
            
            // Map Marker to display on user's screen
            var leafletMarker = L.marker([lat, long]).addTo(leafletMap);

            if (popupText != "") {
                leafletMarker.bindPopup(popupText).openPopup();
            }

            // set Waypoints for the map to be null values, can hardcode in with coordinates in place of null values
            // "geocoder" verifies address is valid, otherwise will default to a centered radius for the city
            var control = L.Routing.control({
                waypoints: [
                    L.latLng(null),
                    L.latLng(null)
                ],
                routeWhileDragging: true,
                show: true,
                geocoder: L.Control.Geocoder.nominatim(),
                autoRoute: true
            }).addTo(leafletMap);
            // adds the container that houses the start and ending input fields
            leafletMap.on('click', function (e) {
                var container = L.DomUtil.create('div'),
                    startBtn = button('Start from this location', container),
                    destBtn = button('Go to this location', container);
                // two different ways for gathering waypoints, can be used with getWaypoints API option as well
                L.DomEvent.on(startBtn, 'click', function () {
                    control.spliceWaypoints(0, 1, e.latlng);
                    map.closePopup();
                });

                L.DomEvent.on(destBtn, 'click', function () {
                    control.spliceWaypoints(control.getWaypoints().length - 1, 1, e.latlng);
                    map.closePopup();
                });

                L.popup().setContent(container).setLatLng(e.latlng).openOn(leafletMap);
            });
        }
       
        // renders something useful if the map is invalid
        function setInvalidMap() {
            var map = document.getElementById("jobSiteMap");
            map.classList.add("invalidMap");
            map.innerHTML = "<div>Invalid Address</div>";
        };


*Jump to: [Page Top](#live-project), [Restaurant Application](#restaurant-application), [Hotel Search cleanup](#hotel-search-cleanup), [Preloader](#preloader), [Save Button Bug](#save-button-bug), [Site Tour](#site-tour), [Anchor Button Cleanup](#anchor-button-cleanup), [JobSite directions](#jobsite-directions)*


### Skills learned from course
Overall, for the duration of the Live Project course for Python, it was great to see how things flowed while working as a group. It was really great to see other classmates projects take shape as pushes were made using Git. I felt that I could see how others projects were starting to develop as we would discuss through our daily stand-ups what we did the prior day and what we planned to accomplish for the current day and also what roadblocks we were hitting as developers so that we could get our issues resolved.



### Summary of Live Project
Through learning how to troubleshoot our problems and only reaching out if we were truly stuck on a problem, I felt it taught us how to be a better developer by troubleshootin gour own issues and really leveraging websites and forums for help and to give us a sense that just because we are junior developers, we are not alone in the world of coding and that everyone runs into issues, but that there is a wealth of knowledge available on the internet and websites like stackoverflow are an amazing resource for debugging code and to connect with the development community.
