# Live Project

## Introduction
For the past 2 weeks while attending the Tech Academy, I participated in the Live Project course were I worked with fellow students on a Django Web Application. While the Web Application was already constructed, we were tasked with implimenting new apps and updating existing apps. While some of these new applications were adding new features to the website for the ease of the consumers use, other existing apps needed to be fixed and updated. Over the course of this Live Project sprint, we got to see how it is to work as a team with fellow developers and work on the storyboards to make the ease of the work load more manageable and improve our [skills](#skills-learned-from-course) as developers. 

## Stories
* [Restaurant Application](#restaurant-application)
* [Hotel Search cleanup](#hotel-search-cleanup)

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

*Jump to: [Page Top](#live-project), [Restaurant Application](#restaurant-application), [Hotel Search cleanup](#hotel-search-cleanup)*


## [skills](#skills-learned-from-course)
Overall, for the duration of the Live Project course for Python, it was great to see how things flowed while working as a group. It was really great to see other classmates projects take shape as pushes were made using Git. I felt that I could see how others projects were starting to develop as we would discuss through our daily stand-ups what we did the prior day and what we planned to accomplish for the current day and also what roadblocks we were hitting as developers so that we could get our issues resolved.

Through learning how to troubleshoot our problems and only reaching out if we were truly stuck on a problem, I felt it taught us how to be a better developer by troubleshootin gour own issues and really leveraging websites and forums for help and to give us a sense that just because we are junior developers, we are not alone in the world of coding and that everyone runs into issues, but that there is a wealth of knowledge available on the internet and websites like stackoverflow are an amazing resource for debugging code and to connect with the development community.
