<h2>
            Blue State Digital Trusted API
        </h2>
<div class="major_section">
<h3>
                Overview
            </h3>
        </div>
<div class="major_section">
<h3>
                Common Record Formats
            </h3>
<p>
                Certain types of data are returned by multiple API requests. These record formats are defined below. (<small>note: All times returned by the API will be in the system timezone</small>)
</p>
<h4>
Error messages
            </h4>
<h5>
                Generic Graph API Errors
</h5>
<p>
                Error messages from the graph API will have <code>error</code> and <code>error_description</code> fields in their JSON response. Here is an example of an error caused by accessing a Graph page that requires HTTPS over an insecure connection:
</p>
            <pre>
<samp>
{
   "error":"invalid_request",
   "error_description":"Secure connection required"
}
</samp>
</pre>
            <h5>
                Permission denied
            </h5>
            <p>
                This error is returned when the Graph page requires the requester to be authenticated and they are not, or is for a resource that the authenticated user does not have permissions to.
            </p>
            <pre>
<samp>
{
   "error":"permission_denied",
   "error_description":"You must be logged in to view this information"
}
</samp>
</pre>
            <h5>
                Rate limit exceeded
            </h5>
            <p>
                This error is returned when a user or IP address has exceeded the allowed rate limit for requests.
            </p>
            <pre>
<samp>
{
   "error":"rate_limit_exceeded",
   "error_description":"Too many requests"
}
</samp>
</pre>
            <h5>
                Invalid parameters
            </h5>
            <p>
                This error is returned when the request is missing a required parameter, or one or more of the supplied parameters are invalid.
            </p>
            <pre>
<samp>
{
   "error":"invalid_parameters",
   "error_description":"The path parameter 'foo' is required"
}
</samp>
</pre>
            <h4>
                Exported JSON Circle Records
            </h4>
            <h5>
                Core: <code>circle</code>
            </h5>
            <p>
                The core circle record contains the circle's slug, name, description, circle type, member count, and creation and modified dates:
            </p>
            <pre>
<samp>
{
   "slug":"test slug",
   "name":"test circle #1",
   "description":"description for circle #1",
   "circle_type":0,
   "member_count":1,
   "create_dt":"2008-10-10 11:14:50",
   "modified_dt":"2008-10-11 18:10:13"
}
</samp>
</pre>
        </div>
        <div class="major_section">
            <h3>
                Common Query Parameters
            </h3>
            <p>
                Every Graph API request, unless otherwise noted, supports certain common query parameters.
            </p>
            <h4>
                <code>callback</code>
            </h4>
            <p>
                <code>callback</code> is an optional parameter that specifies the name of a JavaScript function to use as the JSON-P wrapper for the returned data. It implicitly makes the request a JSON-P request.
            </p>
            <p>
                Example:
            </p>
            <pre>
<samp>?callback=my_func</samp>
</pre>
            <pre>
<samp>
my_func({
   "data":[
      {
         "slug":"slug2",
         "name":"test circle #2",
         "description":"description for circle #2",
         "circle_type":0,
         "member_count":5,
         "create_dt":"2008-10-10 11:14:50",
         "modified_dt":"2008-10-11 18:10:13"
      }
   ]
});
</samp>
</pre>
        </div>
        <div class="major_section">
            <h3>
                Graph API Reference
            </h3>
            <h4>
                Circle Graph API
            </h4><!-- /circles/by_zip -->
            <div class="api_method">
                <h5>
                    <code>/circles/by_zip/$zip/$radius</code>
                </h5>
                <div class="description">
                    <p>
                        Lists all circles within a given radius of a given center point, specified as a zip code (U.S. assumed). Results can also be filtered to be in a specific state by passing a <code>state_cd</code> query parameter.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/graph/circles/by_zip/$zip/$radius
                </p>
                <div class="parameters">
                    <p>
                        Path Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>zip</code> (required)
                        </dt>
                        <dd>
                            The U.S. zip code to use as a center for the radius search.
                        </dd>
                        <dt>
                            <code>radius</code> (required)
                        </dt>
                        <dd>
                            The distance to search from the center of <code>zip</code>.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns one <code>circle</code> element (see Common Record Formats) for each circle that matches the radius search. In addition to the core record, each element also contains a <code>distance</code> element for the distance from the center point. The results are sorted by distance from closest to farthest.
                    </p>
                </div>
                <div class="example">
                    <p>
                        Example:
                    </p>
                    <pre>
<samp>
http://XYZ/page/graph/circles/by_zip/12345/20
</samp>
</pre>
                    <pre>
<samp>
{
   "data":[
      {
         "slug":"slug2",
         "distance":1.5,
         "name":"test circle #2",
         "description":"description for circle #2",
         "circle_type":0,
         "member_count":5,
         "create_dt":"2008-10-10 11:14:50",
         "modified_dt":"2008-10-11 18:10:13"
      }
   ]
}
</samp>
</pre>
                </div>
                <div class="errors">
                    <p>
                        Errors:
                    </p>
                    <p>
                        This request can return a <code>rate_limit_exceeded</code> error, or an <code>invalid_parameters</code> error if the zip code is invalid or the zip or radius path parameters are missing.
                    </p>
                </div>
            </div><
            <h4>
                Constituent Graph API
            </h4><!-- /me/circles -->
            <div class="api_method">
                <h5>
                    <code>/me/circles</code>
                </h5>
                <div class="description">
                    <p>
                        Lists all circles that the currently-logged in constituent is a member of. If the requester is not logged in to the BSD Tools, then an access error is returned.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/graph/me/circles
                </p>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns one <code>circle</code> element (see Common Record Formats) for each circle the currently logged in constituent is a member of.
                    </p>
                </div>
                <div class="example">
                    <p>
                        Example:
                    </p>
                    <pre>
<samp>
http://XYZ/page/graph/me/circles
</samp>
</pre>
                    <pre>
<samp>
{
   "data":[
      {
         "slug":"test slug",
         "name":"test circle #1",
         "description":"description for circle #1",
         "circle_type":0,
         "member_count":1,
         "create_dt":"2008-10-10 11:14:50",
         "modified_dt":"2008-10-11 18:10:13"
      },
      {
         "slug":"slug2",
         "name":"test circle #2",
         "description":"description for circle #2",
         "circle_type":0,
         "member_count":5,
         "create_dt":"2008-10-10 11:14:50",
         "modified_dt":"2008-10-11 18:10:13"
      }
   ]
}
</samp>
</pre>
                </div>
                <div class="errors">
                    <p>
                        Errors:
                    </p>
                    <p>
                        This request can return a <code>rate_limit_exceeded</code> error, or a <code>permission_denied</code> error if there is no currently logged-in constituent.
                    </p>
                </div>
            </div>
            <h4>
                Event RSVP Graph API
            </h4>
            <div class="api_method">
                <h5>
                    <code>/graph/addrsvp</code>
                </h5>
                <div class="description">
                    <p>
                        Allows creation or editing/overwriting of an individual RSVP to either a single event or multiple days of a single event, with or without shifts. <code>event_id</code>, <code>will_attend</code>, and either a <code>guid</code> or both an <code>email</code> address and <code>zip</code> are required for all adds.
                    </p>
                    <div class="example">
                        <p>
                            Example with minimal information (guid):
                        </p>
                        <pre>
<samp>
/page/graph/addrsvp?event_id=6&amp;will_attend=1&amp;<strong>guid=HASH</strong>
</samp>
</pre>
                        <p>
                            Alternate example with minimal information (email &amp; zip):
                        </p>
                        <pre>
<samp>
/page/graph/addrsvp?event_id=6&amp;will_attend=1&amp;<strong>email=user@example.com&amp;zip=02210</strong>
</samp>
</pre>
                    </div>
                    <p>
                        Days of a multi-day event are specified by a coma-separated list in the <code>event_id</code> parameter. The api assumes the RSVP-er is not attending any day of a multi-day event not specified in the an RSVP to another day of the event, and automatically sets <code>will_attend</code> to 0 for upspecified days. The event in the example below lasts a full week, but the cons will attend exactly two day of the seven days.
                    </p>
                    <div class="example">
                        <p>
                            Example of multiday event:
                        </p>
                        <pre>
<samp>
/page/graph/addrsvp?<strong>event_id=10,11</strong>&amp;will_attend=1&amp;guid=HASH
</samp>
</pre>
                        <p>
                            Results in:
                        </p>
                        <table>
                            <tr>
                                <td>
                                    event_id
                                </td>
                                <td>
                                    9
                                </td>
                                <td>
                                    10
                                </td>
                                <td>
                                    11
                                </td>
                                <td>
                                    12
                                </td>
                                <td>
                                    13
                                </td>
                                <td>
                                    14
                                </td>
                                <td>
                                    15
                                </td>
                            </tr>
                            <tr>
                                <td>
                                    will_attend
                                </td>
                                <td>
                                    0
                                </td>
                                <td>
                                    1
                                </td>
                                <td>
                                    1
                                </td>
                                <td>
                                    0
                                </td>
                                <td>
                                    0
                                </td>
                                <td>
                                    0
                                </td>
                                <td>
                                    0
                                </td>
                            </tr>
                        </table>
                    </div>
                    <p>
                        <code>shift_ids</code> is simillarly specified by a comma-separted list and required for all shifted events.
                    </p>
                    <p>
                        If the <code>shift_ids</code> parameter is present, the <code>guests</code> parameter must be a comma-separated list with the same number of comma-separated elements. Each item in a coma-separated <code>guests</code> list coresponds to the elsement of <code>shift_ids</code> in the same position. The following example represents an rsvp to <code>shift_ids</code> 5, 8, and 9. The attendee will have 2 <code>guests</code> for <code>shift_ids</code> 5 and 8, and 3 <code>guests</code> for <code>shift_ids</code> 9.
                    </p>
                    <div class="example">
                        <p>
                            Example of multiple shifts with guests:
                        </p>
                        <pre>
<samp>
/page/graph/addrsvp?event_id=6&amp;will_attend=1&amp;guid=HASH&amp;<strong>shift_ids=5,8,9,4&amp;guests=2,2,0,2</strong>
</samp>
</pre>
                    </div>
                    <p>
                        If <code>shift_ids</code> is not present, the guest parameter matches the <code>event_id</code> parameter in the same way.
                    </p>
                    <div class="example">
                        <p>
                            Example of multiday event:
                        </p>
                        <pre>
<samp>
/page/graph/addrsvp?<strong>event_id=10,11</strong>&amp;will_attend=1&amp;guid=HASH&amp;<strong>guests=0,1</strong>
</samp>
</pre>
                    </div>
                    <p class="http_method">
                        Method: <code>GET</code>/<code>POST</code>
                    </p>
                    <p class="url">
                        URL: /page/graph/addrsvp?event_id=$event_id&amp;will_attend=1&amp; ...(etc. for all query parameters)
                    </p>
                    <div class="parameters">
                        <p>
                            Query Parameters:
                        </p>
                        <dl>
                            <dt>
                                <code>event_id</code> (required)
                            </dt>
                            <dd>
                                Unique integer id of event. Must be specified as a comma-separated list for multiday events.
                            </dd>
                            <dt>
                                <code>will_attend</code> (reqired)
                            </dt>
                            <dd>
                                Boolean. 1 if RSVPer and his or her guests are attending, 0 if not. To cancel an RSVP, set <code>will_attend=0</code>.
                            </dd>
                            <dt>
                                <code>guid</code> (required unless both <code>email</code> <em>and</em> <code>zip</code> given)
                            </dt>
                            <dd>
                                Unique, encrytped cons_id
                            </dd>
                            <dt>
                                <code>email</code> (required unless <code>GUID</code> given)
                            </dt>
                            <dd>
                                Attendee's valid email address.
                            </dd>
                            <dt>
                                <code>zip</code> (required unless <code>GUID</code> given)
                            </dt>
                            <dd>
                                The U.S. zip code of the attendee's residence
                            </dd>
                            <dt>
                                <code>shift_ids</code> (required for shifted events)
                            </dt>
                            <dd>
                                Id(s) of shifts attending. Multiple shifts may be specified by a coma separated list.
                            </dd>
                            <dt>
                                <code>phone</code> (required if <code>is_potential_volunteer=1</code>)
                            </dt>
                            <dd>
                                Contact phone number.
                            </dd>
                        </dl>
                        <p>
                            Additional Parameters
                        </p>
                        <dl>
                            <dt>
                                <code>addr1</code>
                            </dt>
                            <dd>
                                Address line 1.
                            </dd>
                            <dt>
                                <code>addr2</code>
                            </dt>
                            <dd>
                                Address line 2.
                            </dd>
                            <dt>
                                <code>addr3</code>
                            </dt>
                            <dd>
                                Address line 3.
                            </dd>
                            <dt>
                                <code>city</code>
                            </dt>
                            <dd>
                                Attendee's city of residence.
                            </dd>
                            <dt>
                                <code>comment</code>
                            </dt>
                            <dd>
                                Comment to accompany rsvp on the event's page
                            </dd>
                            <dt>
                                <code>country</code>
                            </dt>
                            <dd>
                                Attendee's country of residence.
                            </dd>
                            <dt>
                                <code>firstname</code>
                            </dt>
                            <dd>
                                The attendee's first name.
                            </dd>
                            <dt>
                                <code>guests</code>
                            </dt>
                            <dd>
                                Number of guests to accompany the attendee. Does not include the attendee (so an RSVP-er coming alone would have <code>guests=0</code>). Comma separated list when multiple shifts or multiple days to be attended. For shifted events, each comma separated element corresponds the element in the same position in <code>shift_ids</code>. For multiday, unshifted events, each comma-separated element correspons to the element in the same position in <code>event_id</code>.
                            </dd>
                            <dt>
                                <code>is_potential_volunteer</code>
                            </dt>
                            <dd>
                                Is the attendee willing to volunteer for the event
                            </dd>
                            <dt>
                                <code>is_reminder_sent</code>
                            </dt>
                            <dd>
                                Has a reminder email been sent to the attendee
                            </dd>
                            <dt>
                                <code>lastname</code>
                            </dt>
                            <dd>
                                The attendee's last name.
                            </dd>
                            <dt>
                                <code>pledge_amt</code>
                            </dt>
                            <dd>
                                Amount pledged with RSVP
                            </dd>
                            <dt>
                                <code>pledge_method</code>
                            </dt>
                            <dd>
                                Method in which contribution will be made
                            </dd>
                            <dt>
                                <code>state_cd</code>
                            </dt>
                            <dd>
                                Two letter state code.
                            </dd>
                            <dt>
                                <code>zip_4</code>
                            </dt>
                            <dd>
                                Four digit ext. to <code>zip</code>
                            </dd>
                        </dl>
                    </div>
                </div>
            </div>
            <h4>
                Actions Graph API
            </h4><!-- /getactions -->
            <div class="api_method">
                <h5>
                    <code>/getactions</code>
                </h5>
                <div class="description">
                    <p>
                        Returns the actions taken by the guid passed as a parameter, or based on the value of the 'guid' cookie if it exists. A guid cookie is generated and set if none exists at the time of the call.
                    </p>
                    <p>
                        Actions may take up to 10 minutes to be indexed and available via this API method.
                    </p>
                    <p>
                        The response time of this method will vary greatly depending on the query.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/graph/getactions
                </p>
                <div class="parameters">
                    <p>
                        Query Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>q</code> (optional)
                        </dt>
                        <dd>
                            The query string to search for within actions taken by the user. For full syntax, refer to <a href="http://lucene.apache.org/core/old_versioned_docs/versions/3_0_0/queryparsersyntax.html">Lucene query syntax</a>
                        </dd>
                        <dt>
                            <code>guid</code> (optional)
                        </dt>
                        <dd>
                            If the guid is present as a GET parameter, it is used instead of the guid cookie value, so that actions may be retrieved for known guid values without needing to set a cookie manually
                        </dd>
                        <dt>
                            <code>includeAttributedActions</code> (optional)
                        </dt>
                        <dd>
                            Set to 'true' to include actions that are children of (attributed to) an action taken by the specified guid
                        </dd>
                        <dt>
                            <code>facets</code> (optional)
                        </dt>
                        <dd>
                            JSON-encoded string representing a valid <a href="http://www.elasticsearch.org/guide/reference/api/search/facets/">facet query as specified in the ElasticSearch documentation</a>
                        </dd>
                        <dt>
                            <code>size</code> (optional)
                        </dt>
                        <dd>
                            An integer representing the number of results to return. A maximum of 50 results is enforced, and requests for a larger number of results will return an error
                        </dd>
                        <dt>
                            <code>from</code> (optional)
                        </dt>
                        <dd>
                            The zero-indexed result position to start from, enables pagination of results when used with the size parameter
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an object with the following properties:
                    </p>
                    <dl>
                        <dt>
                            <code>hits</code>
                        </dt>
                        <dd>
                            The list of actions matching the specified query, which is expected to be in Lucene syntax, and the specified guid from the GET parameter or cookie.
                        </dd>
                        <dt>
                            <code>facets</code>
                        </dt>
                        <dd>
                            The results of any facets, if specified in the query
                        </dd>
                        <dt>
                            <code>took</code>
                        </dt>
                        <dd>
                            The time taken, in ms, by the backend search server to retrieve the results
                        </dd>
                        <dt>
                            <code>hitCount</code>
                        </dt>
                        <dd>
                            The total number of hits for this query, regardless of size/from parameters
                        </dd>
                    </dl>
                </div>
                <div class="example">
                    <p>
                        Example:
                    </p>
                    <pre>
<samp>
http://XYZ/page/graph/getactions?q=attributes.fizz:buzz
</samp>
</pre>
                    <pre>
<samp>
{
    "hits":[
        {
            "guid":"1234",
            "name":"testAction",
            "attributes":{
                "fizz":"buzz"
            },
            "sourceCodes":{
                "foo":"bar"
            },
            "id":"16acc1f0-84f6-11e1-a520-1f85079694c1",
            "timestamp":"2012-04-12T23:20:27+0000",
            "attributionCode":"FqzB8IT2EeGlIB-FB5aUwQ",
            "attributedActionId":null,
            "consId":null
        }
    ],
    "facets":[

    ],
    "took":1062,
    "hitCount":1
}
</samp>
</pre>
                </div>
                <pre>
<samp>
http://XYZ/page/graph/getactions?includeAttributedActions=true&amp;facets={"degreesOfSeparation":{"terms":{"field":"degreeOfSeparation","size":10,"global":false}}}&amp;from=1&amp;size=3
</samp>
</pre>
                <pre>
<samp>
{
    "hits":[
        {
            "guid":"1234",
            "name":"__shareUrlGen",
            "attributes":{
                "ac":"1234567890abcdefghijklmnopqrstuvwxyz",
                "guid":"1234",
                "url":"http:\/\/www.google.com"
            },
            "sourceCodes":null,
            "id":"a90cc460-af1b-11e1-a620-05c8e583fe5b",
            "timestamp":"2012-06-05T14:35:13+0000",
            "attributionCode":"qQzEYK8bEeGmIAXI5YP-Ww",
            "attributedActionId":null,
            "consId":null
        },
        {
            "guid":"1234",
            "name":"__shareUrlGen",
            "attributes":{
                "ac":"1234567890abcdefghijklmnopqrstuvwxyz",
                "guid":"1234",
                "url":"http:\/\/www.yahoo.com"
            },
            "sourceCodes":null,
            "id":"6ed20590-af31-11e1-a332-1930b3246611",
            "timestamp":"2012-06-05T17:11:04+0000",
            "attributionCode":"btIFkK8xEeGjMhkwsyRmEQ",
            "attributedActionId":null,
            "consId":null
        },
        {
            "guid":"1234",
            "name":"__shareUrlGen",
            "attributes":{
                "ac":"1234567890abcdefghijklmnopqrstuvwxyz",
                "guid":"1234",
                "url":"http:\/\/www.example.com"
            },
            "sourceCodes":null,
            "id":"9f273c80-af31-11e1-9013-510052e52c7a",
            "timestamp":"2012-06-05T17:12:25+0000",
            "attributionCode":"nyc8gK8xEeGQE1EAUuUseg",
            "attributedActionId":null,
            "consId":null
        }
    ],
    "facets":{
        "degreesOfSeparation":{
            "_type":"terms",
            "missing":19,
            "total":83,
            "other":0,
            "terms":[
                {
                    "term":0,
                    "count":32
                },
                {
                    "term":1,
                    "count":28
                },
                {
                    "term":2,
                    "count":20
                },
                {
                    "term":3,
                    "count":2
                },
                {
                    "term":4,
                    "count":1
                }
            ]
        }
    },
    "took":2717,
    "hitCount":20
}
</samp>
</pre>
            </div>
            <div class="errors">
                <p>
                    Errors:
                </p>
                <p>
                    This request can return a <code>query_error</code> if it receives an error from the search server, most likely due to a badly-formed query string. Please refer to the above query syntax reference for more information.
                </p>
                <p>
                    This request will return a <code>maximum_result_size_exceeded</code> if the size parameter is greater than 50.
                </p>
            </div>
        </div>
        <h4>
            Ladder of Engagement Graph API
        </h4><!-- LOE -->
        <div class="api_method">
            <h5>
                <code>/loe/$GUID</code>
            </h5>
            <div class="description">
                <p>
                    Returns a set of attributes associated with the $GUID indicating the constituent's past level of engagement with the site. This information can be used to tailor or target content with the goal of increasing interest and engagement.
                </p>
            </div>
            <p class="http_method">
                Method: <code>GET</code>
            </p>
            <p class="url">
                URL: /page/graph/loe/$GUID
            </p>
            <div class="parameters">
                <p>
                    Query Parameters: none
                </p>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
                <p>
                    A set of key-value pairs indicating whether or not certain data are present in the constituent's account, or certain actions have been taken, and in some cases actual information about those actions. These include:
                </p>
                <dl>
                    <dt>
                        <code>email</code>
                    </dt>
                    <dd>
                        Boolean, whether or not we have an email address on file
                    </dd>
                    <dt>
                        <code>location</code>
                    </dt>
                    <dd>
                        Boolean, whether or not we have a location (latitude and longitude, from a valid address or zip code) on file
                    </dd>
                    <dt>
                        <code>phone</code>
                    </dt>
                    <dd>
                        The actual phone number associated with the guid, or false if none is on file (if the constituent has multiple phone numbers, the primary phone will be returned; if they're subscribed to QD SMS, the phone number is the one under which they are subscribed)
                    </dd>
                    <dt>
                        <code>donor</code>
                    </dt>
                    <dd>
                        Boolean, whether or not the constituent has ever made a donation
                    </dd>
                    <dt>
                        <code>qd_enrolled</code>
                    </dt>
                    <dd>
                        Boolean, whether or not the constituent is enrolled in Quick Donate (i.e. has saved payment information)
                    </dd>
                    <dt>
                        <code>qd_sms_subscribed</code>
                    </dt>
                    <dd>
                        Boolean, whether or not the constituent is subscribed to Quick Donate text messages and has connected their phone number to their saved payment information
                    </dd>
                    <dt>
                        <code>last_donation</code>
                    </dt>
                    <dd>
                        Date and time (YYYY-MM-DD HH:MM:SS) of the last donation made by the constituent, if any
                    </dd>
                    <dt>
                        <code>outreach</code>
                    </dt>
                    <dd>
                        Boolean, whether or not the constituent has set up a grassroots fundraising page
                    </dd>
                </dl>
            </div>
            <div class="example">
                <p>
                    Example:
                </p>
                <pre>
<samp>
http://XYZ/page/graph/loe/abcdefghij1234567
</samp>
</pre>
                <pre>
<samp>
{
    "email":true,
    "location":true,
    "phone":"6175551234",
    "donor":true,
    "qd_enrolled":true,
    "qd_sms_subscribed":true,
    "last_donation":"2012-07-26 19:16:15",
    "outreach":true
}
</samp>
</pre>
            </div>
            <div class="errors">
                <p>
                    Errors:
                </p>
                <p>
                    If the GUID is not found, the following error will be returned:
                </p>
                <pre>
<samp>
{
    "error":"invalid_cons_id",
    "error_description":"Constituent ID does not exist"
}
</samp>
</pre>
            </div>
        </div><!-- end LOE -->
        <h2>
            Blue State Digital XML API
        </h2>
        <div class="major_section">
            <h3>
                Overview
            </h3>
            <p>
                The Blue State Digital API allows external access to data and functionality inside the Blue State Digital system. The API follows many REST design principles and can be accessed from any common web programming environment, including PHP, Ruby, Perl, Python, and .NET.
            </p>
            <p>
                All API calls use either an HTTP GET or POST request. Some calls support both methods. In addition to the call-specific parameters, all API calls need to include four additional parameters (api_id, api_ts, api_mac, and api_ver), which are be used to validate the API request. These are documented in the Common API Parameters section below.
            </p>
            <p>
                API calls will return standard HTTP error codes to indicate success or failure of a request. If a request fails, a message will be included in the failure providing a detailed reason for the failure. Different calls have different return formats on success. Some return XML or JSON, some simply numbers or strings. See the documentation for the API call to determine returned data format.
            </p>
        </div>
        <div class="major_section">
            <h3>
                Common API Parameters
            </h3>so that actions may be retrieved for known guid values without needing to set a cookie manually
            <p>
                For security, all API calls will require an api_id, api_ts, and api_mac parameter. These serve to authenticate the request. The api_id parameter is configured via the Blue State Digital Control Panel. Each api_id has a corresponding api_secret associated with it. This api_secret is used to calculate the api_mac for a given request. The api_secret is stored on the server making the API call and is never sent over the network as part of an API request. Additionally, the api_ver is required in order to handle backwards compatiblity.
            </p>
            <p>
                It is STRONGLY recommended that you issue a separate api_id/api_secret for each external application that will be accessing the API.
            </p>
            <div class="parameters">
                <p>
                    Parameters:
                </p>
                <dl>
                    <dt>
                        <code>api_id</code>
                    </dt>
                    <dd>
                        the API ID for this request
                    </dd>
                    <dt>
                        <code>api_ts</code>
                    </dt>
                    <dd>
                        Current time in epoch seconds. This must match the epoch seconds value used to calculate the api_mac. It is important that the server sending the request stay keep its clock up-to-date using ntpd. The BSD server will disallow any request that has a timestamp which is more than 30 seconds older or newer than the BSD server's current time.
                    </dd>
                    <dt>
                        <code>api_mac</code>
                    </dt>
                    <dd>
                        The message authentication code (MAC) for the request. See the Understanding the api_mac section for details on how to get this value.
                    </dd>
                    <dt>
                        <code>api_ver</code>
                    </dt>
                    <dd>
                        The version of the API that client code will work with. This allows changes to be made to the API without causing backwards compatibility issues.
                    </dd>
                </dl>
            </div>
            <p>
                These four common API parameters are <strong>ALWAYS</strong> included in the URL, even if the actual API call uses the POST method.
            </p>
        </div>
        <div class="major_section">
            <h3>
                Deferred Results
            </h3>
            <p>
                Certain API functions may not return results right away. If the requested API call is too time consuming, it may return an HTTP response code of 202 (Accepted). The body of the response will contain a single, 32-byte string (the deferred_id). This ID can be used to retrieve the results when they are ready. The API documentation clearly indicates which calls may return deferred results. Any code that uses these calls MUST be capable of handling the deferred result set. The <code>get_deferred_results</code> function is used to retrieve the result set.
            </p>
            <p>
                The following pseudo-code shows how to retrieve deferred results inline with the code making the initial API call. Of course, depending on how long the API call takes to return the results and the nature of your program, you may not want to have your code block while polling for results.
            </p>
            <pre>
<samp>
$response_obj = make_api_call(
    "some_call_that_defers", "some=param&amp;some_other=param");

if ($response_obj.get_status_code() == 202) {
    // we were deferred

    $deferred_id = $response_obj.get_response_body();

    do {
        sleep(30);
        $response_obj = make_api_call(
            "get_deferred_results", "deferred_id=".$deferred_id);
    } while ($response_obj.get_status_code() == 202);
}

$actual_result = $response_obj.get_response_body();
</samp>
</pre>
            <p>
                If you make extensive use of API calls which may defer their results, you may consider using the above code on all of your API calls to save yourself the effort of implementing support in each location where you make a call. It should do the right thing whether the API call defers its results or not.
            </p>
            <h4>
                Deferred Result Callbacks
            </h4>
            <p>
                Instead of polling, you can also choose to have the API "call you back" when a deferred request is ready. In this case, when the deferred request completes, the API will make an HTTP POST request to the URL you have provided as the callback URL, with the POST body containing only the 32 character deferred id. You then proceed to use get_deferred_results as normal to retrieve the results of your call.
            </p>
            <p>
                Callback URLS cannot be behind HTTP authentication or require any more authentication than they carry in their URL parameters, as the API will not know how to authenticate them and will not make anything more than a basic POST request to the supplied URL.
            </p>
            <p>
                To use a deferred result callback, pass the deferred_callback parameter in your API request:
            </p>
            <div class="parameters">
                <dl>
                    <dt>
                        <code>deferred_callback</code>
                    </dt>
                    <dd>
                        the URL to call back to when this request is completed and ready for pickup
                    </dd>
                </dl>
            </div>
            <h4>
                Deferred Results API Calls
            </h4><!-- get_deferred_results -->
            <div class="api_method">
                <h5>
                    <code>get_deferred_results</code>
                </h5>
                <div class="description">
                    <p>
                        Allows software to retrieve the results of a previously requested API call that was deferred (see the Deferred Results section for details).
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/api/get_deferred_results
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>deferred_id</code> (required)
                        </dt>
                        <dd>
                            The ID string (32 characters) provided in response to a previous API call that was deferred.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        If the results are ready, returns the HTTP status code and response body for the original API call. Note that, once retrieved, the results are deleted from the system and are no longer available.
                    </p>
                    <p>
                        If the results are not yet ready, an HTTP status code 202 (Accepted) is returned. This indicates that you should try back later to retrieve your results.
                    </p>
                    <p>
                        If the results have already been retrieved (and are therefore no longer available in the system), the HTTP status code 410 (Gone) is returned.
                    </p>
                    <p>
                        If there is no content to deliver, a 204 (No Content) is returned.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/get_constituents_by_id?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=61722c6a82a16779c956992e351ae1582f4a3cc1&amp;ext_type=van_id&amp;ext_ids=12772,1129,1983321
</samp>
</pre>
            </div>
        </div>
        <div class="major_section">
            <h3>
                Common Record Formats
            </h3>
            <p>
                Certain types of data are returned by multiple API calls. These record formats are defined below. (<small>note: All times returned by the API will be in the system timezone</small>)
            </p>
            <h4>
                Exported XML Circle Records
            </h4>
            <h5>
                Core: <code>circle</code>
            </h5>
            <p>
                The core circle record contains the name, slug, description, circle type, member count and creation date:
            </p>
            <pre>
<samp>
&lt;circle id="1" modified_dt="1223651690"&gt;
    &lt;name&gt;test circle#1&lt;/name&gt;
    &lt;slug&gt;test_slug&lt;/slug&gt;
    &lt;description&gt;description for circle #1&lt;/description&gt;
    &lt;create_dt&gt;2008-10-10 11:14:50&lt;/create_dt&gt;
    &lt;circle_type&gt;0&lt;/circle_type&gt;
    &lt;member_count&gt;0&lt;/member_count&gt;
&lt;/circle&gt;
</samp>
</pre>
            <h4>
                Exported XML Constituent Records
            </h4>
            <p>
                The constituent record is the basic identifier of a person. The core constituent record is remarkably slim, containing only the most basic information about a person (name and whether they have an account). When making an API call that returns a constituent record, you can specify one or more data bundles that enhance the constituent record with additional data. Both the core constituent record and each data bundle contain a modified_dt attribute. This contains the date and time (in epoch seconds) when a record (or bundle) was last modified.
            </p>
            <p>
                Bundles are added to each <code>&lt;cons&gt;</code> XML element. If there a bundle has more than one data element associated with it, all elements will be included inside the <code>&lt;cons&gt;</code> XML element. For example, using the <code>cons_addr</code> bundle for a person with two addresses would result in two <code>&lt;cons_addr&gt;</code> elements inside their <code>&lt;cons&gt;</code> element.
            </p>
            <h5>
                Core: <code>cons</code>
            </h5>
            <p>
                The core constituent record contains only the GUID, name, account status, and account creation date:
            </p>
            <pre>
<samp>
&lt;cons id="4382" modified_dt="1171861200"&gt;
    &lt;guid&gt;ygdFPkyEdomzBhWEFZGREys&lt;/guid&gt;
    &lt;firstname&gt;Bob&lt;/firstname&gt;
    &lt;middlename&gt;Reginald&lt;/middlename&gt;
    &lt;lastname&gt;Smith&lt;/lastname&gt;
    &lt;has_account&gt;1&lt;/has_account&gt;
    &lt;is_banned&gt;0&lt;/is_banned&gt;
    &lt;create_dt&gt;1168146000&lt;/create_dt&gt;
    &lt;suffix&gt;III&lt;/suffix&gt;
    &lt;prefix&gt;Mr&lt;/prefix&gt;
    &lt;gender&gt;M&lt;/gender&gt;
    &lt;source&gt;default&lt;/source&gt;
    &lt;subsource&gt;default&lt;/subsource&gt;
&lt;/cons&gt;
</samp>
</pre>
            <p>
                All other information is added via data bundles.
            </p>
            <h5>
                Data Bundle: <code>circle</code>
            </h5>
            <p>
                The <code>circle</code> bundle provides information about all of the circles that a person is a member of.
            </p>
            <pre>
<samp>
&lt;cons id="4382" modified_dt="1171861200"&gt;
    &lt;guid&gt;ygdFPkyEdomzBhWEFZGREys&lt;/guid&gt;
    &lt;firstname&gt;Bob&lt;/firstname&gt;
    &lt;lastname&gt;Smith&lt;/lastname&gt;
    &lt;has_account&gt;1&lt;/has_account&gt;
    &lt;is_banned&gt;0&lt;/is_banned&gt;
    &lt;create_dt&gt;1168146000&lt;/create_dt&gt;

    <strong>&lt;circle id="2"&gt;
        &lt;name&gt;People for Mr. Smith&lt;/name&gt;
        &lt;url&gt;http://www.example.com/page/group/Smith&lt;/url&gt;
        &lt;joined_dt&gt;1168146011&lt;/joined_dt&gt;
        &lt;is_private&gt;0&lt;/is_private&gt;
        &lt;is_admin&gt;1&lt;/is_admin&gt;
    &lt;/circle&gt;
    &lt;circle id="43"&gt;
        &lt;name&gt;Unions for Unions&lt;/name&gt;
        &lt;url&gt;http://www.example.com/page/group/UfU&lt;/url&gt;
        &lt;joined_dt&gt;1168146011&lt;/joined_dt&gt;
        &lt;is_private&gt;1&lt;/is_private&gt;
        &lt;is_admin&gt;0&lt;/is_admin&gt;
    &lt;/circle&gt;</strong>

&lt;/cons&gt;
</samp>
</pre>
            <h5>
                Data Bundle: <code>community_journal</code>
            </h5>
            <p>
                The <code>community_journal</code> bundle provides information about a person's community blog participation. If a journal exists but does not contain any posts <code>first_post_dt</code>, <code>latest_post_dt</code>, and <code>num_posts</code> will return zero. If the journal posts (if any exist) do not have any comments <code>first_comment_dt</code>, <code>latest_comment_dt</code>, and <code>num_comments</code> will return zero.
            </p>
            <pre>
<samp>
&lt;cons id="4382" modified_dt="1171861200"&gt;
    &lt;guid&gt;ygdFPkyEdomzBhWEFZGREys&lt;/guid&gt;
    &lt;firstname&gt;Bob&lt;/firstname&gt;
    &lt;lastname&gt;Smith&lt;/lastname&gt;
    &lt;has_account&gt;1&lt;/has_account&gt;
    &lt;is_banned&gt;0&lt;/is_banned&gt;
    &lt;create_dt&gt;1168146000&lt;/create_dt&gt;

    <strong>&lt;community_journal id="45"&gt;
        &lt;url&gt;http://www.example.com/page/community/blog/bob&lt;/url&gt;
        &lt;rss_url&gt;http://www.example.com/page/community/blog_rss/bob&lt;/rss_url&gt;
        &lt;first_post_dt&gt;1171861200&lt;/first_post_dt&gt;
        &lt;latest_post_dt&gt;1208394744&lt;/first_post_dt&gt;
        &lt;num_posts&gt;11&lt;/num_posts&gt;
        &lt;first_comment_dt&gt;1171891200&lt;/first_comment_dt&gt;
        &lt;latest_comment_dt&gt;1208394764&lt;/latest_comment_dt&gt;
        &lt;num_comments&gt;243&lt;/num_comments&gt;
    &lt;/community_journal&gt;</strong>

&lt;/cons&gt;
</samp>
</pre>
            <h5>
                Data Bundle: <code>cons_addr</code>
            </h5>
            <p>
                The <code>cons_addr</code> data bundles add address information to a constituent record. <code>cons_addr</code> will add zero or more address elements. Example (with one address):
            </p>
            <pre>
<samp>
&lt;cons id="4382" modified_dt="1171861200"&gt;
    &lt;guid&gt;ygdFPkyEdomzBhWEFZGREys&lt;/guid&gt;
    &lt;firstname&gt;Bob&lt;/firstname&gt;
    &lt;lastname&gt;Smith&lt;/lastname&gt;
    &lt;has_account&gt;1&lt;/has_account&gt;
    &lt;is_banned&gt;0&lt;/is_banned&gt;
    &lt;create_dt&gt;1168146000&lt;/create_dt&gt;

    <strong>&lt;cons_addr id="5733" modified_dt="1168146001"&gt;
        &lt;addr1&gt;123 Fake St.&lt;/addr1&gt;
        &lt;addr2&gt;&lt;/addr2&gt;
        &lt;city&gt;Anytown&lt;/city&gt;
        &lt;state_cd&gt;CA&lt;/state_cd&gt;
        &lt;zip&gt;92345&lt;/zip&gt;
        &lt;zip_4&gt;8311&lt;/zip_4&gt;
        &lt;country&gt;US&lt;/country&gt;
        &lt;is_primary&gt;1&lt;/is_primary&gt;
        &lt;latitude&gt;42.000&lt;/latitude&gt;
        &lt;longitude&gt;71.000&lt;/longitude&gt;
   &lt;/cons_addr&gt;</strong>

&lt;/cons&gt;
</samp>
</pre>
            <h5>
                Data Bundle: <code>cons_email</code> and <code>primary_cons_email</code>
            </h5>
            <p>
                The <code>cons_email</code> and <code>primary_cons_email</code> data bundles add email addresses to a constituent record. <code>cons_email</code> will add zero or more elements. <code>primary_cons_email</code> will add zero or one email elements, choosing the most recently added email address as the primary one. Example (with two email addresses):
            </p>
            <pre>
<samp>
&lt;cons id="4382" modified_dt="1171861200"&gt;
    &lt;guid&gt;ygdFPkyEdomzBhWEFZGREys&lt;/guid&gt;
    &lt;firstname&gt;Bob&lt;/firstname&gt;
    &lt;lastname&gt;Smith&lt;/lastname&gt;
    &lt;has_account&gt;1&lt;/has_account&gt;
    &lt;is_banned&gt;0&lt;/is_banned&gt;
    &lt;create_dt&gt;1168146000&lt;/create_dt&gt;

    <strong>&lt;cons_email id="8991" modified_dt="1168146011"&gt;
        &lt;email&gt;bsmith@somecompany.com&lt;/email&gt;
        &lt;email_type&gt;work&lt;/email_type&gt;
        &lt;is_subscribed&gt;0&lt;/is_subscribed&gt;
        &lt;is_primary&gt;0&lt;/is_primary&gt;
    &lt;/cons_email&gt;

    &lt;cons_email id="12702" modified_dt="1178510447"&gt;
        &lt;email&gt;bob_smith@someisp.com&lt;/email&gt;
        &lt;email_type&gt;personal&lt;/email_type&gt;
        &lt;is_subscribed&gt;1&lt;/is_subscribed&gt;
        &lt;is_primary&gt;1&lt;/is_primary&gt;
    &lt;/cons_email&gt;</strong>

&lt;/cons&gt;
</samp>
</pre>
            <h5>
                Data Bundle: <code>cons_field</code>
            </h5>
            <p>
                The <code>cons_field</code> data bundle adds custom constituent fields to a constituent record. <code>cons_field</code> will add zero or more elements. Example (with two custom constituent fields):
            </p>
            <pre>
<samp>
&lt;cons id="4382" modified_dt="1171861200"&gt;
    &lt;guid&gt;ygdFPkyEdomzBhWEFZGREys&lt;/guid&gt;
    &lt;firstname&gt;Bob&lt;/firstname&gt;
    &lt;lastname&gt;Smith&lt;/lastname&gt;
    &lt;has_account&gt;1&lt;/has_account&gt;
    &lt;is_banned&gt;0&lt;/is_banned&gt;
    &lt;create_dt&gt;1168146000&lt;/create_dt&gt;

    <strong>&lt;cons_field id="176"&gt;
        &lt;value&gt;custom value 0&lt;/value&gt;
    &lt;/cons_field&gt;

    &lt;cons_email id="177"&gt;
        &lt;value&gt;custom value 1&lt;/value&gt;
    &lt;/cons_field&gt;</strong>

&lt;/cons&gt;
</samp>
</pre>
            <h5>
                Data Bundle: <code>cons_friends</code>
            </h5>
            <p>
                The <code>cons_friends</code> bundle provides information about a person's socialnet relationships. If the constituent has any consummated socialnet relationships, then the socialnet element will contain one or more friend elements.
            </p>
            <pre>
<samp>
&lt;cons id="4382"&gt;
    &lt;guid&gt;ygdFPkyEdomzBhWEFZGREys&lt;/guid&gt;
    &lt;firstname&gt;Bob&lt;/firstname&gt;
    &lt;lastname&gt;Smith&lt;/lastname&gt;
    &lt;has_account&gt;1&lt;/has_account&gt;
    &lt;is_banned&gt;0&lt;/is_banned&gt;
    &lt;create_dt&gt;1168146000&lt;/create_dt&gt;

    <strong>&lt;socialnet&gt;
        &lt;friend id="5321"&gt;
            &lt;invite_dt&gt;1189111883&lt;/invite_dt&gt;
            &lt;consummated_dt&gt;1189115528&lt;/consummated_dt&gt;
        &lt;/friend&gt;
    &lt;/socialnet&gt;</strong>

&lt;/cons&gt;
</samp>
</pre>
            <div>
                <p>
                    Attribute Definitions:
                </p>
                <dl>
                    <dt>
                        id
                    </dt>
                    <dd>
                        integer
                    </dd>
                </dl>
            </div>
            <h5>
                Data Bundle: <code>cons_group</code>
            </h5>
            <p>
                The <code>cons_group</code> bundle provides information about a person's constituent group membership.
            </p>
            <pre>
<samp>
&lt;cons id="4382" modified_dt="1171861200"&gt;
    &lt;guid&gt;ygdFPkyEdomzBhWEFZGREys&lt;/guid&gt;
    &lt;firstname&gt;Bob&lt;/firstname&gt;
    &lt;lastname&gt;Smith&lt;/lastname&gt;
    &lt;has_account&gt;1&lt;/has_account&gt;
    &lt;is_banned&gt;0&lt;/is_banned&gt;
    &lt;create_dt&gt;1168146000&lt;/create_dt&gt;

    <strong>&lt;cons_group id="17"  modified_dt="1168146011"/&gt;
    &lt;cons_group id="41" modified_dt="1163196031" /&gt;</strong>

&lt;/cons&gt;
</samp>
</pre>
            <h5>
                Data Bundle: <code>cons_phone</code> and <code>primary_cons_phone</code>
            </h5>
            <p>
                The <code>cons_phone</code> and <code>primary_cons_phone</code> data bundles add phone numbers to a constituent record. cons_addr will add zero or more elements. <code>primary_cons_phone</code> will add zero or one phone number elements, choosing the most recently added phone number as the primary one. Example (with two phone numbers):
            </p>
            <pre>
<samp>
&lt;cons id="4382" modified_dt="1171861200"&gt;
    &lt;guid&gt;ygdFPkyEdomzBhWEFZGREys&lt;/guid&gt;
    &lt;firstname&gt;Bob&lt;/firstname&gt;
    &lt;lastname&gt;Smith&lt;/lastname&gt;
    &lt;has_account&gt;1&lt;/has_account&gt;
    &lt;is_banned&gt;0&lt;/is_banned&gt;
    &lt;create_dt&gt;1168146000&lt;/create_dt&gt;

    <strong>&lt;cons_phone id="6933" modified_dt="1168146030"&gt;
        &lt;phone&gt;4082345678&lt;/phone&gt;
        &lt;phone_type&gt;home&lt;/phone_type&gt;
        &lt;is_primary&gt;0&lt;/is_primary&gt;
        &lt;is_subscribed&gt;0&lt;/is_subscribed&gt;
    &lt;/cons_phone&gt;
    &lt;cons_phone id="7322" modified_dt="1173243602"&gt;
        &lt;phone&gt;4089876432&lt;/phone&gt;
        &lt;phone_type&gt;mobile&lt;/phone_type&gt;
        &lt;is_primary&gt;1&lt;/is_primary&gt;
        &lt;is_subscribed&gt;1&lt;/is_subscribed&gt;
    &lt;/cons_phone&gt;</strong>

&lt;/cons&gt;
</samp>
</pre>
            <h5>
                Data Bundle: <code>event</code>
            </h5>
            <p>
                The event bundle provides information about a person's event participation.
            </p>
            <pre>
<samp>
&lt;cons id="4382" modified_dt="1171861200"&gt;
    &lt;guid&gt;ygdFPkyEdomzBhWEFZGREys&lt;/guid&gt;
    &lt;firstname&gt;Bob&lt;/firstname&gt;
    &lt;lastname&gt;Smith&lt;/lastname&gt;
    &lt;has_account&gt;1&lt;/has_account&gt;
    &lt;is_banned&gt;0&lt;/is_banned&gt;
    &lt;create_dt&gt;1168146000&lt;/create_dt&gt;

    <strong>&lt;event&gt;
        &lt;num_attended&gt;3&lt;/num_attended&gt;
        &lt;first_attended_dt&gt;1163146011&lt;/first_attended_dt&gt;
        &lt;latest_attended_dt&gt;1168147011&lt;/latest_attended_dt&gt;
        &lt;num_created&gt;1&lt;/num_created&gt;
        &lt;first_created_dt&gt;1168146011&lt;/first_created_dt&gt;
        &lt;latest_created_dt&gt;1168146020&lt;/latest_created_dt&gt;
        &lt;attended_rss_url&gt;http://www.example.com/page/events/&lt;/attended_rss_url&gt;
        &lt;created_rss_url&gt;http://www.example.com/page/events/&lt;/created_rss_url&gt;
    &lt;/event&gt;</strong>

&lt;/cons&gt;
</samp>
</pre>
            <h5>
                Data Bundle: <code>ext_X</code>
            </h5>
            <p>
                The <code>ext_X</code> bundle allows the inclusion of external IDs (as set with the <code>set_ext_ids</code> API call). X is to be replaced with the external ID type specified on the <code>set_ext_ids</code> call. The following example assumes that a data bundle of ext_van_id was specified and that an external ID of type van_id had been specified for the resulting constituent record:
            </p>
            <pre>
<samp>
&lt;cons id="4382" modified_dt="1171861200"&gt;
    &lt;guid&gt;ygdFPkyEdomzBhWEFZGREys&lt;/guid&gt;
    &lt;firstname&gt;Bob&lt;/firstname&gt;
    &lt;lastname&gt;Smith&lt;/lastname&gt;
    &lt;has_account&gt;1&lt;/has_account&gt;
    &lt;is_banned&gt;0&lt;/is_banned&gt;
    &lt;create_dt&gt;1168146000&lt;/create_dt&gt;

    <strong>&lt;ext_id type="van_id" id="My_Ext_id_381128342" modified_dt="1168146011"/&gt;</strong>

&lt;/cons&gt;
</samp>
</pre>
            <h4>
                Incoming XML Constituent Records
            </h4>
            <p>
                The API allow for incoming bundle data in similar formats to outgoing data, with several changes for IDs. Incoming data must be encoded as UTF-8.
            </p>
            <h5>
                Core: <code>cons</code>
            </h5>
            <p>
                The core constituent record contains only the name, account status, and account creation date (GUID cannot be set by an external call; it will be generated when the constituent is created:
            </p>
            <pre>
<samp>
&lt;cons <em>id="4382" ext_id="ext_20034" ext_type="client_system"</em>&gt;
    &lt;firstname&gt;Bob&lt;/firstname&gt;
    &lt;lastname&gt;Smith&lt;/lastname&gt;
    &lt;is_banned&gt;0&lt;/is_banned&gt;
    &lt;create_dt&gt;1168146000&lt;/create_dt&gt;
&lt;/cons&gt;
</samp>
</pre>
            <p>
                In incoming data, the <code>id</code>, <code>ext_id</code>, and <code>ext_type</code> attributes are optional. The following rules apply to these attributes:
            </p>
            <ul>
                <li>If a record has an <code>id</code> attribute, it will be rejected if that <code>id</code> does not match.
                </li>
                <li>If it has no <code>id</code>, <code>ext_id</code>, or <code>ext_type</code> attributes, it will be added.
                </li>
                <li>If <code>ext_id</code> or <code>ext_type</code> are specified but not both, the record will be rejected.
                </li>
                <li>If <code>ext_id</code> and <code>ext_type</code> are specified specified, the record will be added or updated depending on if the <code>ext_id</code>/<code>ext_type</code> combination provides a mapping to an existing constituent record. If the record is added, the <code>ext_id</code>/<code>ext_type</code> mapping will be added also.
                </li>
            </ul>
            <div>
                <p>
                    Field Definitions:
                </p>
                <dl>
                    <dt>
                        firstname:
                    </dt>
                    <dd>
                        string, up to 128 characters
                    </dd>
                    <dt>
                        middlename:
                    </dt>
                    <dd>
                        string, up to 128 characters
                    </dd>
                    <dt>
                        lastname:
                    </dt>
                    <dd>
                        string, up to 128 characters
                    </dd>
                    <dt>
                        prefix:
                    </dt>
                    <dd>
                        string, up to 16 characters
                    </dd>
                    <dt>
                        suffix:
                    </dt>
                    <dd>
                        string, up to 16 characters
                    </dd>
                    <dt>
                        is_banned:
                    </dt>
                    <dd>
                        integer, 0 or 1
                    </dd>
                    <dt>
                        create_dt:
                    </dt>
                    <dd>
                        unix timestamp
                    </dd>
                    <dt>
                        ext_id:
                    </dt>
                    <dd>
                        string, up to 255 characters
                    </dd>
                    <dt>
                        ext_type:
                    </dt>
                    <dd>
                        string, up to 100 characters
                    </dd>
                </dl>
            </div>
            <p>
                All other information is added via data bundles.
            </p>
            <h5>
                Data Bundle: <code>cons_addr</code>
            </h5>
            <p>
                The <code>cons_addr</code> data bundles add address information to a constituent record in BSD's database. <code>cons_addr</code> will add zero or more address elements. Incoming <code>cons_addr</code> records do not contain IDs; they are automatically associated with the cons record they are nested in. Example (with one address):
            </p>
            <pre>
<samp>
&lt;cons id="4382"&gt;
    &lt;firstname&gt;Bob&lt;/firstname&gt;
    &lt;lastname&gt;Smith&lt;/lastname&gt;
    &lt;is_banned&gt;0&lt;/is_banned&gt;
    &lt;create_dt&gt;1168146000&lt;/create_dt&gt;

    <strong>&lt;cons_addr&gt;
        &lt;addr1&gt;123 Fake St.&lt;/addr1&gt;
        &lt;addr2&gt;&lt;/addr2&gt;
        &lt;city&gt;Anytown&lt;/city&gt;
        &lt;state_cd&gt;CA&lt;/state_cd&gt;
        &lt;zip&gt;92345&lt;/zip&gt;
        &lt;zip_4&gt;8311&lt;/zip_4&gt;
        &lt;country&gt;US&lt;/country&gt;
        &lt;is_primary&gt;1&lt;/is_primary&gt;
        &lt;latitude&gt;42.000&lt;/latitude&gt;
        &lt;longitude&gt;71.000&lt;/longitude&gt;
    &lt;/cons_addr&gt;</strong>

&lt;/cons&gt;
</samp>
</pre>
            <div>
                <p>
                    Field Definitions:
                </p>
                <dl>
                    <dt>
                        addr1:
                    </dt>
                    <dd>
                        string, up to 255 characters
                    </dd>
                    <dt>
                        addr2:
                    </dt>
                    <dd>
                        string, up to 255 characters
                    </dd>
                    <dt>
                        city:
                    </dt>
                    <dd>
                        string, up to 64 characters
                    </dd>
                    <dt>
                        state_cd:
                    </dt>
                    <dd>
                        string, 2 characters
                    </dd>
                    <dt>
                        zip:
                    </dt>
                    <dd>
                        string, up to 16 characters
                    </dd>
                    <dt>
                        zip_4:
                    </dt>
                    <dd>
                        string, up to 4 characters
                    </dd>
                    <dt>
                        country:
                    </dt>
                    <dd>
                        string, two characters
                    </dd>
                    <dt>
                        is_primary:
                    </dt>
                    <dd>
                        integer, 0 or 1
                    </dd>
                </dl>
            </div>
            <h5>
                Data Bundle: <code>cons_email</code> and <code>primary_cons_email</code>
            </h5>
            <p>
                The <code>cons_email</code> and <code>primary_cons_email</code> data bundles add email addresses to a constituent record. <code>cons_email</code> will add zero or more elements. <code>primary_cons_email</code> will add zero or one email elements, choosing the most recently added email address as the primary one. Incoming <code>cons_email</code> records do not contain IDs; they are automatically associated with the cons record they are nested in. Example (with two email addresses):
            </p>
            <pre>
<samp>
&lt;cons id="4382"&gt;
    &lt;firstname&gt;Bob&lt;/firstname&gt;
    &lt;lastname&gt;Smith&lt;/lastname&gt;
    &lt;is_banned&gt;0&lt;/is_banned&gt;
    &lt;create_dt&gt;1168146000&lt;/create_dt&gt;

    <strong>&lt;cons_email&gt;
        &lt;email&gt;bsmith@somecompany.com&lt;/email&gt;
        &lt;email_type&gt;work&lt;/email_type&gt;
        &lt;is_subscribed&gt;0&lt;/is_subscribed&gt;
        &lt;is_primary&gt;0&lt;/is_primary&gt;
    &lt;/cons_email&gt;
    &lt;cons_email&gt;
        &lt;email&gt;bob_smith@someisp.com&lt;/email&gt;
        &lt;email_type&gt;personal&lt;/email_type&gt;
        &lt;is_subscribed&gt;1&lt;/is_subscribed&gt;
        &lt;is_primary&gt;1&lt;/is_primary&gt;
    &lt;/cons_email&gt;</strong>

&lt;/cons&gt;
</samp>
</pre>
            <div>
                <p>
                    Field Definitions:
                </p>
                <dl>
                    <dt>
                        email:
                    </dt>
                    <dd>
                        string, up to 128 characters
                    </dd>
                    <dt>
                        email_type:
                    </dt>
                    <dd>
                        constant string, <code>personal</code> or <code>work</code>
                    </dd>
                    <dt>
                        is_subscribed:
                    </dt>
                    <dd>
                        integer, 0 or 1
                    </dd>
                    <dt>
                        is_primary:
                    </dt>
                    <dd>
                        integer, 0 or 1
                    </dd>
                </dl>
            </div>
            <h5>
                Data Bundle: <code>cons_group</code>
            </h5>
            <p>
                The <code>cons_group</code> bundle provides information about a person's constituent group membership.
            </p>
            <pre>
<samp>
&lt;cons id="4382"&gt;
    &lt;firstname&gt;Bob&lt;/firstname&gt;
    &lt;lastname&gt;Smith&lt;/lastname&gt;
    &lt;is_banned&gt;0&lt;/is_banned&gt;
    &lt;create_dt&gt;1168146000&lt;/create_dt&gt;

    <strong>&lt;cons_group id="17" /&gt;
    &lt;cons_group id="41" /&gt;</strong>

&lt;/cons&gt;
</samp>
</pre>
            <div>
                <p>
                    Attribute Definitions:
                </p>
                <dl>
                    <dt>
                        id:
                    </dt>
                    <dd>
                        integer
                    </dd>
                    <dt>
                        name:
                    </dt>
                    <dd>
                        string, up to 255 characters
                    </dd>
                </dl>
            </div>
            <h5>
                Data Bundle: <code>cons_phone</code> and <code>primary_cons_phone</code>
            </h5>
            <p>
                The <code>cons_phone</code> and <code>primary_cons_phone</code> data bundles add phone numbers to a constituent record. <code>primary_cons_phone</code> will add zero or one phone number elements, choosing the most recently added phone number as the primary one. Incoming <code>cons_phone</code> records do not contain IDs; they are automatically associated with the cons record they are nested in. Example (with two phone numbers):
            </p>
            <pre>
<samp>
&lt;cons id="4382"&gt;
    &lt;firstname&gt;Bob&lt;/firstname&gt;
    &lt;lastname&gt;Smith&lt;/lastname&gt;
    &lt;is_banned&gt;0&lt;/is_banned&gt;
    &lt;create_dt&gt;1168146000&lt;/create_dt&gt;

    <strong>&lt;cons_phone id="6933" modified_dt="1168146030"&gt;
        &lt;phone&gt;4082345678&lt;/phone&gt;
        &lt;phone_type&gt;home&lt;/phone_type&gt;
        &lt;is_primary&gt;0&lt;/is_primary&gt;
        &lt;is_subscribed&gt;0&lt;/is_subscribed&gt;
    &lt;/cons_phone&gt;
    &lt;cons_phone id="7322" modified_dt="1173243602"&gt;
        &lt;phone&gt;4089876432&lt;/phone&gt;
        &lt;phone_type&gt;mobile&lt;/phone_type&gt;
        &lt;is_primary&gt;1&lt;/is_primary&gt;
        &lt;is_subscribed&gt;1&lt;/is_subscribed&gt;
    &lt;/cons_phone&gt;</strong>

&lt;/cons&gt;
</samp>
</pre>
            <div>
                <p>
                    Field Definitions:
                </p>
                <dl>
                    <dt>
                        phone:
                    </dt>
                    <dd>
                        string, up to 30 characters
                    </dd>
                    <dt>
                        phone_type:
                    </dt>
                    <dd>
                        constant string, <code>home</code>, <code>work</code>, <code>mobile</code>, <code>fax</code>, <code>unknown</code>, or empty
                    </dd>
                    <dt>
                        is_primary:
                    </dt>
                    <dd>
                        integer, 0 or 1
                    </dd>
                    <dt>
                        is_subscribed:
                    </dt>
                    <dd>
                        integer, 0 or 1
                    </dd>
                </dl>
            </div>
            <h5>
                Data Bundle: <code>ext_X</code>
            </h5>
            <p>
                The <code>ext_X</code> bundle allows the inclusion of external IDs (as set with the <code>set_ext_ids</code> API call). X is to be replaced with the external ID type specified on the <code>set_ext_ids</code> call. The following example assumes that a data bundle of ext_van_id was specified and that an external ID of type van_id had been specified for the resulting constituent record:
            </p>
            <pre>
<samp>
&lt;cons id="4382"&gt;
    &lt;firstname&gt;Bob&lt;/firstname&gt;
    &lt;lastname&gt;Smith&lt;/lastname&gt;
    &lt;is_banned&gt;0&lt;/is_banned&gt;
    &lt;create_dt&gt;1168146000&lt;/create_dt&gt;

    <strong>&lt;ext_id type="van_id" id="vfgw3811wf28342" /&gt;</strong>

&lt;/cons&gt;
</samp>
</pre>
            <div>
                <p>
                    Attribute Definitions:
                </p>
                <dl>
                    <dt>
                        type:
                    </dt>
                    <dd>
                        string, up to 100 characters
                    </dd>
                    <dt>
                        id:
                    </dt>
                    <dd>
                        string, up to 255 characters
                    </dd>
                </dl>
            </div>
            <h4>
                Exported XML Constituent Group Records
            </h4>
            <p>
                Constituent groups are sets of constituents with some common attribute or grouping. The core constituent group is small, similar to the core constituent record. Also like the core constituent record, the constituent group record contains a modified_dt attribute. This contains the date and time (in epoch seconds) when a constituent group (or its membership) was last modified.
            </p>
            <h5>
                Core: <code>cons_group</code>
            </h5>
            <p>
                The core constituent group record contains only the <code>name</code>, <code>slug</code>, <code>group_type</code>, and description.
            </p>
            <pre>
<samp>
&lt;cons_group id="42" modified_dt="1171861200"&gt;
    &lt;name&gt;First Quarter Donors&lt;/name&gt;
    &lt;slug&gt;q1donors&lt;/slug&gt;
    &lt;description&gt;People who donated in Q1 2007&lt;/description&gt;
    &lt;is_banned&gt;0&lt;/is_banned&gt;
    &lt;create_dt&gt;1168146000&lt;/create_dt&gt;
    &lt;group_type&gt;manual&lt;/group_type&gt;
    &lt;members&gt;162&lt;/members&gt;
    &lt;unique_emails&gt;164&lt;/unique_emails&gt;
    &lt;unique_emails_subscribed&gt;109&lt;/unique_emails_subscribed&gt;
    &lt;count_dt&gt;1213861583&lt;/count_dt&gt;
&lt;/cons_group&gt;
</samp>
</pre>
            <h4>
                Incoming XML Constituent Group Records
            </h4>
            <p>
                The API allow for incoming constituent group data in similar formats to outgoing data. Incoming data must be encoded as UTF-8.
            </p>
            <h5>
                Core: <code>cons_group</code>
            </h5>
            <p>
                The core constituent group record contains only the name, slug, group type, and description.
            </p>
            <pre>
<samp>
&lt;cons_group&gt;
    &lt;name&gt;Third Quarter Fundraisers&lt;/name&gt;
    <em>&lt;slug&gt;q3fundraisers&lt;/slug&gt;
    &lt;description&gt;People raising money for Q3 2007&lt;/description&gt;
    &lt;group_type&gt;manual&lt;/group_type&gt;
    &lt;create_dt&gt;1168146000&lt;/create_dt&gt;</em>
&lt;/cons_group&gt;
</samp>
</pre>
            <ul>
                <li>In incoming data, no id is specified. The <code>cons_group</code>_id that was assigned to the new group is returned on successful requests.
                </li>
                <li>
                    <code>slug</code> is optional; the default is no slug.
                </li>
                <li>
                    <code>description</code> is optional; the default is no description.
                </li>
                <li>
                    <code>group_type</code> is optional; the default is "manual".
                </li>
                <li>
                    <code>create_dt</code> is optional; the default is the current time.
                </li>
            </ul>
            <div>
                <p>
                    Field Definitions:
                </p>
                <dl>
                    <dt>
                        name:
                    </dt>
                    <dd>
                        string, up to 255 characters
                    </dd>
                    <dt>
                        slug:
                    </dt>
                    <dd>
                        string, up to 32 characters
                    </dd>
                    <dt>
                        description:
                    </dt>
                    <dd>
                        text
                    </dd>
                    <dt>
                        group_type:
                    </dt>
                    <dd>
                        string, up to 32 characters
                    </dd>
                    <dt>
                        create_dt:
                    </dt>
                    <dd>
                        unix timestamp
                    </dd>
                </dl>
            </div>
            <h4>
                Exported XML Signup Form Records
            </h4>
            <h5>
                Core: <code>count_by_field</code>
            </h5>
            <p>
                The core count_by_field record contains the number of constituents that filled out a certain signup form field:
            </p>
            <pre>
<samp>
&lt;signup_form_field&gt;
    &lt;value&gt;form_value2&lt;/value&gt;
    &lt;count&gt;2&lt;/count&gt;
&lt;/signup_form_field&gt;
</samp>
</pre>
            <h5>
                Core: <code>signup_count</code>
            </h5>
            <p>
                The core signup_count record contains the only the number of constituents that matched the query:
            </p>
            <pre>
<samp>
&lt;count&gt;6&lt;/count&gt;
</samp>
</pre>
            <h5>
                Core: <code>signup_form</code>
            </h5>
            <p>
                The core signup_form record contains the signup_form_name, slug, public title, and creation date:
            </p>
            <pre>
<samp>
&lt;signup_form id="1" modified_dt="1267728690"&gt;
    &lt;signup_form_name&gt;Default Signup Form&lt;/signup_form_name&gt;
    &lt;signup_form_slug&gt;slug&lt;/signup_form_slug&gt;
    &lt;form_public_title&gt;public title&lt;/form_public_title&gt;
    &lt;create_dt&gt;2010-02-08 18:33:11&lt;/create_dt&gt;
&lt;/signup_form&gt;
</samp>
</pre>
            <h5>
                Core: <code>signup_form_field</code>
            </h5>
            <p>
                The core signup_form_field record contains the label, description, display_order, is_shown, is_required, is_custom_field, cons_field_id, and creation date:
            </p>
            <pre>
<samp>
&lt;signup_form_field id="84"&gt;
    &lt;format&gt;1&lt;/format&gt;
    &lt;label&gt;Last Name&lt;/label&gt;
    &lt;description&gt;Last Name&lt;/description&gt;
    &lt;display_order&gt;1&lt;/display_order&gt;
    &lt;is_shown&gt;1&lt;/is_shown&gt;
    &lt;is_required&gt;0&lt;/is_required&gt;
    &lt;break_after&gt;0&lt;/break_after&gt;
    &lt;is_custom_field&gt;0&lt;/is_custom_field&gt;
    &lt;cons_field_id&gt;0&lt;/cons_field_id&gt;
    &lt;create_dt&gt;2010-02-08 18:33:11&lt;/create_dt&gt;
    &lt;extra_def&gt;&lt;/extra_def&gt;
&lt;/signup_form_field&gt;
</samp>
</pre>
            <p>
                Extra_def will have the following elements inside of it:
            </p>
            <p>
                For textboxes
            </p>
            <pre>
<samp>
&lt;size&gt;10&lt;/size&gt;
&lt;maxlength&gt;10&lt;/maxlength&gt;
&lt;rule&gt;10&lt;/rule&gt;
</samp>
</pre>
            <p>
                For textareas
            </p>
            <pre>
<samp>
&lt;cols&gt;10&lt;/cols&gt;
&lt;rows&gt;10&lt;/rows&gt;
</samp>
</pre>
            <p>
                For dropdown lists
            </p>
            <pre>
<samp>
&lt;options&gt;one|1
two|2&lt;/options&gt;
&lt;columns&gt;10&lt;/columns&gt;
</samp>
</pre>
            <p>
                For checkbox lists
            </p>
            <pre>
<samp>
&lt;options&gt;one|1
two|2&lt;/options&gt;
&lt;columns&gt;10&lt;/columns&gt;
</samp>
</pre>
            <p>
                For radio button lists
            </p>
            <pre>
<samp>
&lt;options&gt;one|1
two|2&lt;/options&gt;
&lt;columns&gt;10&lt;/columns&gt;
</samp>
</pre>
            <p>
                For static fields
            </p>
            <pre>
<samp>
&lt;text&gt;hi&lt;/text&gt;
</samp>
</pre>
            <p>
                For binary checkboxes get "binarycheckbox_email" added to the main bundle, along with:
            </p>
            <pre>
<samp>
&lt;desc&gt;user facing description&lt;/desc&gt;
&lt;default&gt;0&lt;/default&gt;
&lt;cons_group_id&gt;0&lt;/cons_group_id&gt;
</samp>
</pre>
            <p>
                For upload fields
            </p>
            <pre>
<samp>
&lt;file_size&gt;100&lt;/file_size&gt;
&lt;file_types&gt;
    &lt;file_type&gt;application/msword:doc&lt;file_type&gt;
&lt;/file_types&gt;
</samp>
</pre>
            <p>
                For autocomplete fields
            </p>
            <pre>
<samp>
&lt;size&gt;3&lt;/size&gt;
&lt;maxlength&gt;100&lt;/maxlength&gt;
&lt;source&gt;http://www.bluestatedigital.com/source/to/text/file&lt;/source&gt;
&lt;must_match&gt;1&lt;/must_match&gt;
</samp>
</pre>
            <p>
                For hidden fields, nothing is added
            </p>
            <h4>
                Exported XML Signup Records
            </h4>
            <h5>
                Core: <code>stg_signups</code>
            </h5>
            <p>
                The stg_signups includes stg_signup records. If there are stg_signup_extras associated with the stg_signup record they are included as children of the stg_signup record. If there are associated stg_signup_extra_values associated with the stg_signup_extra records they are included as children of the stg_signup_extra record.
            </p>
            <pre>
<samp>
&lt;stg_signups&gt;
    &lt;stg_signup stg_signup_id="1"&gt;
        &lt;stg_signup_id&gt;1&lt;/stg_signup_id&gt;
        &lt;signup_form_id/&gt;
        &lt;cons_upload_id&gt;1&lt;/cons_upload_id&gt;
        &lt;stg_invitation_id/&gt;
        &lt;cons_source_id&gt;6&lt;/cons_source_id&gt;
        &lt;ext_id/&gt;
        &lt;ext_type/&gt;
        &lt;mailing_recipient_id/&gt;
        &lt;mailing_link_id/&gt;
        &lt;stg_contribution_id/&gt;
        &lt;spk_letter_id/&gt;
        &lt;email&gt;email@email.com&lt;/email&gt;
        &lt;work_email/&gt;
        &lt;email_opt_in&gt;1&lt;/email_opt_in&gt;
        &lt;prefix/&gt;
        &lt;firstname&gt;First&lt;/firstname&gt;
        &lt;middlename/&gt;
        &lt;lastname&gt;Last&lt;/lastname&gt;
        &lt;suffix/&gt;
        &lt;salutation/&gt;
        &lt;gender/&gt;
        &lt;birth_dt/&gt;
        &lt;title/&gt;
        &lt;employer/&gt;
        &lt;occupation/&gt;
        &lt;income/&gt;
        &lt;mobile_opt_in/&gt;
        &lt;mobile/&gt;
        &lt;phone/&gt;
        &lt;work_phone/&gt;
        &lt;fax/&gt;
        &lt;addr1&gt;1 Some Street&lt;/addr1&gt;
        &lt;addr2/&gt;
        &lt;addr3/&gt;
        &lt;city&gt;Somewhere&lt;/city&gt;
        &lt;state_cd&gt;XX&lt;/state_cd&gt;
        &lt;zip&gt;00000&lt;/zip&gt;
        &lt;zip_4&gt;0000&lt;/zip_4&gt;
        &lt;country/&gt;
        &lt;transaction_amt/&gt;
        &lt;transaction_dt/&gt;
        &lt;contribution_status/&gt;
        &lt;source/&gt;
        &lt;subsource/&gt;
        &lt;is_contrib_recurring/&gt;
        &lt;opt_in/&gt;
        &lt;create_dt&gt;2012-07-30 03:12:13&lt;/create_dt&gt;
        &lt;create_app&gt;constituent upload&lt;/create_app&gt;
        &lt;create_user/&gt;
        &lt;create_ip_addr/&gt;
        &lt;create_user_agent/&gt;
        &lt;status&gt;2&lt;/status&gt;
        &lt;note&gt;1&lt;/note&gt;
        &lt;chapter_id&gt;1&lt;/chapter_id&gt;
        &lt;stg_signup_extra stg_signup_extra_id="1"&gt;
            &lt;stg_signup_extra_id&gt;1&lt;/stg_signup_extra_id&gt;
            &lt;stg_signup_id&gt;1&lt;/stg_signup_id&gt;
            &lt;signup_form_field_id&gt;0&lt;/signup_form_field_id&gt;
            &lt;nonsignup_cons_field_id&gt;1&lt;/nonsignup_cons_field_id&gt;
            &lt;create_dt&gt;2012-07-30 03:12:14&lt;/create_dt&gt;
            &lt;create_app&gt;constituent upload&lt;/create_app&gt;
            &lt;create_user/&gt;
            &lt;modified_dt/&gt;
            &lt;modified_app/&gt;
            &lt;modified_user/&gt;
            &lt;status&gt;1&lt;/status&gt;
            &lt;note/&gt;
            &lt;stg_signup_extra_value stg_signup_extra_value_id="1"&gt;
                &lt;stg_signup_extra_value_id&gt;1&lt;/stg_signup_extra_value_id&gt;
                &lt;stg_signup_extra_id&gt;1&lt;/stg_signup_extra_id&gt;
                &lt;value_varchar&gt;Test&lt;/value_varchar&gt;
                &lt;value_text/&gt;
                &lt;value_storage_key/&gt;
                &lt;create_dt&gt;2012-07-30 03:12:15&lt;/create_dt&gt;
                &lt;create_app&gt;constituent upload&lt;/create_app&gt;
                &lt;create_user/&gt;
                &lt;modified_dt/&gt;
                &lt;modified_app/&gt;
                &lt;modified_user/&gt;
                &lt;status&gt;1&lt;/status&gt;
                &lt;note/&gt;
            &lt;/stg_signup_extra_value&gt;
        &lt;/stg_signup_extra&gt;
    &lt;/stg_signup&gt;
&lt;/stg_signups&gt;
</samp>
</pre>
            <h4>
                Exported XML Wrapper Records
            </h4>
            <h5>
                Core: <code>wrappers</code>
            </h5>
            <p>
                The core wrapper record contains the name, description, and creation date:
            </p>
            <pre>
<samp>
&lt;wrapper id="1" modified_dt="1265250882"&gt;
    &lt;name&gt;default wrapper&lt;/name&gt;
    &lt;description&gt;This is the default wrapper&lt;/description&gt;
    &lt;create_dt&gt;2010-02-04 02:34:42&lt;/create_dt&gt;
&lt;/wrapper&gt;
</samp>
</pre>
            <h4>
                JSON Event Blobs
            </h4>
            <h5>
                Event: <code>list_rsvps</code>
            </h5>
            <p>
                An HTTP response code and a JSON array of objects, each of which represents an an RSVP (ie one row of the event_attendee table). The response will contains the all available parameters from the internal representaion of the attendee. A field's value is the empty string or <code>null</code> if it has not been set.
            </p>
            <pre>
<samp>
<strong>HTTP/1.0 200 OK</strong>
date: Sun, 08 Jan 2012 16:46:51 GMT
content-length: 1533
content-type: application/json; charset=utf-8
connection: close
server: Apache/2.0.63 (Unix) mod_ssl/2.0.63 OpenSSL/0.9.8e-fips-rhel5

{
    "attendees": [
        {
            "addr1": null,
            "addr2": null,
            "addr3": null,
            "attendee_cons_id": "12",
            "city": "winnetka",
            "comment": null,
            "country": null,
            "email": "user@example.com",
            "event_id": "11",
            "firstname": "User",
            "guests": "0",
            "is_potential_volunteer": "0",
            "is_reminder_sent": "0",
            "lastname": null,
            "phone": null,
            "pledge_amt": null,
            "pledge_method": null,
            "shift_ids": "",
            "state_cd": "IL",
            "will_attend": "0",
            "zip": "60093",
            "zip_4": null
        }
    ]
}

</samp>
</pre>
            <h4>
                Bundle Specification
            </h4>
            <p>
                Most API calls that return constituent records have an optional <code>bundles</code> parameter. This parameter allows you to specify which data bundles you want included in the result set. Bundle names are separated by commas. If a bundle is required (e.g. constituent records without any data for that bundle should not be returned at all), the bundle name should have an asterisk appended to it.
            </p>
            <p>
                The following <code>bundles</code> parameter would include all address, phone, email information, and an activist score for each constituent:
            </p>
            <p>
                <code>cons_addr,cons_phone,cons_email</code>
            </p>
            <p>
                The following <code>bundles</code> parameter would include all address, phone, and email information for each constituent, but would exclude any constituent records that did not have a phone number:
            </p>
            <p>
                <code>cons_addr,cons_phone*,cons_email</code>
            </p>
            <h4>
                Filter Specification
            </h4>
            <p>
                Some constituent API calls can also take a filter parameter. This allows you to further limit the records returned by a particular API call. Filter parameters take the form of a comma-separated list of logical tests. These tests are ANDed together. Most logical tests involve an operator (<kbd>=</kbd>, <kbd>&gt;</kbd>, <kbd>&lt;</kbd>). Some Boolean tests have no operator and no right hand side.
            </p>
            <p>
                If the <kbd>=</kbd> operator is used, multiple values can be specified on the right hand side of the logical test by separating them with commas and enclosing them in parentheses.
            </p>
            <p>
                Note that the filter mechanism is not intended to provide sophisticated query building capabilities. Rather, it's purpose is to reduce the size of a result set to be more manageable. Further filtering and querying can be done by the code making the API call after the larger result set has been received and processed.
            </p>
            <p>
                The following criteria are supported in a filter specification.
            </p>
            <dl>
                <dt>
                    <code>state_cd</code>
                </dt>
                <dd>
                    <dl>
                        <dt>
                            Description:
                        </dt>
                        <dd>
                            The constituent has at least one address record in the state(s) specified.
                        </dd>
                        <dt>
                            Supported operators:
                        </dt>
                        <dd>
                            <kbd>=</kbd>
                        </dd>
                        <dt>
                            Example:
                        </dt>
                        <dd>
                            <samp>state_cd=(IA,NV,NH,SC)</samp>
                        </dd>
                    </dl>
                </dd>
                <dt>
                    <code>primary_state_cd</code>
                </dt>
                <dd>
                    <dl>
                        <dt>
                            Description:
                        </dt>
                        <dd>
                            The constituent's primary address record is in the state(s) specified.
                        </dd>
                        <dt>
                            Supported operators:
                        </dt>
                        <dd>
                            <kbd>=</kbd>
                        </dd>
                        <dt>
                            Example:
                        </dt>
                        <dd>
                            <samp>primary_state_cd=IA</samp>
                        </dd>
                    </dl>
                </dd>
                <dt>
                    <code>is_subscribed</code>
                </dt>
                <dd>
                    <dl>
                        <dt>
                            Description:
                        </dt>
                        <dd>
                            The constituent has at least one email address that is marked as subscribed.
                        </dd>
                        <dt>
                            Supported operators:
                        </dt>
                        <dd>
                            None
                        </dd>
                        <dt>
                            Example:
                        </dt>
                        <dd>
                            <samp>is_subscribed</samp>
                        </dd>
                    </dl>
                </dd>
                <dt>
                    <code>has_account</code>
                </dt>
                <dd>
                    <dl>
                        <dt>
                            Description:
                        </dt>
                        <dd>
                            The constituent has a login account/password set up.
                        </dd>
                        <dt>
                            Supported operators:
                        </dt>
                        <dd>
                            None
                        </dd>
                        <dt>
                            Example:
                        </dt>
                        <dd>
                            <samp>has_account</samp>
                        </dd>
                    </dl>
                </dd>
                <dt>
                    <code>has_mobile_phone</code>
                </dt>
                <dd>
                    <dl>
                        <dt>
                            Description:
                        </dt>
                        <dd>
                            The constituent has a mobile phone number.
                        </dd>
                        <dt>
                            Supported operators:
                        </dt>
                        <dd>
                            None
                        </dd>
                        <dt>
                            Example:
                        </dt>
                        <dd>
                            <samp>has_mobile_phone</samp>
                        </dd>
                    </dl>
                </dd>
                <dt>
                    <code>signup_form_id</code>
                </dt>
                <dd>
                    <dl>
                        <dt>
                            Description:
                        </dt>
                        <dd>
                            The constituents have filled out the signup form specified.
                        </dd>
                        <dt>
                            Supported operators:
                        </dt>
                        <dd>
                            =
                        </dd>
                        <dt>
                            Example:
                        </dt>
                        <dd>
                            <samp>signup_form_id=18</samp>
                        </dd>
                    </dl>
                </dd>
                <dt>
                    <code>email</code>
                </dt>
                <dd>
                    <dl>
                        <dt>
                            Description:
                        </dt>
                        <dd>
                            The constituent's email matches the email specified.
                        </dd>
                        <dt>
                            Supported operators:
                        </dt>
                        <dd>
                            =
                        </dd>
                        <dt>
                            Example:
                        </dt>
                        <dd>
                            <samp>email=username@gmail.com</samp>
                        </dd>
                    </dl>
                </dd>
                <dt>
                    <code>cons_group</code>
                </dt>
                <dd>
                    <dl>
                        <dt>
                            Description:
                        </dt>
                        <dd>
                            The constituent is part of one of the specified cons_groups.
                        </dd>
                        <dt>
                            Supported operators:
                        </dt>
                        <dd>
                            <kbd>=</kbd>
                        </dd>
                        <dt>
                            Example:
                        </dt>
                        <dd>
                            <samp>cons_group=(1,20,300)</samp>
                        </dd>
                    </dl>
                </dd>
                <dt>
                    <code>not_cons_group</code>
                </dt>
                <dd>
                    <dl>
                        <dt>
                            Description:
                        </dt>
                        <dd>
                            The constituent has to not be part of one of the specified cons_groups.
                        </dd>
                        <dt>
                            Supported operators:
                        </dt>
                        <dd>
                            <kbd>=</kbd>
                        </dd>
                        <dt>
                            Example:
                        </dt>
                        <dd>
                            <samp>not_cons_group=(1,20,300)</samp>
                        </dd>
                    </dl>
                </dd>
            </dl>
            <p>
                The following example filter would return only constituent records in the state of California or Nevada that had a subscribed email address:
            </p>
            <pre>
<samp>
state_cd=(CA,NV),is_subscribed,cons_group=20
</samp>
</pre>
            <p>
                Because filters contain the <kbd>=</kbd> sign and other characters which have significance in URLs, they must be encoded when being specified as a API call parameter. The above example when used in a URL and properly encoded, might look like:
            </p>
            <pre>
<samp>
http://XYZ/page/api/foo?<strong>filter=state_cd%3D%28CA%2CNV%29%2Cis_subscribed%2Ccons_group%3D20</strong>&amp;api_id=X&amp;api_mac=Y&amp;api_ts=N
</samp>
</pre>
        </div>
        <div class="major_section">
            <h3>
                API Reference
            </h3>
            <h4>
                Account API Calls
            </h4><!-- check_credentials -->
            <div class="api_method">
                <h5>
                    <code>check_credentials</code>
                </h5>
                <div class="description">
                    <p>
                        Allows a client to send user credentials to the API and see if those credentials are valid. If valid, the API will return a constituent record.
                    </p>
                    <p>
                        <strong>Important: because of the sensitive nature of accounts and passwords, this API method must <em>always</em> be accessed over HTTPS.</strong>
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/account/check_credentials
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>userid</code> (required)
                        </dt>
                        <dd>
                            The email address of the user to check
                        </dd>
                        <dt>
                            <code>password</code> (required)
                        </dt>
                        <dd>
                            The user's password
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns a constituent record with primary_cons_email and cons_addr bundles if successful (see the Common Record Formats section for details). If not, HTTP status code 403 Forbidden returned.
                    </p>
                    <p>
                        Please note that the authentication checks are rate limited by IP address and userid. Three unsuccessful logins will lock the user/IP from any attempts for 30 seconds.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <pre>
<samp>
https://XYZ/page/api/account/check_credentials?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=61722c6a82a16779c956992e351ae1582f4a3cc1&amp;userid=user1%40domain.com&amp;password=password1
</samp>
</pre>
            </div>
        </div><!-- create_account -->
        <div class="api_method">
            <h5>
                <code>create_account</code>
            </h5>
            <div class="description">
                <p>
                    This method allows creating a new frontend user account. If a user with the same email already exists, the system will check to see if the submitted password matches the existing password. If it does, the existing constituent record will be returned. Otherwise an error will be returned noting the email address as already taken.
                </p>
                <p>
                    <strong>Important: because of the sensitive nature of accounts and passwords, this API method must <em>always</em> be accessed over HTTPS.</strong>
                </p>
            </div>
            <p class="http_method">
                Method: <code>GET</code> or <code>POST</code>
            </p>
            <p class="url">
                URL: /page/api/account/create_account
            </p>
            <div class="parameters">
                <p>
                    Parameters:
                </p>
                <dl>
                    <dt>
                        <code>firstname</code> (required)
                    </dt>
                    <dd>
                        The user's first name
                    </dd>
                    <dt>
                        <code>lastname</code> (required)
                    </dt>
                    <dd>
                        The user's last name
                    </dd>
                    <dt>
                        <code>email</code> (required)
                    </dt>
                    <dd>
                        The user's email address. This will also be their username.
                    </dd>
                    <dt>
                        <code>password</code> (required)
                    </dt>
                    <dd>
                        The password to set for the new account
                    </dd>
                    <dt>
                        <code>zip</code> (required)
                    </dt>
                    <dd>
                        The user's zip or postal code
                    </dd>
                </dl>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
                <p>
                    Returns the new or existing constituent record with primary_cons_email and cons_addr bundles (see the Common Record Formats section for details).
                </p>
            </div>
            <p class="deferred">
                Deferred Results: Never
            </p>
            <p>
                Example:
            </p>
            <pre>
<samp>
https://XYZ/page/api/account/create_account?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=61722c6a82a16779c956992e351ae1582f4a3cc1&amp;firstname=joe&amp;lastname=user&amp;email=joe%40example.com&amp;password=******&amp;zip=01234
</samp>
</pre>
        </div><!-- reset_password -->
        <div class="api_method">
            <h5>
                <code>reset_password</code>
            </h5>
            <div class="description">
                <p>
                    Allows a client to request that a user's password be reset. If the userid is valid, the user will receive a new password via an email that is identical to the email sent when someone fills out the Forgot Password form on the website.
                </p>
            </div>
            <p class="http_method">
                Method: <code>GET</code>
            </p>
            <p class="url">
                URL: /page/api/account/reset_password
            </p>
            <div class="parameters">
                <p>
                    Parameters:
                </p>
                <dl>
                    <dt>
                        <code>userid</code> (required)
                    </dt>
                    <dd>
                        The email address of the user requesting the password reset.
                    </dd>
                </dl>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
                <p>
                    Returns 204 No Content except in the case of general API errors (e.g. authentication or missing parameters).
                </p>
            </div>
            <p class="deferred">
                Deferred Results: Never
            </p>
            <p>
                Example:
            </p>
            <pre>
<samp>
http://XYZ/page/api/account_password?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=61722c6a82a16779c956992e351ae1582f4a3cc1&amp;userid=user1%40domain.com
</samp>
</pre>
        </div><!-- Set_password -->
        <div class="api_method">
            <h5>
                <code>set_password</code>
            </h5>
            <div class="description">
                <p>
                    This method allows a client to set a user's password. If the userid is valid, the user will have their password reset, if the user is invalid no error will be thrown but nothing will be set.
                </p>
                <p>
                    <strong>Important: because of the sensitive nature of accounts and passwords, this API method must <em>always</em> be accessed over HTTPS.</strong>
                </p>
            </div>
            <p class="http_method">
                Method: <code>GET</code> or <code>POST</code>
            </p>
            <p class="url">
                URL: /page/api/account/set_password
            </p>
            <div class="parameters">
                <p>
                    Parameters:
                </p>
                <dl>
                    <dt>
                        <code>userid</code> (required)
                    </dt>
                    <dd>
                        The email address of the user
                    </dd>
                    <dt>
                        <code>password</code> (required)
                    </dt>
                    <dd>
                        The user's new password
                    </dd>
                </dl>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
                <p>
                    Returns 204 No Content except in the case of general API errors (e.g. authentication or missing parameters).
                </p>
            </div>
            <p class="deferred">
                Deferred Results: Never
            </p>
            <p>
                Example:
            </p>
            <pre>
<samp>
https://XYZ/page/api/account/set_password?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=61722c6a82a16779c956992e351ae1582f4a3cc1&amp;userid=user1%40domain.com&amp;password=password1
</samp>
</pre>
        </div>
        <h4>
            Branches API Calls
        </h4>
        <div class="api_method">
            <h5>
                <code>list_branches</code>
            </h5>
            <div class="description">
                <p>
                    Retrieve a list of all branches
                </p>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/api/branches/list_branches
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                </div>
                <dl>
                    <dt>
                        <code>No parameters</code>
                    </dt>
                </dl>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        An XML-formatted description of zero or more branches:
                    </p>
                    <pre>
        <samp>
&lt;?xml version="1.0"?&gt;
&lt;api&gt;
    &lt;branch id="1" modified_dt="2013-01-03 15:51:53"&gt;
        &lt;name&gt;Test Chapter #1&lt;/name&gt;
        &lt;create_dt&gt;2013-01-03 15:51:53&lt;/create_dt&gt;
    &lt;/branch&gt;
    &lt;branch id="2" modified_dt="2013-01-03 15:51:53"&gt;
        &lt;name&gt;Test Chapter #2&lt;/name&gt;
        &lt;create_dt&gt;2013-01-03 15:51:53&lt;/create_dt&gt;
    &lt;/branch&gt;
&lt;/api&gt;
        </samp>

</pre>
                    <p>
                        Returns zero or more branch records (see Common Record Formats).
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <div>
                    <samp>http://XYZ/page/api/branches/list_branches?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298526&amp;api_mac=ecf927ac5dbcb825068838e5019f24204dabb0e6</samp>
                </div>
            </div>
            <h4>
                Action API Calls
            </h4><!-- check_credentials -->
            <div class="api_method">
                <h5>
                    <code>add</code>
                </h5>
                <div class="description">
                    <p>
                        Allows a client to add an action with source codes and attributes for a particular guid (which may or may not represent an existing cons)
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/action/add
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>Note: the parameters for this API are expected to be sent as JSON, not form-urlencoded. See the example request below.
                    <dl>
                        <dt>
                            <code>guid</code> (required)
                        </dt>
                        <dd>
                            String. The guid of the user performing the action, either from the cookie 'guid' value, the cons' guid or a newly created guid
                        </dd>
                        <dt>
                            <code>name</code> (required)
                        </dt>
                        <dd>
                            String. The name of the action
                        </dd>
                        <dt>
                            <code>attributes</code> (required)
                        </dt>
                        <dd>
                            JSON map. A set of key-value pairs representing attributes of the action performed
                        </dd>
                        <dt>
                            <code>sourceCode</code> (required)
                        </dt>
                        <dd>
                            JSON map. A set of key-value pairs representing the source of the action performed
                        </dd>
                        <dt>
                            <code>timestamp</code> (optional)
                        </dt>
                        <dd>
                            Integer. The time at which the action occurred, in UNIX timestamp format (seconds since Jan 1, 1970)
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        The UUIDv1 of the action created
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <pre>
<samp>
POST /page/api/action/add?api_ver=1&amp;api_id=testfetch&amp;api_ts=1317752440&amp;api_mac=f4b0088409836fa79004c5cfdb7cff5ba6b4aea1 HTTP/1.1
Host: example.bluestatedigital.com
Accept-Encoding: identity
Content-Length: 149
Content-type: application/json

{
  "guid":"36wQOB1IBoVOhIJVT73Xa4A",
  "name":"testActionName",
  "attributes":{
    "attrKey1":"attrValue1"
  },
  "sourceCodes":{
    "sourceCodeKey1":"sourceCodeValue1"
  }
}
</samp>
</pre><br>
                <pre>
<samp>
reply: 'HTTP/1.1 200 OK'
Date: Tue, 04 Oct 2011 18:20:40 GMT
Server: Apache/2.0.63 (Unix) mod_ssl/2.0.63 OpenSSL/0.9.7g PHP/5.2.6
X-Powered-By: PHP/5.2.6
Content-Length: 36
Content-Type: application/json; charset=utf-8

24f250c0-1dd5-11b2-9773-49ed3101b68c
</samp>
</pre>
            </div><!-- check_credentials -->
            <div class="api_method">
                <h5>
                    <code>get</code>
                </h5>
                <div class="description">
                    <p>
                        Allows a client to retrieve a list of actions performed by a particular guid
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/action/get
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>guid</code> (required)
                        </dt>
                        <dd>
                            String. The guid of the user performing the action, either from the cookie 'guid' value, the cons' guid or a newly created guid
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        JSON list of actions, which are returned as JSON maps.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <pre>
<samp>
GET /page/api/action/get?guid=36wQOB1IBoVOhIJVT73Xa4A&amp;api_ver=1&amp;api_id=testfetch&amp;api_ts=1317752808&amp;api_mac=3939374b345a28c851316273ba297b7cf143b84f HTTP/1.1
Host: example.bluestatedigital.com
</samp>
</pre><br>
                <pre>
<samp>
Date: Tue, 04 Oct 2011 18:26:48 GMT
Server: Apache/2.0.63 (Unix) mod_ssl/2.0.63 OpenSSL/0.9.7g PHP/5.2.6
X-Powered-By: PHP/5.2.6
Content-Length: 2322
Content-Type: application/json; charset=utf-8

[
  {
    "guid":"36wQOB1IBoVOhIJVT73Xa4A",
    "name":"testActionName",
    "attributes":{
      "attrKey1":"attrValue1"
    },
    "sourceCodes":{
      "sourceCodeKey1":"sourceCodeValue1"
    },
    "id":"24f0eff0-1dd5-11b2-8563-e5a8347227c7"
  }
]
</samp>
</pre>
            </div>
            <h4>
                Circle (circle) API Calls
            </h4><!-- list_circles -->
            <div class="api_method">
                <h5>
                    <code>list_circles</code>
                </h5>
                <div class="description">
                    <p>
                        Fetches a list of all circles in the system.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/circle/list_circles
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>circle_type</code> (optional)
                        </dt>
                        <dd>
                            If circle_type is present, list_circles will return all circles which are of type 'circle_type'. 'circle_type' can be '0', for basic circles; or '1', for field circles.
                        </dd>
                        <dt>
                            <code>state_cd</code> (optional)
                        </dt>
                        <dd>
                            If state_cd is present, list_circles will return all circles which have zip codes in the specified state_cd.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns one <code>&lt;circle&gt;</code> element (see Common Record Formats) for each circle in the system.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <pre>
<samp>
http://XYZ/page/api/circle/list_circles?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=3c3452091bf7ff0e2be8b638694a187ca7c0a013
</samp>
</pre>
            </div><!-- get_cons_ids_for_circle -->
            <div class="api_method">
                <h5>
                    <code>get_cons_ids_for_circle</code>
                </h5>
                <div class="description">
                    <p>
                        This method takes a circle id and returns a list of the constituent ids in that circle. The result is a text file with one constituent id per line.
                    </p>
                    <p>
                        <strong>Important: because of the potential time involved in manipulating circles, the final result code of this call is always deferred.</strong>
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/circle/get_cons_ids_for_circle
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>circle_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the circle to list membership of.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/circle/get_cons_ids_for_circle?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171&amp;circle_id=42
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
circle_id=42
</samp>
</pre>
            </div><!-- get_ext_ids_for_circle -->
            <div class="api_method">
                <h5>
                    <code>get_ext_ids_for_circle</code>
                </h5>
                <div class="description">
                    <p>
                        External IDs (<code>ext_id</code>) are primary key identifiers associated with an external system or service. These can be stored alongside their corresponding Blue State Digital constituent records for easy access and reference. The external ID type (ext_type) specifies which external service/system a given ID is associated with. This method allows retrieving the members of a circle as a list of their external ids. Any circle members who do not have an external id of the requested type will not be included in the list.
                    </p>
                    <p>
                        <strong>Important: because of the potential time involved in manipulating circles, the final result code of this call is always deferred.</strong>
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/circle/get_ext_ids_for_circle
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>circle_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the circle to list membership of.
                        </dd>
                        <dt>
                            <code>ext_type</code> (required)
                        </dt>
                        <dd>
                            External ID type. This is a string that identifies the external system with which the ext_ids values are associated. Must be a string containing only lowercase alphanumeric characters and underscores (<kbd>[0-9a-z_]</kbd>) and can be no more than 40 bytes in length.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/circle/get_ext_ids_for_circle?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171&amp;circle_id=42&amp;ext_type=dwid
</samp>
</pre>
            </div><!-- set_cons_ids_for_circle -->
            <div class="api_method">
                <h5>
                    <code>set_cons_ids_for_circle</code>
                </h5>
                <div class="description">
                    <p>
                        This method takes a constituent circle id and a list of constituent ids and replaces the current circle members with the list of constituent ids.
                    </p>
                    <p>
                        <strong>Important: because of the potential time involved in manipulating circles, the final result code of this call is always deferred.</strong>
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>, <code>POST</code>, or <code>PUT</code>
                </p>
                <p class="url">
                    URL: /page/api/circle/set_cons_ids_for_circle
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>circle_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the circle to replace membership for.
                        </dd>
                        <dt>
                            <code>cons_ids</code> (required)
                        </dt>
                        <dd>
                            When using GET or POST, a comma-separated list of constituent ids to set as the members of the circle. When using PUT, the cons_ids should be one per line as the only PUT data, and circle_id must be passed as a GET parameter.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/circle/set_cons_ids_for_circle?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
circle_id=42&amp;cons_ids=13,47,521,1033,1045
</samp>
</pre>
            </div><!-- set_ext_ids_for_circle -->
            <div class="api_method">
                <h5>
                    <code>set_ext_ids_for_circle</code>
                </h5>
                <div class="description">
                    <p>
                        External IDs (<code>ext_id</code>) are primary key identifiers associated with an external system or service. These can be stored alongside their corresponding Blue State Digital constituent records for easy access and reference. The external ID type (<code>ext_type</code>) specifies which external service/system a given ID is associated with. This method allows setting the membership of a constituent by specifying external ids instead of having to look up Blue State constituent ids first. The external ids must already exist in Blue State's system. Any external ids that are not found will be ignored and those constituents will not be added to the circle.
                    </p>
                    <p>
                        <strong>Important: because of the potential time involved in manipulating circles, the final result code of this call is always deferred.</strong>
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>, <code>POST</code>, or <code>PUT</code>
                </p>
                <p class="url">
                    URL: /page/api/circle/set_ext_ids_for_circle
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>circle_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the circle to replace membership for.
                        </dd>
                        <dt>
                            <code>ext_type</code> (required)
                        </dt>
                        <dd>
                            External ID type. This is a string that identifies the external system with which the ext_ids values are associated. Must be a string containing only lowercase alphanumeric characters and underscores (<kbd>[0-9a-z_]</kbd>) and can be no more than 40 bytes in length.
                        </dd>
                        <dt>
                            <code>ext_ids</code> (required)
                        </dt>
                        <dd>
                            When using GET or POST, a comma-separated list of external ids to set as the members of the circle. When using PUT, the ext_ids should be one per line as the only PUT data, and circle_id and ext_type must be passed as GET parameters.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/circle/set_ext_ids_for_circle?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
circle_id=42&amp;ext_type=van_id&amp;ext_ids=12772,1129,1983321,2008001,414456
</samp>
</pre>
            </div><!-- add_cons_ids_to_circle -->
            <div class="api_method">
                <h5>
                    <code>add_cons_ids_to_circle</code>
                </h5>
                <div class="description">
                    <p>
                        This method takes a circle id and a comma-separated list of constituent ids and adds those constituents to the circle.
                    </p>
                    <p>
                        <strong>Important: because of the potential time involved in manipulating circles, the final result code of this call is always deferred.</strong>
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/circle/add_cons_ids_to_circle
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>circle_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the circle to add constituents to.
                        </dd>
                        <dt>
                            <code>cons_ids</code> (required)
                        </dt>
                        <dd>
                            A comma-separated list of constituent ids to add to the circle.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/circle/add_cons_ids_to_circle?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
circle_id=42&amp;cons_ids=13,47,521
</samp>
</pre>
            </div><!-- add_ext_ids_to_circle -->
            <div class="api_method">
                <h5>
                    <code>add_ext_ids_to_circle</code>
                </h5>
                <div class="description">
                    <p>
                        External IDs (<code>ext_id</code>) are primary key identifiers associated with an external system or service. These can be stored alongside their corresponding Blue State Digital constituent records for easy access and reference. The external ID type (<code>ext_type</code>) specifies which external service/system a given ID is associated with. This method allows adding constituents to a circle by specifying their external id instead of having to look up their Blue State constituent id first. The external ids must already exist in Blue State's system. Any external ids that are not found will be ignored and those constituents will not be added to the circle.
                    </p>
                    <p>
                        <strong>Important: because of the potential time involved in manipulating circles, the final result code of this call is always deferred.</strong>
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/circle/add_ext_ids_to_circle
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>circle_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the circle to add constituents to.
                        </dd>
                        <dt>
                            <code>ext_type</code> (required)
                        </dt>
                        <dd>
                            External ID type. This is a string that identifies the external system with which the ext_ids values are associated. Must be a string containing only lowercase alphanumeric characters and underscores (<kbd>[0-9a-z_]</kbd>) and can be no more than 40 bytes in length.
                        </dd>
                        <dt>
                            <code>ext_ids</code> (required)
                        </dt>
                        <dd>
                            A comma-separated list of external ids to add to the circle.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/circle/add_cons_ids_to_circle?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
circle_id=45&amp;ext_type=van_id&amp;ext_ids=12772,1129,1983321
</samp>
</pre>
            </div><!-- remove_cons_ids_from_circle -->
            <div class="api_method">
                <h5>
                    <code>remove_cons_ids_from_circle</code>
                </h5>
                <div class="description">
                    <p>
                        This method takes a circle id and a comma-separated list of constituent ids and removes those constituents from the circle.
                    </p>
                    <p>
                        <strong>Important: because of the potential time involved in manipulating circles, the final result code of this call is always deferred.</strong>
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/circle/remove_cons_ids_from_circle
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>circle_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the circle to remove constituents from.
                        </dd>
                        <dt>
                            <code>cons_ids</code> (required)
                        </dt>
                        <dd>
                            A comma-separated list of constituent ids to remove from the circle.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/circle/remove_cons_ids_from_circle?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
circle_id=42&amp;cons_ids=13,47,521
</samp>
</pre>
            </div><!-- remove_ext_ids_from_circle -->
            <div class="api_method">
                <h5>
                    <code>remove_ext_ids_from_circle</code>
                </h5>
                <div class="description">
                    <p>
                        External IDs (<code>ext_id</code>) are primary key identifiers associated with an external system or service. These can be stored alongside their corresponding Blue State Digital constituent records for easy access and reference. The external ID type (<code>ext_type</code>) specifies which external service/system a given ID is associated with. This method allows removing constituents from a circle by specifying their external id instead of having to look up their Blue State constituent id first. The external ids must already exist in Blue State's system. Any external ids that are not found will be ignored and those constituents will not be removed from the circle.
                    </p>
                    <p>
                        <strong>Important: because of the potential time involved in manipulating circles, the final result code of this call is always deferred.</strong>
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/circle/remove_ext_ids_from_circle
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>circle_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the circle to remove constituents from.
                        </dd>
                        <dt>
                            <code>ext_type</code> (required)
                        </dt>
                        <dd>
                            External ID type. This is a string that identifies the external system with which the ext_ids values are associated. Must be a string containing only lowercase alphanumeric characters and underscores (<kbd>[0-9a-z_]</kbd>) and can be no more than 40 bytes in length.
                        </dd>
                        <dt>
                            <code>ext_ids</code> (required)
                        </dt>
                        <dd>
                            A comma-separated list of external ids to remove from the circle.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/circle/remove_ext_ids_from_circle?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
circle_id=42&amp;ext_type=van_id&amp;ext_ids=12772,1129,1983321
</samp>
</pre>
            </div><!-- move_cons_ids_to_circle -->
            <div class="api_method">
                <h5>
                    <code>move_cons_ids_to_circle</code>
                </h5>
                <div class="description">
                    <p>
                        This method allows moving constituents from the source circle to the target circle by specifying their constituent ids. Any constituent ids that are not found will be ignored and those constituents will not be moved from the source circle to the target circle.
                    </p>
                    <p>
                        <strong>Important: because of the potential time involved in manipulating circles, the final result code of this call is always deferred.</strong>
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/circle/move_cons_ids_to_circle
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>from_circle_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the circle to remove constituents from.
                        </dd>
                        <dt>
                            <code>to_circle_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the circle to add constituents to.
                        </dd>
                        <dt>
                            <code>cons_ids</code> (required)
                        </dt>
                        <dd>
                            A comma-separated list of constituent ids to move from the source circle to the target circle.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/circle/move_cons_ids_to_circle?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
from_circle_id=42&amp;to_circle_id=45&amp;cons_ids=12772,1129,1983321
</samp>
</pre>
            </div><!-- move_ext_ids_to_circle -->
            <div class="api_method">
                <h5>
                    <code>move_ext_ids_to_circle</code>
                </h5>
                <div class="description">
                    <p>
                        External IDs (<code>ext_id</code>) are primary key identifiers associated with an external system or service. These can be stored alongside their corresponding Blue State Digital constituent records for easy access and reference. The external ID type (<code>ext_type</code>) specifies which external service/system a given ID is associated with. This method allows moving constituents from the source circle to the target circle by specifying their external id instead of having to look up their Blue State constituent id first. The external ids must already exist in Blue State's system. Any external ids that are not found will be ignored and those constituents will not be moved from the source circle to the target circle.
                    </p>
                    <p>
                        <strong>Important: because of the potential time involved in manipulating circles, the final result code of this call is always deferred.</strong>
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/circle/move_ext_ids_to_circle
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>from_circle_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the circle to remove constituents from.
                        </dd>
                        <dt>
                            <code>to_circle_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the circle to add constituents to.
                        </dd>
                        <dt>
                            <code>ext_type</code> (required)
                        </dt>
                        <dd>
                            External ID type. This is a string that identifies the external system with which the ext_ids values are associated. Must be a string containing only lowercase alphanumeric characters and underscores (<kbd>[0-9a-z_]</kbd>) and can be no more than 40 bytes in length.
                        </dd>
                        <dt>
                            <code>ext_ids</code> (required)
                        </dt>
                        <dd>
                            A comma-separated list of external ids to move from the source circle to the target circle.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/circle/move_ext_ids_to_circle?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
from_circle_id=42&amp;to_circle_id=45&amp;ext_type=van_id&amp;ext_ids=12772,1129,1983321
</samp>
</pre>
            </div><!-- set_circle_administrator -->
            <div class="api_method">
                <h5>
                    <code>set_circle_administrator</code>
                </h5>
                <div class="description">
                    <p>
                        Takes a cons_id and promotes them to administrator of the given circle_id. If the cons_id is not yet a member of the circle, this method adds them to the circle before promoting them.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/circle/set_circle_administrator
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>circle_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the circle to set an administrator for.
                        </dd>
                        <dt>
                            <code>cons_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the constituent to promote to administrator.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/circle/set_circle_administrator?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
circle_id=42&amp;cons_id=45
</samp>
</pre>
            </div><!-- demote_circle_administrator -->
            <div class="api_method">
                <h5>
                    <code>demote_circle_administrator</code>
                </h5>
                <div class="description">
                    <p>
                        Takes a <code>cons_id</code> and demotes them from administrator of the given <code>circle_id</code>. If the <code>cons_id</code> is not yet a member of the circle, this method throws an <code>api_exception</code>. If the <code>circle_id</code> or the <code>cons_id</code> don't refer to actual cons or circles, then an <code>api_exception</code> is thrown as well.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/circle/demote_circle_administrator
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>circle_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the circle to demote an administrator from.
                        </dd>
                        <dt>
                            <code>cons_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the constituent to demote from administrator.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/circle/demote_circle_administrator?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
circle_id=42&amp;cons_id=45
</samp>
</pre>
            </div><!-- set_circle_owner -->
            <div class="api_method">
                <h5>
                    <code>set_circle_owner</code>
                </h5>
                <div class="description">
                    <p>
                        Takes a <code>cons_id</code> and sets them as the owner of the given <code>circle_id</code>. If the <code>cons_id</code> is not yet a member of the circle, this method adds them. If they are already a member, it promotes them to an admin. If the <code>circle_id</code> or the <code>cons_id</code> don't refer to actual cons or circles, then an <code>api_exception</code> is thrown. Note that there is only one owner per circle. Setting a new owner will overwrite the old owner.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/circle/set_circle_owner
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>circle_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the circle to set a new owner for.
                        </dd>
                        <dt>
                            <code>cons_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the constituent to set as owner.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/circle/set_circle_owner?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
circle_id=42&amp;cons_id=45
</samp>
</pre>
            </div>
            <h4>
                Constituent (cons) API Calls
            </h4><!-- get_constituents_by_id -->
            <div class="api_method">
                <h5>
                    <code>get_constituents_by_id</code>
                </h5>
                <div class="description">
                    <p>
                        Takes one or more constituent IDs as a parameter and returns the matching constituent records.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/get_constituents_by_id
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>cons_ids</code> (required)
                        </dt>
                        <dd>
                            One or more (comma separated) constituent IDs.
                        </dd>
                        <dt>
                            <code>bundles</code> (optional)
                        </dt>
                        <dd>
                            The data bundles to include in the records. See the Constituent Records-&gt;Bundle Specification section for details.
                        </dd>
                        <dt>
                            <code>filter</code> (optional)
                        </dt>
                        <dd>
                            The filter to apply to the resulting records. See the Constituent Records-&gt;Filter Specification section for details.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns zero or more <code>&lt;cons&gt;</code> elements (see Common Record Formats), containing all of the constituent records that match the specified constituent IDs.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/get_constituents_by_id?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=3c3452091bf7ff0e2be8b638694a187ca7c0a013&amp;cons_ids=57,3882,31,132&amp;bundles=cons_addr,cons_phone
</samp>
</pre>
            </div>
            <div class="api_method">
                <h5>
                    <code>get_constituents</code>
                </h5>
                <div class="description">
                    <p>
                        Takes one or more filters as a parameter and returns the matching constituent records.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/get_constituents
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                </div>
                <dl>
                    <dt>
                        <code>filter</code> (required)
                    </dt>
                    <dd>
                        At least one filter must be specified. See the Constituent Records-&gt;Filter Specification section for details.
                    </dd>
                    <dt>
                        <code>bundles</code> (optional)
                    </dt>
                    <dd>
                        The data bundles to include in the records. See the Constituent Records-&gt;Bundle Specification section for details.
                    </dd>
                </dl>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns zero or more <code>&lt;cons&gt;</code> elements (see Common Record Formats), containing all of the constituent records that match the specified constituent IDs.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <div>
                    <samp>http://XYZ/page/api/cons/get_constituents?filter=state_cd%3D%28MA%2CNY%29&amp;api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274737903&amp;api_mac=5edfe8bdb8483655339f05f73c3fcb18cc65aa5f</samp>
                </div>
            </div><!-- get_constituents_by_ext_id -->
            <div class="api_method">
                <h5>
                    <code>get_constituents_by_ext_id</code>
                </h5>
                <div class="description">
                    <p>
                        Takes one or more external IDs (as previously set with the <code>set_ext_ids</code> API call) as a parameter and returns the matching constituent records.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/get_constituents_by_ext_id
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>ext_type</code> (required)
                        </dt>
                        <dd>
                            The type of the external ID (as passed to <code>set_ext_ids</code>).
                        </dd>
                        <dt>
                            <code>ext_ids</code> (required)
                        </dt>
                        <dd>
                            One or more (comma separated) external IDs.
                        </dd>
                        <dt>
                            <code>bundles</code> (optional)
                        </dt>
                        <dd>
                            The data bundles to include in the records. See the Constituent Records-&gt;Bundle Specification section for details.
                        </dd>
                        <dt>
                            <code>filter</code> (optional)
                        </dt>
                        <dd>
                            The filter to apply to the resulting records. See the Constituent Records-&gt;Filter Specification section for details.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns zero or more <code>&lt;cons&gt;</code> elements (see Common Record Formats), containing all of the constituent records that match the specified external IDs.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/get_constituents_by_ext_id?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=61722c6a82a16779c956992e351ae1582f4a3cc1&amp;ext_type=van_id&amp;ext_ids=f12772,l1129,1m983321&amp;bundles=cons_addr,cons_phone
</samp>
</pre>
            </div><!-- get_constituents_by_guid -->
            <div class="api_method">
                <h5>
                    <code>get_constituents_by_guid</code>
                </h5>
                <div class="description">
                    <p>
                        Takes one or more GUIDs (non-sequential, random, unique identifiers for constituents) as a parameter and returns the matching constituent records.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/get_constituents_by_guid
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>guids</code> (required)
                        </dt>
                        <dd>
                            One or more (comma separated) GUIDs.
                        </dd>
                        <dt>
                            <code>bundles</code> (optional)
                        </dt>
                        <dd>
                            The data bundles to include in the records. See the Constituent Records-&gt;Bundle Specification section for details.
                        </dd>
                        <dt>
                            <code>filter</code> (optional)
                        </dt>
                        <dd>
                            The filter to apply to the resulting records. See the Constituent Records-&gt;Filter Specification section for details.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns zero or more <code>&lt;cons&gt;</code> elements (see Common Record Formats), containing all of the constituent records that match the specified external IDs.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/get_constituents_by_guid?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=61722c6a82a16779c956992e351ae1582f4a3cc1&amp;guids=ygdFPkyEdomzBhWEFZGREys&amp;bundles=cons_addr,cons_email
</samp>
</pre>
            </div><!-- get_updated_constituents -->
            <div class="api_method">
                <h5>
                    <code>get_updated_constituents</code>
                </h5>
                <div class="description">
                    <p>
                        Returns all constituent records that have been updated since a particular date and time. If a <code>bundles</code> parameter is specified, this API call will return look at the modified date on each bundle as well as the modified date on the main constituent record to determine which constituents to return. If, for example, the <code>cons_addr</code> bundle is included, records will be returned that have had a <code>cons_addr</code> record modified since the <code>changed_since</code> time, even if the core constituent record has not been updated.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/get_updated_constituents
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>changed_since</code> (required)
                        </dt>
                        <dd>
                            All constituent records that have changed since this time (specified in epoch seconds) will be returned by the API call.
                        </dd>
                        <dt>
                            <code>bundles</code> (optional)
                        </dt>
                        <dd>
                            The data bundles to include in the records. See the Constituent Records-&gt;Bundle Specification section for details.
                        </dd>
                        <dt>
                            <code>filter</code> (optional)
                        </dt>
                        <dd>
                            The filter to apply to the resulting records. See the Constituent Records-&gt;Filter Specification section for details.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns zero or more <code>&lt;cons&gt;</code> elements (see Common Record Formats), containing all of the constituent records that have been updated since the <code>changed_since</code> time.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/get_updated_constituents?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=c1fd787ec5431781f1d601f6e3299fe083ca56c4&amp;changed_since=1179856723&amp;bundles=cons_email&amp;filter=state_cd%3DIA
</samp>
</pre>
            </div><!-- get_updated_constituent_ids -->
            <div class="api_method">
                <h5>
                    <code>get_updated_constituent_ids</code>
                </h5>
                <div class="description">
                    <p>
                        Returns all constituent ids that have been updated since a particular date and time as a JSON array.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/get_updated_constituent_ids
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>changed_since</code> (required)
                        </dt>
                        <dd>
                            All constituent records that have changed since this time (specified in epoch seconds) will be returned by the API call.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns a JSON array with the cons_id of all matching constituents.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/get_updated_constituent_ids?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=c1fd787ec5431781f1d601f6e3299fe083ca56c4&amp;changed_since=1179856723
</samp>
</pre>
            </div><!-- set_ext_ids -->
            <div class="api_method">
                <h5>
                    <code>set_ext_ids</code>
                </h5>
                <div class="description">
                    <p>
                        External IDs (<code>ext_id</code>) are primary key identifiers associated with an external system or service. These can be stored alongside their corresponding Blue State Digital constituent records for easy access and reference. The external ID type (<code>ext_type</code>) specifies which external service/system a given ID is associated with. A given constituent record can only have one <code>ext_id</code> for a given <code>ext_type</code>. A given <code>ext_type</code>/<code>ext_id</code> pair can only apply to one specific constituent record. ext_ids can be numeric or alpha-numeric strings no longer than 255 characters.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/set_ext_ids
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>ext_type</code> (required)
                        </dt>
                        <dd>
                            External ID type. This is a string that identifies the external system with which the ext_ids values are associated. Must be a string containing only lowercase alphanumeric characters and underscores ([0-9a-z_]) and can be no more than 40 bytes in length.
                        </dd>
                        <dt>
                            <code>cons_id=ext_id</code> (required)
                        </dt>
                        <dd>
                            Each <code>cons_id=ext_id</code> parameter sets a mapping for one BSD cons_id to one external ID. For example, 4322=12772 would associate the BSD cons_id 4322 with the external ID 12772. If the ext_id is 0, any existing external ID mapping with the BSD cons_id will be removed.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/set_ext_ids?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
ext_type=van_id&amp;4322=12772&amp;3321=1129&amp;9217=1983321
</samp>
</pre>
            </div><!-- delete_constituents_by_id -->
            <div class="api_method">
                <h5>
                    <code>delete_constituents_by_id</code>
                </h5>
                <div class="description">
                    <p>
                        Takes one or more constituent IDs as a parameter and deletes the matching constituent records.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/delete_constituents_by_id
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>cons_ids</code> (required)
                        </dt>
                        <dd>
                            One or more (comma separated) constituent IDs.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/delete_constituents_by_id?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=3c3452091bf7ff0e2be8b638694a187ca7c0a013&amp;cons_ids=57,3882,31,132
</samp>
</pre>
            </div><!-- get_custom_constituent_fields -->
            <div class="api_method">
                <h5>
                    <code>get_custom_constituent_fields</code>
                </h5>
                <div class="description">
                    <p>
                        Returns all existing custom constituent fields.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/get_custom_constituent_fields
                </p>
                <div class="parameters">
                    <p>
                        Parameters: None
                    </p>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns zero or more <code>&lt;cons_field&gt;</code> elements (see <a href="#Data-Bundle--cons_field">Common Record Formats</a>).
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/get_custom_constituent_fields?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=c1fd787ec5431781f1d601f6e3299fe083ca56c4&amp;
</samp>
</pre>
            </div><!-- upload_bulk_constituent_data -->
            <div class="api_method">
                <h5>
                    <code>upload_bulk_constituent_data</code>
                </h5>
                <div class="description">
                    <p>
                        Allows constituents to be added in the same format as Upload Constituents on the Constituents control panel page. The file contents and column mappings must be part of the call.
                    </p>
                    <p>
                        The constituent information is merged into the the existing data; existing cons will have their records updated, multiple uploads of the same data will be combined.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>POST</code> or <code>PUT</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/upload_bulk_constituent_data
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>CSV data</code> (required)
                        </dt>
                        <dd>
                            <p>
                                The constituent data to add must be passed in either the <code>csv_data</code> PUT parameter or the POST request body. If passed in the POST body the entire request body is interpreted as the uploaded file. The format is the same as used for Upload Constituents via the control panel (CSV: rows of comma-separated values terminated with a newline).
                            </p>
                        </dd>
                        <dt>
                            <code>column_map</code> (required)
                        </dt>
                        <dd>
                            <p>
                                A comma-separated list of table column names in which to store the CSV fields. If a field in the data is not stored, leave the column name empty. The column names are the same as shown on the Upload Constituents page.
                            </p>
                            <p>
                                <em>NOTE:</em> It is important that every CSV data field be present in the column map. Fields not stored are represented by comma-separated blanks. For example, to extract the first name, last name, email address but skip affiliation and home state from the line <code>John,Smith,Democrat,johnsmith@johnsmith.com,California</code> use the mapping <code>firstname,lastname,,email,</code>. Note the un-mapped columns signified by the ,, and the trailing ,
                            </p>
                        </dd>
                        <dt>
                            <code>has_header</code> (optional)
                        </dt>
                        <dd>
                            If set to 1, the first row of the CSV data will be assumed to be a header row and will be ignored. If 0 or if not present all rows will be read as data.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure. The upload is done in two phases: inserting the data and creating constituents. The returned status is for the data insertion phase; constituent creation is done as for signups and will take longer. The upload progress can be monitored on the Upload Constituents status page, <a href="/modules/constituent/admin/upload_status.php">/modules/constituent/admin/upload_status.php</a>
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    <em>Note:</em> The deferred results become available once the data has been inserted, but possibly before constituent creation has completed.
                </p>
                <p>
                    Example:
                </p>
                <p>
                    Adds the constituent name and email address to the database from a CSV file containing other fields.
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>http://XYZ/page/api/cons/bulk_upload_constituent_data?has_header=1&amp;column_map=firstname,lastname,,email,&amp;api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=3c3452091bf7ff0e2be8b638694a187ca7c0a013</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
FirstName,LastName,Affiliation,EmailAddress,Residence
John,Smith,Democrat,johnsmith@johnsmith.com,California
Johnny,Smithson,Independent,johnny_s@johnsmith.com,California
</samp>
</pre>
            </div><!-- get_bulk_constituent_data -->
            <div class="api_method">
                <h5>
                    <code>get_bulk_constituent_data</code>
                </h5>
                <div class="description">
                    <p>
                        Provides an easy way to get bulk data about all or some of the constituents in the Blue State Digital database. Not every piece of information is available via this call and data is returned in a flat (CSV), rather than structured format. This is intended as a means of downloading large amounts of data for easy caching in a local database.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>POST</code> or <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/get_bulk_constituent_data
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>fields</code> (required)
                        </dt>
                        <dd>
                            <p>
                                The fields to include in the data set. cons_id is always included and does not need to be specified. This should be one or more of following field IDs, separated by commas:
                            </p>
                        </dd>
                        <dt>
                            <code>firstname</code>
                        </dt>
                        <dd>
                            The first name of the constituent.
                        </dd>
                        <dt>
                            <code>lastname</code>
                        </dt>
                        <dd>
                            The last name of the constituent.
                        </dd>
                        <dt>
                            <code>primary_addr1</code>
                        </dt>
                        <dd>
                            The addr1 field of the constituent's primary address.
                        </dd>
                        <dt>
                            <code>primary_addr2</code>
                        </dt>
                        <dd>
                            The addr2 field of the constituent's primary address.
                        </dd>
                        <dt>
                            <code>primary_city</code>
                        </dt>
                        <dd>
                            The city field of the constituent's primary address.
                        </dd>
                        <dt>
                            <code>primary_state_cd</code>
                        </dt>
                        <dd>
                            The state field of the constituent's primary address.
                        </dd>
                        <dt>
                            <code>primary_zip</code>
                        </dt>
                        <dd>
                            The zip field of the constituent's primary address.
                        </dd>
                        <dt>
                            <code>primary_zip_4</code>
                        </dt>
                        <dd>
                            The zip+4 field of the constituent's primary address.
                        </dd>
                        <dt>
                            <code>primary_country</code>
                        </dt>
                        <dd>
                            The country field of the constituent's primary address.
                        </dd>
                        <dt>
                            <code>primary_phone</code>
                        </dt>
                        <dd>
                            The phone field of the constituent's primary address.
                        </dd>
                        <dt>
                            <code>primary_email</code>
                        </dt>
                        <dd>
                            The constituent's primary email address.
                        </dd>
                        <dt>
                            <code>is_subscribed</code>
                        </dt>
                        <dd>
                            1 if the constituent is subscribed to receive email. 0 if the constituent is NOT subscribed to receive email.
                        </dd>
                        <dt>
                            <code>has_account</code>
                        </dt>
                        <dd>
                            returns 1 or 0 depending on whether the user has a log in account.
                        </dd>
                        <dt>
                            <code>ext_X</code>
                        </dt>
                        <dd>
                            Where X is an external ID type (as set using the <code>set_ext_ids</code> API call). For example:
                            <pre>
<samp>
ext_van_id
</samp>
</pre>
                            <p>
                                If a field ID has an asterisk appended to it, the field will be treated as required. Any constituent record that does not have a value for that field will not be included in the resulting data set. For example, a <code>fields</code> value of
                            </p>
                            <pre>
<samp>
firstname, lastname, primary_zip*, ext_van_id
</samp>
</pre>
                            <p>
                                would only export recipients who had a valid primary_zip.
                            </p>
                        </dd>
                        <dt>
                            <code>cons_ids</code> (optional)
                        </dt>
                        <dd>
                            One or more (comma separated) constituent IDs. If not specified, data for all constituents will be returned (this may result in a VERY large data set).
                        </dd>
                        <dt>
                            <code>filter</code> (optional)
                        </dt>
                        <dd>
                            The filter to apply to the resulting records. See the Constituent Records-&gt;Filter Specification section for details.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns data for zero or more constituent records in the format specified. If the format is CVS, the first row of the CSV will contain headers indicating which values are in which column. If the format is XML, the result will contain a series of <code>&lt;cons_bulk&gt;</code> elements, each containing one element for each requested field. The XML format returned by this function does NOT match the structured common <code>&lt;cons&gt;</code> element format returned by other constituent API calls.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    Example:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/get_bulk_constituent_data?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=88720bf92a70348cadf30c0cb375708fc9ab3911
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
format=csv&amp;fields=firstname,lastname,primary_email,ext_van_id
</samp>
</pre>
            </div><!-- set_constituent_data -->
            <div class="api_method">
                <h5>
                    <code>set_constituent_data</code>
                </h5>
                <div class="description">
                    <p>
                        This method allows creating or updating constituent records in BSD's system for one or more constituents. All fields described in the "Incoming Data" specifications can be set using this method, and it can be called for one or more constituents, as defined by top-level cons elements in the incoming XML. The XML data must be encoded in UTF-8.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/set_constituent_data
                </p>
                <div class="parameters">
                    <p>
                        Parameters: POST data
                    </p>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure. On success, an XML document is returned describing the results of each insert. For example:
                    </p>
                    <pre>
&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;api&gt;
    &lt;cons is_new="0" id="22"/&gt;
    &lt;cons is_new="1" id="1090"/&gt;
&lt;/api&gt;

</pre>
                    <p>
                        The "is_new" attribute tells if this constituent was created via this API call or 0 if it was simply modified and the "id" attribute is the the constituent's id. The records are returned in the order in which they were specified in the parameters.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/set_constituent_data?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;api&gt;
    &lt;cons id="4382" <em>send_password="y"</em>&gt;
        &lt;firstname&gt;Bob&lt;/firstname&gt;
        &lt;lastname&gt;Smith&lt;/lastname&gt;
        &lt;is_banned&gt;0&lt;/is_banned&gt;
        &lt;create_dt&gt;1168146000&lt;/create_dt&gt;

        &lt;cons_email&gt;
            &lt;email&gt;bsmith@somecompany.com&lt;/email&gt;
            &lt;email_type&gt;work&lt;/email_type&gt;
            &lt;is_subscribed&gt;0&lt;/is_subscribed&gt;
            &lt;is_primary&gt;0&lt;/is_primary&gt;
        &lt;/cons_email&gt;
    &lt;/cons&gt;
&lt;/api&gt;
</samp>
</pre>
                <p>
                    If a new cons is being created and <code>send_password</code> is set to <kbd>y</kbd> it will cause the system to generate a random password and email it to the new cons.
                </p>
            </div><!-- set_custom_constituent_fields -->
            <div class="api_method">
                <h5>
                    <code>set_custom_constituent_fields</code>
                </h5>
                <div class="description">
                    <p>
                        This method allows creating or updating a constituent record's custom fields.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/set_custom_constituent_fields
                </p>
                <div class="parameters">
                    <dl>
                        <dd>
                            <code>cons_id</code> (required)
                        </dd>
                        <dt>
                            The constituent ID we are modifying.
                        </dt>
                        <dd>
                            <code>delete_missing</code> (optional)
                        </dd>
                        <dt>
                            1 or 0. If 1, delete any custom constituent fields associated with the cons and not referenced in this call. Defaults to 1 if not specified
                        </dt>
                    </dl>
                    <p>
                        Parameters: POST data
                    </p>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure. On success, the created or updated cons_field_value_ids are returned. For example:
                    </p>
                    <pre>
&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;api&gt;
    &lt;cons_field_value is_new="0" id="1090"/&gt;
    &lt;cons_field_value is_new="1" id="1091"/&gt;
&lt;/api&gt;

</pre>
                    <p>
                        The "is_new" attribute tells if this constituent field was created via this API call or 0 if it was simply modified and the "id" attribute is the the constituent field value id.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/set_custom_constituent_fields?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171&amp;cons_id=1
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;api&gt;
    &lt;cons_field id="23"&gt;
        &lt;value&gt;0&lt;/value&gt;
    &lt;/cons_field&gt;
&lt;/api&gt;
</samp>
</pre>
            </div><!-- merge_constituents_by_email -->
            <div class="api_method">
                <h5>
                    <code>merge_constituents_by_email</code>
                </h5>
                <div class="description">
                    <p>
                        Given an email address, this method will find all constituents who have that email and merge them into one constituent. All matching constituents are archived and a new constituent is created with all of the data of the matched constituents. The data is deduped and a primary field for records that require them (email, address, etc), is chosen amongst the merged results by always preferring the newest record.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/merge_constituents_by_email
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>email</code>
                        </dt>
                        <dd>
                            Email address of constituents to be merged.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns the constituent ID (<code>cons_id</code>) of the new, merged constituent in the following format:
                    </p>
                    <pre>
    &lt;?xml version="1.0" encoding="UTF-8"?&gt;
    &lt;api&gt;
        &lt;cons id="22"/&gt;
    &lt;/api&gt;

</pre>
                </div>
                <p class="deferred">
                    Deferred Results: Always
                </p>
            </div><!-- merge_constituents_by_id -->
            <div class="api_method">
                <h5>
                    <code>merge_constituents_by_id</code>
                </h5>
                <div class="description">
                    <p>
                        Operates the same way that <code>merge_constituents_by_email</code> works, except instead of specifying an email address to match constituents, this API call takes a comma separated list of constituent IDs as POST data. All constituents specified will be merged into one constituent.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/merge_constituents_by_id
                </p>
                <div class="parameters">
                    <p>
                        Parameters: POST data consisting of a comma separated list of constituent ids.
                    </p>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns the constituent ID (<code>cons_id</code>) of the new, merged constituent in the following format:
                    </p>
                    <pre>
    &lt;?xml version="1.0" encoding="UTF-8"?&gt;
    &lt;api&gt;
        &lt;cons id="22"/&gt;
    &lt;/api&gt;

</pre>
                </div>
                <p class="deferred">
                    Deferred Results: Always
                </p>
            </div><!-- get_unsubscribed_constituents_by_group_id -->
            <div class="api_method">
                <h5>
                    <code>get_unsubscribed_constituents_by_group_id</code>
                </h5>
                <div class="description">
                    <p>
                        This method allows getting a list of constituents that have been unsubscribed from a group by it's id.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/get_unsubscribed_constituents_by_group_id
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>group_id</code> (required)
                        </dt>
                        <dd>
                            The group_id of the group to look up unsubscribed constituents for
                        </dd>
                        <dt>
                            <code>format</code> (optional)
                        </dt>
                        <dd>
                            The format of the response, either json or xml. Defaults to xml.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure. On success, a document is returned containing the cons_ids of the unsubscribed constituents.
                    </p><samp>&lt;?xml version="1.0" encoding="utf-8"?&gt; &lt;api&gt; &lt;cons&gt; &lt;cons_id&gt;34&lt;/cons_id&gt; &lt;/cons&gt; &lt;cons&gt; &lt;cons_id&gt;121&lt;/cons_id&gt; &lt;/cons&gt; &lt;cons&gt; &lt;cons_id&gt;3435&lt;/cons_id&gt; &lt;/cons&gt; &lt;/api&gt;</samp>
                    <p>
                        or
                    </p><samp>[ {"cons_id":34}, {"cons_id":121}, {"cons_id":3435} ]</samp>
                    <p>
                        Format depends on the format attribute passed the to the API
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/get_unsubscribed_constituents_by_group_id?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171&amp;group_id=5&amp;format=json
</samp>
</pre>
            </div><!-- email_register -->
            <div class="api_method">
                <h5>
                    <code>email_register</code>
                </h5>
                <div class="description">
                    <p>
                        This method allows creating constituent emails in BSD's system for a single constituent. If the email or constituent does not exist, they will be created, otherwise, they will be updated.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/email_register
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>cons_id</code> (optional)
                        </dt>
                        <dd>
                            The numeric id of the constituent to add the email to. If not supplied, a new constituent will be created.
                        </dd>
                        <dt>
                            <code>email</code> (required)
                        </dt>
                        <dd>
                            The email address to register or update.
                        </dd>
                        <dt>
                            <code>is_subscribed</code> (optional)
                        </dt>
                        <dd>
                            Set the email to subscribed or not (1 or 0)
                        </dd>
                        <dt>
                            <code>guid</code> (optional)
                        </dt>
                        <dd>
                            The guid to look up the constituent by.
                        </dd>
                        <dt>
                            <code>format</code> (optional)
                        </dt>
                        <dd>
                            The format of the response, either json or xml. Defaults to xml.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure. On success, a document is returned containing the cons_id and email_id for the email.
                    </p><samp>&lt;?xml version="1.0" encoding="utf-8"?&gt; &lt;api&gt; &lt;cons_email&gt; &lt;cons_id&gt;34&lt;/cons_id&gt; &lt;email_id&gt;234&lt;/email_id&gt; &lt;/cons_email&gt; &lt;/api&gt;</samp>
                    <p>
                        or
                    </p><samp>{ "cons_id" : 34, "email_id" : 234 }</samp>
                    <p>
                        Format depends on the format attribute passed the to the API
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/email_register?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171&amp;cons_id=34&amp;email=test@test.com&amp;is_subscribed=0&amp;format=json
</samp>
</pre>
            </div><!-- email_unsubscribe -->
            <div class="api_method">
                <h5>
                    <code>email_unsubscribe</code>
                </h5>
                <div class="description">
                    <p>
                        This method allows unsubscribing constituent emails in BSD's system.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/email_unsubscribe
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>email</code> (required)
                        </dt>
                        <dd>
                            The email address to unsubscribe.
                        </dd>
                        <dt>
                            <code>reason</code> (optional)
                        </dt>
                        <dd>
                            The reason for unsubscribing the email.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/email_unsubscribe?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171&amp;email=test@test.com&amp;reason=User%20Requested
</samp>
</pre>
            </div><!-- email_lookup -->
            <div class="api_method">
                <h5>
                    <code>email_lookup</code>
                </h5>
                <div class="description">
                    <p>
                        This method allows looking up constituent emails in BSD's system.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/email_lookup
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>cons_id</code> (required*)
                        </dt>
                        <dd>
                            The numeric id of the constituent.
                        </dd>
                        <dt>
                            <code>email</code> (optional)
                        </dt>
                        <dd>
                            The email address to lookup.
                        </dd>
                        <dt>
                            <code>is_subscribed</code> (optional)
                        </dt>
                        <dd>
                            Filter the email by subscribed or not (1 or 0)
                        </dd>
                        <dt>
                            <code>guid</code> (required*)
                        </dt>
                        <dd>
                            The guid to look up the constituent by.
                        </dd>
                        <dt>
                            <code>format</code> (optional)
                        </dt>
                        <dd>
                            The format of the response, either json or xml. Defaults to xml.
                        </dd>
                    </dl>
                    <p>
                        * Only one of cons_id or guid is required.
                    </p>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure. On success, a document is returned containing the email records found.
                    </p><samp>&lt;?xml version="1.0" encoding="utf-8"?&gt; &lt;api&gt; &lt;cons_email&gt; &lt;cons_id&gt;34&lt;/cons_id&gt; &lt;email_id&gt;234&lt;/email_id&gt; &lt;email&gt;test@test.com&lt;/email&gt; &lt;is_subscribed&gt;1&lt;/is_subscribed&gt; &lt;/cons_email&gt; &lt;cons_email&gt; &lt;cons_id&gt;31&lt;/cons_id&gt; &lt;email_id&gt;24&lt;/email_id&gt; &lt;email&gt;test2@gmail.com&lt;/email&gt; &lt;is_subscribed&gt;1&lt;/is_subscribed&gt; &lt;/cons_email&gt; &lt;/api&gt;</samp>
                    <p>
                        or
                    </p><samp>[ { "cons_id" : 34, "email_id" : 234, "email_id" : "test@test.com", "is_subscribed" : 1 }, { "cons_id" : 31, "email_id" : 24 "email_id" : "test2@gmail.com", "is_subscribed" : 1 } ]</samp>
                    <p>
                        Format depends on the format attribute passed the to the API
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/email_lookup?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171&amp;cons_id=34&amp;format=json
</samp>
</pre>
            </div><!-- email_delete -->
            <div class="api_method">
                <h5>
                    <code>email_delete</code>
                </h5>
                <div class="description">
                    <p>
                        This method allows deleting constituent emails in BSD's system.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/email_delete
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>cons_id</code> (required*)
                        </dt>
                        <dd>
                            The numeric id of the constituent.
                        </dd>
                        <dt>
                            <code>email</code> (required)
                        </dt>
                        <dd>
                            The email address to delete.
                        </dd>
                        <dt>
                            <code>is_subscribed</code> (optional)
                        </dt>
                        <dd>
                            Filter the email by subscribed or not (1 or 0)
                        </dd>
                        <dt>
                            <code>guid</code> (required*)
                        </dt>
                        <dd>
                            The guid to look up the constituent by.
                        </dd>
                    </dl>
                    <p>
                        * Only one of cons_id or guid is required.
                    </p>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure. On success, a document is returned containing the email records found.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/email_delete?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171&amp;cons_id=34&amp;email=test@test.com
</samp>
</pre>
            </div><!-- address_register -->
            <div class="api_method">
                <h5>
                    <code>address_register</code>
                </h5>
                <div class="description">
                    <p>
                        This method allows registering constituent addresses in the BSD system
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/address_register
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>cons_id</code> (required*)
                        </dt>
                        <dd>
                            The numeric id of the constituent to add the address to.
                        </dd>
                        <dt>
                            <code>addr1</code> (optional)
                        </dt>
                        <dd>
                            Line 1 of the address
                        </dd>
                        <dt>
                            <code>addr2</code> (optional)
                        </dt>
                        <dd>
                            Line 2 of the address
                        </dd>
                        <dt>
                            <code>city</code> (optional)
                        </dt>
                        <dd>
                            City of the address
                        </dd>
                        <dt>
                            <code>state</code> (optional)
                        </dt>
                        <dd>
                            State of the address in 2 character uppercase. Ex MA
                        </dd>
                        <dt>
                            <code>postcode</code> (optional)
                        </dt>
                        <dd>
                            Postal code of the address. For the US, that is 9 digits, ex. 02021-2345
                        </dd>
                        <dt>
                            <code>guid</code> (required*)
                        </dt>
                        <dd>
                            The guid to look up the constituent by.
                        </dd>
                    </dl>
                    <p>
                        * Only one of cons_id or guid is required.
                    </p>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/address_register?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171&amp;cons_id=65&amp;line1=280+Summer+Street&amp;city=Boston&amp;state=MA&amp;zip=02145-0934
</samp>
</pre>
            </div><!-- address_lookup -->
            <div class="api_method">
                <h5>
                    <code>address_lookup</code>
                </h5>
                <div class="description">
                    <p>
                        This method allows looking up constituent addresses in BSD's system.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/address_lookup
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>cons_id</code> (required*)
                        </dt>
                        <dd>
                            The numeric id of the constituent.
                        </dd>
                        <dt>
                            <code>addr1</code> (optional)
                        </dt>
                        <dd>
                            Line 1 of the address
                        </dd>
                        <dt>
                            <code>addr2</code> (optional)
                        </dt>
                        <dd>
                            Line 2 of the address
                        </dd>
                        <dt>
                            <code>city</code> (optional)
                        </dt>
                        <dd>
                            City of the address
                        </dd>
                        <dt>
                            <code>state</code> (optional)
                        </dt>
                        <dd>
                            State of the address in 2 character uppercase. Ex MA
                        </dd>
                        <dt>
                            <code>postcode</code> (optional)
                        </dt>
                        <dd>
                            Postal code of the address. For the US, that is 9 digits, ex. 02021-2345
                        </dd>
                        <dt>
                            <code>guid</code> (required*)
                        </dt>
                        <dd>
                            The guid to look up the constituent by.
                        </dd>
                        <dt>
                            <code>format</code> (optional)
                        </dt>
                        <dd>
                            Format of the API output, json or xml. The default is xml
                        </dd>
                    </dl>
                    <p>
                        * Only one of cons_id or guid is required.
                    </p>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure. On success, a document is returned containing the address records found.
                    </p><samp>&lt;?xml version="1.0" encoding="utf-8"?&gt; &lt;api&gt; &lt;cons_addr&gt; &lt;cons_id&gt;34&lt;/cons_id&gt; &lt;cons_addr_id&gt;234&lt;/cons_addr_id&gt; &lt;addr1&gt;21 abc st&lt;/addr1&gt; &lt;addr2&gt;Suite 102&lt;/addr2&gt; &lt;city&gt;Boston&lt;/city&gt; &lt;state&gt;MA&lt;/state&gt; &lt;postcode&gt;02021-2345&lt;/postcode&gt; &lt;/cons_addr&gt; &lt;cons_addr&gt; &lt;cons_id&gt;34&lt;/cons_id&gt; &lt;cons_addr_id&gt;224&lt;/cons_addr_id&gt; &lt;addr1&gt;1 def st&lt;/addr1&gt; &lt;addr2&gt;Suite 12&lt;/addr2&gt; &lt;city&gt;Boston&lt;/city&gt; &lt;state&gt;MA&lt;/state&gt; &lt;postcode&gt;02022-5176&lt;/postcode&gt; &lt;/cons_addr&gt; &lt;/api&gt;</samp>
                    <p>
                        or
                    </p><samp>[ { "cons_id" : 34, "cons_addr_id" : 234, "addr1" : "21 abc st", "addr2" : "Suite 102", "city" : "Boston", "state" : "MA", "postcode" : "02021-2345" }, { "cons_id" : 34, "cons_addr_id" : 224, "addr1" : "1 def st", "addr2" : "Suite 12", "city" : "Boston", "state" : "MA", "postcode" : "02022-5176" } ]</samp>
                    <p>
                        Format depends on the format attribute passed the to the API
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/address_lookup?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171&amp;cons_id=34&amp;state=MA&amp;city=boston
</samp>
</pre>
            </div><!-- address_delete -->
            <div class="api_method">
                <h5>
                    <code>address_delete</code>
                </h5>
                <div class="description">
                    <p>
                        This method allows deleting constituent addresses in BSD's system.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/address_delete
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>cons_id</code> (required*)
                        </dt>
                        <dd>
                            The numeric id of the constituent.
                        </dd>
                        <dt>
                            <code>addr1</code> (optional)
                        </dt>
                        <dd>
                            Line 1 of the address
                        </dd>
                        <dt>
                            <code>addr2</code> (optional)
                        </dt>
                        <dd>
                            Line 2 of the address
                        </dd>
                        <dt>
                            <code>city</code> (optional)
                        </dt>
                        <dd>
                            City of the address
                        </dd>
                        <dt>
                            <code>state</code> (optional)
                        </dt>
                        <dd>
                            State of the address in 2 character uppercase. Ex MA
                        </dd>
                        <dt>
                            <code>postcode</code> (optional)
                        </dt>
                        <dd>
                            Postal code of the address. For the US, that is 9 digits, ex. 02021-2345
                        </dd>
                        <dt>
                            <code>guid</code> (required*)
                        </dt>
                        <dd>
                            The guid to look up the constituent by.
                        </dd>
                    </dl>
                    <p>
                        * Only one of cons_id or guid is required.
                    </p>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure. On success, a document is returned containing the email records found.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/address_delete?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171&amp;cons_id=34&amp;email=test@test.com
</samp>
</pre>
            </div><!-- phone_register -->
            <div class="api_method">
                <h5>
                    <code>phone_register</code>
                </h5>
                <div class="description">
                    <p>
                        This method allows registering constituent phones in BSD's system.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/phone_register
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>cons_id</code> (required*)
                        </dt>
                        <dd>
                            The numeric id of the constituent.
                        </dd>
                        <dt>
                            <code>phone</code> (required)
                        </dt>
                        <dd>
                            The phone number.
                        </dd>
                        <dt>
                            <code>type</code> (optional)
                        </dt>
                        <dd>
                            The phone type. Valid types: home, work, fax, mobile
                        </dd>
                        <dt>
                            <code>is_subscribed</code> (optional)
                        </dt>
                        <dd>
                            Phone subscription status
                        </dd>
                        <dt>
                            <code>guid</code> (required*)
                        </dt>
                        <dd>
                            The guid to look up the constituent by.
                        </dd>
                    </dl>
                    <p>
                        * Only one of cons_id or guid is required.
                    </p>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/phone_register?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171&amp;cons_id=34&amp;phone=7812341234&amp;type=mobile&amp;is_subscribed=1
</samp>
</pre>
            </div><!-- phone_unsubscribe -->
            <div class="api_method">
                <h5>
                    <code>phone_unsubscribe</code>
                </h5>
                <div class="description">
                    <p>
                        This method allows unsubscribing constituent phones in BSD's system.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/phone_unsubscribe
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>phone</code> (required)
                        </dt>
                        <dd>
                            The phone number.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/phone_unsubscribe?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171&amp;phone=5648974285
</samp>
</pre>
            </div><!-- phone_lookup -->
            <div class="api_method">
                <h5>
                    <code>phone_lookup</code>
                </h5>
                <div class="description">
                    <p>
                        This method allows looking up constituent phones in BSD's system.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/phone_lookup
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>cons_id</code> (required*)
                        </dt>
                        <dd>
                            The numeric id of the constituent.
                        </dd>
                        <dt>
                            <code>phone</code> (required)
                        </dt>
                        <dd>
                            The phone number.
                        </dd>
                        <dt>
                            <code>type</code> (optional)
                        </dt>
                        <dd>
                            The phone type. Valid types: home, work, fax, mobile
                        </dd>
                        <dt>
                            <code>is_subscribed</code> (optional)
                        </dt>
                        <dd>
                            Phone subscription status
                        </dd>
                        <dt>
                            <code>guid</code> (required*)
                        </dt>
                        <dd>
                            The guid to look up the constituent by.
                        </dd>
                    </dl>
                    <p>
                        * Only one of cons_id or guid is required.
                    </p>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure. On success, a document is returned containing the phone records found.
                    </p><samp>&lt;?xml version="1.0" encoding="utf-8"?&gt; &lt;api&gt; &lt;cons_phone&gt; &lt;cons_id&gt;34&lt;/cons_id&gt; &lt;cons_phone_id&gt;234&lt;/cons_phone_id&gt; &lt;phone&gt;1234567890&lt;/phone&gt; &lt;type&gt;home&lt;/type&gt; &lt;is_subscribed&gt;1&lt;/is_subscribed&gt; &lt;/cons_phone&gt; &lt;cons_phone&gt; &lt;cons_id&gt;34&lt;/cons_id&gt; &lt;cons_phone_id&gt;224&lt;/cons_phone_id&gt; &lt;phone&gt;4569871235&lt;/phone&gt; &lt;type&gt;mobile&lt;/type&gt; &lt;is_subscribed&gt;1&lt;/is_subscribed&gt; &lt;/cons_phone&gt; &lt;/api&gt;</samp>
                    <p>
                        or
                    </p><samp>[ { "cons_id" : 34, "cons_phone_id" : 234, "phone" : "1234567890", "type" : "home", "is_subscribed" : 1 }, { "cons_id" : 34, "cons_phone_id" : 224, "phone" : "4569871235", "type" : "mobile", "is_subscribed" : 1 } ]</samp>
                    <p>
                        Format depends on the format attribute passed the to the API
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/phone_lookup?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171&amp;cons_id=34&amp;format=json
</samp>
</pre>
            </div><!-- phone_delete -->
            <div class="api_method">
                <h5>
                    <code>phone_delete</code>
                </h5>
                <div class="description">
                    <p>
                        This method allows deleting constituent phones in BSD's system.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons/phone_delete
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>cons_id</code> (required*)
                        </dt>
                        <dd>
                            The numeric id of the constituent.
                        </dd>
                        <dt>
                            <code>phone</code> (required)
                        </dt>
                        <dd>
                            The phone number.
                        </dd>
                        <dt>
                            <code>type</code> (optional)
                        </dt>
                        <dd>
                            The phone type. Valid types: home, work, fax, mobile
                        </dd>
                        <dt>
                            <code>is_subscribed</code> (optional)
                        </dt>
                        <dd>
                            Phone subscription status
                        </dd>
                        <dt>
                            <code>guid</code> (required*)
                        </dt>
                        <dd>
                            The guid to look up the constituent by.
                        </dd>
                    </dl>
                    <p>
                        * Only one of cons_id or guid is required.
                    </p>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure. On success, a document is returned containing the email records found.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons/phone_delete?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171&amp;cons_id=34&amp;phone=1234567890
</samp>
</pre>
            </div><!-- Constituent Group Begin -->
            <h4>
                Constituent Group (<code>cons_group</code>) API Calls
            </h4><!-- list_constituent_groups -->
            <div class="api_method">
                <h5>
                    <code>list_constituent_groups</code>
                </h5>
                <div class="description">
                    <p>
                        Fetches a list of all constituent groups in the system.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons_group/list_constituent_groups
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns one <code>&lt;cons_group&gt;</code> element (see Common Record Formats) for each constituent group in the system.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons_group/list_constituent_groups?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=3c3452091bf7ff0e2be8b638694a187ca7c0a013
</samp>
</pre>
            </div><!-- get_constituent_group -->
            <div class="api_method">
                <h5>
                    <code>get_constituent_group</code>
                </h5>
                <div class="description">
                    <p>
                        Returns a list (see list_constituent_groups) of one constituent group, or an error code if the group does not exist.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons_group/get_constituent_group
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>cons_group_id</code> (required)
                        </dt>
                        <dd>
                            The constituent group id to get.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns one <code>&lt;cons_group&gt;</code> element (see Common Record Formats) for each constituent group in the system.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons_group/get_constituent_group?cons_group_id=72&amp;api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=3c3452091bf7ff0e2be8b638694a187ca7c0a013
</samp>
</pre>
            </div><!-- get_constituent_group_by_name -->
            <div class="api_method">
                <h5>
                    <code>get_constituent_group_by_name</code>
                </h5>
                <div class="description">
                    <p>
                        Returns a list (see list_constituent_groups) of one constituent group, or an empty list if a group with the given name does not exist.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons_group/get_constituent_group_by_name
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>name</code> (required)
                        </dt>
                        <dd>
                            The constituent group name to look up.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns one <code>&lt;cons_group&gt;</code> element (see Common Record Formats) for the constituent group matching <code>name</code>, or none if there is no match.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons_group/get_constituent_group_by_name?name=my_group_name&amp;api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=3c3452091bf7ff0e2be8b638694a187ca7c0a013
</samp>
</pre>
            </div><!-- get_constituent_group_by_slug -->
            <div class="api_method">
                <h5>
                    <code>get_constituent_group_by_slug</code>
                </h5>
                <div class="description">
                    <p>
                        Returns a list (see list_constituent_groups) of one constituent group, or an empty list if a group with the given slug does not exist.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons_group/get_constituent_group_by_slug
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>slug</code> (required)
                        </dt>
                        <dd>
                            The constituent group slug to look up.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns one <code>&lt;cons_group&gt;</code> element (see Common Record Formats) for the constituent group matching <code>slug</code>, or none if there is no match.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons_group/get_constituent_group_by_slug?slug=my_cons_group&amp;api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=3c3452091bf7ff0e2be8b638694a187ca7c0a013
</samp>
</pre>
            </div><!-- rename_group -->
            <div class="api_method">
                <h5>
                    <code>rename_group</code>
                </h5>
                <div class="description">
                    <p>
                        Renames a constituent group
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons_group/rename_group
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>cons_group_id</code> (required)
                        </dt>
                        <dd>
                            The constituent group id to get.
                        </dd>
                        <dt>
                            <code>new_name</code> (required)
                        </dt>
                        <dd>
                            The new name for the group
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Nothing
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons_group/rename_group?cons_group_id=1&amp;new_name=mynewnameapi_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=3c3452091bf7ff0e2be8b638694a187ca7c0a013
</samp>
</pre>
            </div><!-- add_constituent_groups -->
            <div class="api_method">
                <h5>
                    <code>add_constituent_groups</code>
                </h5>
                <div class="description">
                    <p>
                        Create one or more constituent groups.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons_group/add_constituent_groups
                </p>
                <div class="paremeters">
                    <p>
                        Parameters: <code>POST</code> data
                    </p>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns one <code>&lt;cons_group&gt;</code> element (see Common Record Formats) for each constituent group that was created, with its id and with all optional fields filled in with their defaults.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons_group/add_constituent_groups?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=3c3452091bf7ff0e2be8b638694a187ca7c0a013
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;api&gt;
    &lt;cons_group&gt;
        &lt;name&gt;Star Canvassers&lt;/name&gt;
    &lt;/cons_group&gt;
&lt;/api&gt;
</samp>
</pre>
            </div><!-- delete_constituent_groups -->
            <div class="api_method">
                <h5>
                    <code>delete_constituent_groups</code>
                </h5>
                <div class="description">
                    <p>
                        Delete one or more constituent groups.
                    </p>
                    <p>
                        <strong>Important: because of the potential time involved in manipulating groups, the final result code of this call is always deferred.</strong>
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons_group/delete_constituent_groups
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>cons_group_ids</code> (required)
                        </dt>
                        <dd>
                            A comma-separated list of constituent group ids to delete.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    Example:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons_group/delete_constituent_groups?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=3c3452091bf7ff0e2be8b638694a187ca7c0a013&amp;cons_group_ids=42,43
</samp>
</pre>
            </div><!-- get_cons_ids_for_group -->
            <div class="api_method">
                <h5>
                    <code>get_cons_ids_for_group</code>
                </h5>
                <div class="description">
                    <p>
                        This method takes a constituent group id and returns a list of the constituent ids in that group. The result is a text file with one constituent id per line.
                    </p>
                    <p>
                        <strong>Important: because of the potential time involved in manipulating groups, the final result code of this call is always deferred.</strong>
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons_group/get_cons_ids_for_group
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>cons_group_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the constituent group to list membership of.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons_group/get_cons_ids_for_group?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171&amp;cons_group_id=42
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
cons_group_id=42
</samp>
</pre>
            </div><!-- get_ext_ids_for_group -->
            <div class="api_method">
                <h5>
                    <code>get_ext_ids_for_group</code>
                </h5>
                <div class="description">
                    <p>
                        External IDs (<code>ext_id</code>) are primary key identifiers associated with an external system or service. These can be stored alongside their corresponding Blue State Digital constituent records for easy access and reference. The external ID type (ext_type) specifies which external service/system a given ID is associated with. This method allows retrieving the members of a group as a list of their external ids. Any group members who do not have an external id of the requested type will not be included in the list.
                    </p>
                    <p>
                        <strong>Important: because of the potential time involved in manipulating groups, the final result code of this call is always deferred.</strong>
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code> or <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/cons_group/get_ext_ids_for_group
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>cons_group_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the constituent group to list membership of.
                        </dd>
                        <dt>
                            <code>ext_type</code> (required)
                        </dt>
                        <dd>
                            External ID type. This is a string that identifies the external system with which the ext_ids values are associated. Must be a string containing only lowercase alphanumeric characters and underscores (<kbd>[0-9a-z_]</kbd>) and can be no more than 40 bytes in length.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons_group/get_ext_ids_for_group?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171&amp;cons_group_id=42&amp;ext_type=dwid
</samp>
</pre>
            </div><!-- set_cons_ids_for_group -->
            <div class="api_method">
                <h5>
                    <code>set_cons_ids_for_group</code>
                </h5>
                <div class="description">
                    <p>
                        This method takes a constituent group id and a list of constituent ids and replaces the current group members with the list of constituent ids.
                    </p>
                    <p>
                        <strong>Important: because of the potential time involved in manipulating groups, the final result code of this call is always deferred.</strong>
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>, <code>POST</code>, or <code>PUT</code>
                </p>
                <p class="url">
                    URL: /page/api/cons_group/set_cons_ids_for_group
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>cons_group_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the constituent group to replace membership for.
                        </dd>
                        <dt>
                            <code>cons_ids</code> (required)
                        </dt>
                        <dd>
                            When using GET or POST, a comma-separated list of constituent ids to set as the members of the group. When using PUT, the cons_ids should be one per line as the only PUT data, and cons_group_id must be passed as a GET parameter.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons_group/set_cons_ids_for_group?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
cons_group_id=42&amp;cons_ids=13,47,521,1033,1045
</samp>
</pre>
            </div><!-- set_ext_ids_for_group -->
            <div class="api_method">
                <h5>
                    <code>set_ext_ids_for_group</code>
                </h5>
                <div class="description">
                    <p>
                        External IDs (<code>ext_id</code>) are primary key identifiers associated with an external system or service. These can be stored alongside their corresponding Blue State Digital constituent records for easy access and reference. The external ID type (<code>ext_type</code>) specifies which external service/system a given ID is associated with. This method allows setting the membership of a constituent by specifying external ids instead of having to look up Blue State constituent ids first. The external ids must already exist in Blue State's system. Any external ids that are not found will be ignored and those constituents will not be added to the group.
                    </p>
                    <p>
                        <strong>Important: because of the potential time involved in manipulating groups, the final result code of this call is always deferred.</strong>
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>, <code>POST</code>, or <code>PUT</code>
                </p>
                <p class="url">
                    URL: /page/api/cons_group/set_ext_ids_for_group
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>cons_group_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the constituent group to replace membership for.
                        </dd>
                        <dt>
                            <code>ext_type</code> (required)
                        </dt>
                        <dd>
                            External ID type. This is a string that identifies the external system with which the ext_ids values are associated. Must be a string containing only lowercase alphanumeric characters and underscores (<kbd>[0-9a-z_]</kbd>) and can be no more than 40 bytes in length.
                        </dd>
                        <dt>
                            <code>ext_ids</code> (required)
                        </dt>
                        <dd>
                            When using GET or POST, a comma-separated list of external ids to set as the members of the constituent group. When using PUT, the ext_ids should be one per line as the only PUT data, and cons_group_id and ext_type must be passed as GET parameters.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons_group/set_ext_ids_for_group?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
cons_group_id=42&amp;ext_type=van_id&amp;ext_ids=12772,1129,1983321,2008001,414456
</samp>
</pre>
            </div><!-- add_cons_ids_to_group -->
            <div class="api_method">
                <h5>
                    <code>add_cons_ids_to_group</code>
                </h5>
                <div class="description">
                    <p>
                        This method takes a constituent group id and a list of constituent ids and adds those constituents to the group.
                    </p>
                    <p>
                        <strong>Important: because of the potential time involved in manipulating groups, the final result code of this call is always deferred.</strong>
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>, <code>POST</code>, or <code>PUT</code>
                </p>
                <p class="url">
                    URL: /page/api/cons_group/add_cons_ids_to_group
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>cons_group_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the constituent group to add constituents to.
                        </dd>
                        <dt>
                            <code>cons_ids</code> (required)
                        </dt>
                        <dd>
                            When using GET or POST, a comma-separated list of constituent ids to add to the constituent group When using PUT, the cons_ids should be one per line as the only PUT data, and the cons_group_id must be passed as a GET parameter.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons_group/add_cons_ids_to_group?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
cons_group_id=42&amp;cons_ids=13,47,521
</samp>
</pre>
            </div><!-- add_ext_ids_to_group -->
            <div class="api_method">
                <h5>
                    <code>add_ext_ids_to_group</code>
                </h5>
                <div class="description">
                    <p>
                        External IDs (<code>ext_id</code>) are primary key identifiers associated with an external system or service. These can be stored alongside their corresponding Blue State Digital constituent records for easy access and reference. The external ID type (ext_type) specifies which external service/system a given ID is associated with. This method allows adding constituents to a constituent group by specifying their external id instead of having to look up their Blue State constituent id first. The external ids must already exist in Blue State's system. Any external ids that are not found will be ignored and those constituents will not be added to the group.
                    </p>
                    <p>
                        <strong>Important: because of the potential time involved in manipulating groups, the final result code of this call is always deferred.</strong>
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>, <code>POST</code>, or <code>PUT</code>
                </p>
                <p class="url">
                    URL: /page/api/cons_group/add_ext_ids_to_group
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>cons_group_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the constituent group to add constituents to.
                        </dd>
                        <dt>
                            <code>ext_type</code> (required)
                        </dt>
                        <dd>
                            External ID type. This is a string that identifies the external system with which the ext_ids values are associated. Must be a string containing only lowercase alphanumeric characters and underscores (<kbd>[0-9a-z_]</kbd>) and can be no more than 40 bytes in length.
                        </dd>
                        <dt>
                            <code>ext_ids</code> (required)
                        </dt>
                        <dd>
                            When using GET or POST, a comma-separated list of external ids to add to the constituent group. When using PUT, the ext_ids should be one per line as the only PUT data, and cons_group_id and ext_type must be passed as GET parameters.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons_group/add_cons_ids_to_group?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
cons_group_id=45&amp;ext_type=van_id&amp;ext_ids=12772,1129,1983321
</samp>
</pre>
            </div><!-- remove_cons_ids_from_group -->
            <div class="api_method">
                <h5>
                    <code>remove_cons_ids_from_group</code>
                </h5>
                <div class="description">
                    <p>
                        This method takes a constituent group id and a list of constituent ids and removes those constituents from the group.
                    </p>
                    <p>
                        <strong>Important: because of the potential time involved in manipulating groups, the final result code of this call is always deferred.</strong>
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>, <code>POST</code>, or <code>PUT</code>
                </p>
                <p class="url">
                    URL: /page/api/cons_group/remove_cons_ids_from_group
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>cons_group_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the constituent group to remove constituents from.
                        </dd>
                        <dt>
                            <code>cons_ids</code> (required)
                        </dt>
                        <dd>
                            When using GET or POST, a comma-separated list of constituent ids to remove from the constituent group. When using PUT, the cons_ids should be one per line as the only PUT data, and the cons_group_id must be passed as a GET parameter.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons_group/remove_cons_ids_from_group?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
cons_group_id=42&amp;cons_ids=13,47,521
</samp>
</pre>
            </div><!-- remove_ext_ids_from_group -->
            <div class="api_method">
                <h5>
                    <code>remove_ext_ids_from_group</code>
                </h5>
                <div class="description">
                    <p>
                        External IDs (ext_id) are primary key identifiers associated with an external system or service. These can be stored alongside their corresponding Blue State Digital constituent records for easy access and reference. The external ID type (ext_type) specifies which external service/system a given ID is associated with. This method allows removing constituents from a constituent group by specifying their external id instead of having to look up their Blue State constituent id first. The external ids must already exist in Blue State's system. Any external ids that are not found will be ignored and those constituents will not be removed from the group.
                    </p>
                    <p>
                        <strong>Important: because of the potential time involved in manipulating groups, the final result code of this call is always deferred.</strong>
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>, <code>POST</code>, or <code>PUT</code>
                </p>
                <p>
                    URL: /page/api/cons_group/remove_ext_ids_from_group
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>cons_group_id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the constituent group to remove constituents from.
                        </dd>
                        <dt>
                            <code>ext_type</code> (required)
                        </dt>
                        <dd>
                            External ID type. This is a string that identifies the external system with which the ext_ids values are associated. Must be a string containing only lowercase alphanumeric characters and underscores (<kbd>[0-9a-z_]</kbd>) and can be no more than 40 bytes in length.
                        </dd>
                        <dt>
                            <code>ext_ids</code> (required)
                        </dt>
                        <dd>
                            When using GET or POST, a comma-separated list of external ids to remove from the constituent group. When using PUT, the ext_ids should be one per line as the only PUT data, and cons_group_id and ext_type must be passed as GET parameters.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an HTTP status code indicating success or failure.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <pre>
<samp>
http://XYZ/page/api/cons_group/remove_ext_ids_from_group?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171
</samp>
</pre>
                <p>
                    POST:
                </p>
                <pre>
<samp>
cons_group_id=42&amp;ext_type=van_id&amp;ext_ids=12772,1129,1983321
</samp>
</pre>
            </div>
            <h4>
                Entity API Calls (entity)
            </h4>
            <div class="api_method">
                <h5>
                    <code>fetch</code>
                </h5>
                <div class="description">
                    <p>
                        This method gets the entity with the given primary key.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/api/entity/fetch
                </p>
                <pre>
<samp>
    http://XYZ/page/api/entity/fetch/{class}/{pk1_name}/{pk1_value}/{pk2_name}/{pk2_value}/... (etc. for all primary key parts)
    </samp>
</pre>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        If we are able to retrieve the entity successfully, we will return a 200 OK response code and a one-dimensional JSON map of key - value pairs If the entity is not found for the given ID, we will return a 404 Not Found response code In any other case, e.g. the database is unavailable or other internal error, we will return a 500 Internal Service Error response code
                    </p>
                </div>
                <p>
                    Example:
                </p>
                <pre>
<samp>
    GET /page/api/entity/fetch/cons/cons_id/3908692 HTTP/1.1
    Accept: application/json
    </samp>
</pre>
                <pre>
<samp>
    HTTP/1.0 200 OK
    Date: Fri, 31 Dec 1999 23:59:59 GMT
    Content-Type: application/json
    Content-Length: ####

    {"cons_id":"3908692", "cons_source_id":"4", "prefix":"", "firstname":"Joel", "middlename":"", "lastname":"Barciauskas", "suffix":"", "salutation":"", "gender":"", "birth_dt: 0000-00-00 00:00":"00", "title":"", "employer":"Blue State Digital", "occupation":"se", "income":"0.00", "source":"feb5post", "subsource":"OLWEB", "userid":"foobar@gmail.com", "password":"aaaaaaaaaaaaaaaaaaaaa", "is_validated":"0", "is_banned":"0", "change_password_next_login":"0", "create_dt: 2008-02-06 21:27":"30", "create_app":"signup_db_script", "create_user":"system", "modified_dt: 2011-05-11 00:32":"33", "modified_app":"signup_db_script", "modified_user":"system", "status":"1", "note":"102472629", "is_deleted":"0" }
    </samp>
</pre>
            </div>
            <h4>
                Core API Calls (core)
            </h4>
            <div class="api_method">
                <h5>
                    <code>set_list</code>
                </h5>
                <div class="description">
                    <p>
                        Given a JSON blob of key/value pairs, this api call will save the list in the database (which can be used elsewhere). The value associated with each key must be an array of values for that given key.  Multiple lists may be set at once with this api call.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/api/core/set_list
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <pre>
<samp>
    {
        "funds":
        [
            "fund10",
            "fund20",
            "fund30",
            "fund40",
            "fund50"
        ],
        "appeal":
        [
            "appeal10",
            "appeal20",
            "appeal30",
            "appeal40",
            "appeal50"
        ],
        "campaign":
        [
            "campaign1",
            "campaign2",
            "campaign3",
            "campaign4",
            "campaign5"
        ]
    }
    </samp>
</pre>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Upon success, it will return the message ["SUCCESS"]
                    </p>
                </div>
            </div>
            <h4>
                Outreach (outreach) API Calls
            </h4>
            <div class="api_method">
                <h5>
                    <code>get_page_by_id</code>
                </h5>
                <div class="description">
                    <p>
                        This method gets the outreach page with the given id.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/api/outreach/get_page_by_id
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                    <dl>
                        <dt>
                            <code>id</code> (required)
                        </dt>
                        <dd>
                            The numeric id of the outreach page to get.
                        </dd>
                    </dl>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns the outreach page record (see Common Record Formats).
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <div>
                    <samp>http://XYZ/page/api/outreach/get_page_by_id?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298526&amp;api_mac=ecf927ac5dbcb825068838e5019f24204dabb0e6&amp;id=123</samp>
                </div>
            </div>
            <div class="api_method">
                <h5>
                    <code>set_page_data</code>
                </h5>
                <div class="description">
                    <p>
                        This method creates a new or updates an existing outreach page. The data must be sent in the XML format as defined above (see Common Record Formats) and must be encoded in UTF-8.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/outreach/set_page_data
                </p>
                <div class="parameters">
                    <p>
                        Parameters: POST data
                    </p>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns the outreach page record (see Common Record Formats).
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <div>
                    <samp>http://XYZ/page/api/outreach/set_page_data?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298526&amp;api_mac=ecf927ac5dbcb825068838e5019f24204dabb0e6</samp>
                </div>
                <p>
                    POST:
                </p>
                <div>
                    <samp>&lt;?xml version="1.0" encoding="utf-8"?&gt; &lt;outreach_page <em>id="123"</em>&gt; &lt;outreach_campaign_id&gt;1&lt;/outreach_campaign_id&gt; &lt;cons_id&gt;1&lt;/cons_id&gt; &lt;slug&gt;shortname&lt;/slug&gt; &lt;goal_amt&gt;500.00&lt;/goal_amt&gt; &lt;page_text&gt;Some page text.&lt;/page_text&gt; &lt;page_title&gt;Fundraising Page&lt;/page_title&gt; &lt;email_subject&gt;&lt;/email_subject&gt; &lt;email_body&gt;&lt;/email_body&gt; &lt;offline_contributions_number&gt;0&lt;/offline_contributions_number&gt; &lt;offline_contributions_amount&gt;0.00&lt;/offline_contributions_amount&gt; &lt;/outreach_page&gt;</samp>
                </div>
            </div>
            <p>
                When <code>id</code> is set the existing outreach page is updated, when it is not set a new outreach page is created.
            </p>
            <h4>
                Share (share) API Calls
            </h4>
            <div class="api_method">
                <h5>
                    <code>list_top_recruiters</code>
                </h5>
                <div class="description">
                    <p>
                        This method lists the top share recruiter's names and the number of users each has recruited
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/api/share/list_top_recruiters
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                </div>
                <dl>
                    <dt>
                        <code>limit (optional). The maximum number of share recruiters to return (defaults to 25. Also, the limit cannot exceed 25)</code>
                    </dt>
                </dl>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns zero or more recruitment records (see example response below).
                    </p>
                    <pre>
<samp>
&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;api&gt;
    &lt;shareRecruiter&gt;
        &lt;firstname&gt;Barack&lt;/firstname&gt;
        &lt;lastname&gt;Obama&lt;/lastname&gt;
        &lt;recruitCount&gt;500&lt;/recruitCount&gt;
    &lt;/shareRecruiter&gt;
    &lt;shareRecruiter&gt;
        &lt;firstname&gt;Joe&lt;/firstname&gt;
        &lt;lastname&gt;Biden&lt;/lastname&gt;
        &lt;recruitCount&gt;375&lt;/recruitCount&gt;
    &lt;/shareRecruiter&gt;
    &lt;shareRecruiter&gt;
        &lt;firstname&gt;Jane&lt;/firstname&gt;
        &lt;lastname&gt;Doe&lt;/lastname&gt;
        &lt;recruitCount&gt;350&lt;/recruitCount&gt;
    &lt;/shareRecruiter&gt;
    &lt;shareRecruiter&gt;
        &lt;firstname&gt;John&lt;/firstname&gt;
        &lt;lastname&gt;Doe&lt;/lastname&gt;
        &lt;recruitCount&gt;12&lt;/recruitCount&gt;
    &lt;/shareRecruiter&gt;
    &lt;shareRecruiter&gt;
        &lt;firstname&gt;James&lt;/firstname&gt;
        &lt;lastname&gt;Doe&lt;/lastname&gt;
        &lt;recruitCount&gt;1&lt;/recruitCount&gt;
    &lt;/shareRecruiter&gt;
&lt;/api&gt;
</samp>
</pre>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <div>
                    <samp>http://XYZ/page/api/share/list_top_recruiters?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298526&amp;api_mac=ecf927ac5dbcb825068838e5019f24204dabb0e6&amp;limit=10</samp>
                </div>
            </div>
            <h4>
                Share API Calls
            </h4>
            <div class="api_method">
                <h5>
                    <code>list_forms</code>
                </h5>
                <div class="description">
                    <p>
                        This method retrieves information about all available signup forms.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/api/share/list_forms
                </p>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        An XML-formatted description of zero or more share forms:
                    </p>
                    <pre>
<samp>
&lt;?xml version="1.0"?&gt;
&lt;api&gt;
    &lt;share_form id="1" title="Default Share Form" slug="default-share"&gt;
        &lt;subject&gt;Default subject for the Default Share Form&lt;/subject&gt;
        &lt;body editable="true"&gt;Default body for the Default Share Form&lt;/body&gt;
    &lt;/share_form&gt;
    &lt;share_form id="2" title="Default Tell-a-Friend" slug="tell-a-friend"&gt;
        &lt;subject editable="true"&gt;Default subject for the Default Tell-a-Friend Form&lt;/subject&gt;
        &lt;body&gt;Default body for the Default Tell-a-Friend Form&lt;/body&gt;
    &lt;/share_form&gt;
&lt;/api&gt;
        </samp>
</pre>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <div>
                    <samp>http://XYZ/page/api/share/list_forms?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298526&amp;api_mac=ecf927ac5dbcb825068838e5019f24204dabb0e6</samp>
                </div>
            </div>
            <div class="api_method">
                <h5>
                    <code>get_max_recipients</code>
                </h5>
                <div class="description">
                    <p>
                        This method retrieves the maximum number of recipients that can be targetted by a single share submission.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/api/share/get_max_recipients
                </p>
                <div class="parameters">
                    <p>
                        Parameters: None
                    </p>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        An XML block containing the maximum number of allowable recipients:
                    </p>
                    <pre>
<samp>
&lt;?xml version="1.0"?&gt;
&lt;api&gt;
    &lt;max_recipients_per_share&gt;10&lt;/max_recipients_per_share&gt;
&lt;/api&gt;
        </samp>
</pre>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <div>
                    <samp>http://XYZ/page/api/share/get_max_recipients?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298526&amp;api_mac=ecf927ac5dbcb825068838e5019f24204dabb0e6</samp>
                </div>
            </div>
            <div class="api_method">
                <h5>
                    <code>send_share</code>
                </h5>
                <div class="description">
                    <p>
                        This method sends share the specified share email to the specified recipients.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/share/send_share
                </p>
                <div class="parameters">
                    <p>
                        Parameters: POST Data
                    </p>
                    <p>
                        POST Data for the <code>send_share</code> call is structured as follows:
                    </p>
                    <pre>
<samp>
&lt;?xml version="1.0"?&gt;
&lt;api&gt;
    &lt;share_form id="1"&gt;
        &lt;from&gt;
            &lt;firstname&gt;Jack&lt;/firstname&gt;
            &lt;lastname&gt;Sprat&lt;/lastname&gt;
            &lt;email&gt;jsprat@example.com&lt;/email&gt;
            &lt;replyto&gt;jsprat@example.com&lt;/replyto&gt;
        &lt;/from&gt;
        &lt;recipients&gt;
            &lt;email&gt;llouie@example.com&lt;/email&gt;
            &lt;email&gt;jjiminy@example.com&lt;/email&gt;
        &lt;/recipients&gt;
        &lt;body&gt;THE BODY OF THE SHARE&lt;/body&gt;
        &lt;ip_address&gt;192.0.2.0&lt;/ip_address&gt;
        &lt;user_agent&gt;NCSA_Mosaic/2.0 (Windows 3.1)&lt;/user_agent&gt;
        &lt;source&gt;SOURCE&lt;/source&gt;
        &lt;subsource&gt;SUB-SOURCE&lt;/subsource&gt;
    &lt;/share_form&gt;
&lt;/api&gt;
        </samp>
</pre>
                    <p>
                        Notes:
                    </p>
                    <ol>
                        <li>If <code>&lt;replyto&gt;</code> is ommitted, the senderâ€™s email will be used.
                        </li>
                        <li>If <code>&lt;subject&gt;</code> or <code>&lt;body&gt;</code> is ommitted, then the default for that form will be used.
                        </li>
                    </ol>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        An HTTP 200 code on success, or an HTTP 400 code on failure with errors specified as follows:
                    </p>
                    <pre>
<samp>
&lt;?xml version="1.0"?&gt;
&lt;api&gt;
    &lt;error&gt;Missing share form ID&lt;/error&gt;
    &lt;error&gt;No recipients specified&lt;/error&gt;
    &lt;error&gt;Missing sender first name&lt;/error&gt;
    &lt;error&gt;Missing sender last name&lt;/error&gt;
    &lt;error&gt;Missing sender email&lt;/error&gt;
    &lt;error&gt;Missing IP address&lt;/error&gt;
    &lt;error&gt;Missing user agent&lt;/error&gt;
&lt;/api&gt;
        </samp>
</pre>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <div>
                    <samp>http://XYZ/page/api/share/send_share?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298526&amp;api_mac=ecf927ac5dbcb825068838e5019f24204dabb0e6</samp>
                </div>
            </div>
            <h4>
                Signup (signup) API Calls
            </h4>
            <div class="api_method">
                <h5>
                    <code>list_forms</code>
                </h5>
                <div class="description">
                    <p>
                        This method lists all signup forms and relevant data about those forms.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/api/signup/list_forms
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                </div>
                <dl>
                    <dt>
                        <code>No parameters</code>
                    </dt>
                </dl>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns zero or more signup form records (see Common Record Formats).
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <div>
                    <samp>http://XYZ/page/api/signup/list_forms?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298526&amp;api_mac=ecf927ac5dbcb825068838e5019f24204dabb0e6</samp>
                </div>
            </div>
            <div class="api_method">
                <h5>
                    <code>get_form</code>
                </h5>
                <div class="description">
                    <p>
                        This method gets all properties of the specified signup form.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/api/signup/get_form
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                </div>
                <dl>
                    <dt>
                        <code>signup_form_id</code>
                    </dt>
                    <dd>
                        The id of the signup form to get information for
                    </dd>
                </dl>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns an extended signup form record.
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <div>
                    <samp>http://XYZ/page/api/signup/get_form?signup_form_id=1&amp;api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298526&amp;api_mac=ecf927ac5dbcb825068838e5019f24204dabb0e6</samp>
                </div>
            </div>
            <div class="api_method">
                <h5>
                    <code>list_form_fields</code>
                </h5>
                <div class="description">
                    <p>
                        Retrieves a list of all form fields associated with a specified signup form.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/api/signup/list_form_fields
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                </div>
                <dl>
                    <dt>
                        <code>signup_form_id</code> (required)
                    </dt>
                    <dd>
                        The ID of the signup form.
                    </dd>
                </dl>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns zero or more signup form field records (see Common Record Formats).
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <div>
                    <samp>http://XYZ/page/api/signup/list_form_fields?signup_form_id=9&amp;api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b</samp>
                </div>
            </div>
            <div class="api_method">
                <h5>
                    <code>signup_count</code>
                </h5>
                <div class="description">
                    <p>
                        Retrieves a list of all form fields associated with a specified signup form.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/api/signup/signup_count
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                </div>
                <dl>
                    <dt>
                        <code>signup_form_id</code> (required)
                    </dt>
                    <dd>
                        The ID of the signup form.
                    </dd>
                    <dt>
                        <code>signup_form_field_ids</code> (optional)
                    </dt>
                    <dd>
                        This works a lot like the bundles. For example, 1,4* would only count constituents that have filled out the signup form field with ID 1 and have NOT filled out the signup form field with ID 4
                    </dd>
                </dl>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns a count of the number of constituents that meet the criteria (see Common Record Formats).
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <div>
                    <samp>http://XYZ/page/api/signup/signup_count?signup_form_field_ids=93&amp;signup_form_id=9&amp;api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274300032&amp;api_mac=29541021c4c929cdaed79404abb8375b395e708f</samp>
                </div>
            </div>
            <div class="api_method">
                <h5>
                    <code>count_by_field</code>
                </h5>
                <div class="description">
                    <p>
                        This API allows the user to specify a signup form and signup form field and then the API will return the number of signups per item in the specified signup form field.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/api/signup/count_by_field
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                </div>
                <dl>
                    <dt>
                        <code>signup_form_id</code> (required)
                    </dt>
                    <dd>
                        The ID of the signup form.
                    </dd>
                    <dt>
                        <code>signup_form_field_id</code> (required)
                    </dt>
                    <dd>
                        The ID of the signup form field to group by.
                    </dd>
                </dl>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns a list of form field values and number of constituents that have filled out that field
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: <strong>Always</strong>
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <div>
                    <samp>http://XYZ/page/api/signup/count_by_field?signup_form_id=9&amp;signup_form_field_id=93&amp;api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b</samp>
                </div>
            </div>
            <div class="api_method">
                <h5>
                    <code>process_signup</code>
                </h5>
                <div class="description">
                    <p>
                        Given an XML document that represents the fields of a signup form, this API will process the signup form in the same way that the web form works.
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/signup/process_signup
                </p>
                <div class="parameters">
                    <p>
                        Parameters: POST Data
                    </p>
                    <p>
                        POST Data for each signup form is structured as follows:
                    </p>
                    <pre>
&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;api&gt;
  &lt;signup_form id="9"&gt;
    &lt;signup_form_field id="82"&gt;user@domain.com&lt;/signup_form_field&gt;
    &lt;signup_form_field id="83"&gt;First Name&lt;/signup_form_field&gt;
    &lt;signup_form_field id="84"&gt;Last Name&lt;/signup_form_field&gt;
    &lt;signup_form_field id="85"&gt;Address&lt;/signup_form_field&gt;
    &lt;signup_form_field id="86"&gt;City&lt;/signup_form_field&gt;
    &lt;signup_form_field id="87"&gt;MA&lt;/signup_form_field&gt;
    &lt;signup_form_field id="88"&gt;02135&lt;/signup_form_field&gt;
    &lt;signup_form_field id="89"&gt;USA&lt;/signup_form_field&gt;
    &lt;signup_form_field id="90"&gt;1112223333&lt;/signup_form_field&gt;
    &lt;signup_form_field id="108"&gt;custom-form-field&lt;/signup_form_field&gt;
    &lt;signup_form_field id="120"&gt;
        &lt;items&gt;
            &lt;item&gt;1&lt;/item&gt;
            &lt;item&gt;2&lt;/item&gt;
        &lt;/items&gt;
    &lt;/signup_form_field&gt;
    &lt;signup_form_field id="123"&gt;custom-cons-field&lt;/signup_form_field&gt;
    &lt;signup_form_field id="124"&gt;
        &lt;file&gt;
            &lt;filename&gt;filename.jpg&lt;/filename&gt;
            &lt;data&gt;[BASE64 encoded data]&lt;/data&gt;
        &lt;/file&gt;
    &lt;/signup_form_field&gt;
  &lt;/signup_form&gt;
&lt;/api&gt;

</pre>
                    <p>
                        All form field values are either specified as CDATA or if there is more than one item (in the event of a custom checkbox field), the &lt;items&gt; tag is used to specify an array of items. For file uploads, the &lt;file&gt; tag is used along with the tags for filename and the data encoded in base 64.
                    </p>
                    <p>
                        Multiple signup forms can be specified in one API call. Signups will be processed one at a time and the first signup form to have errors will abort all subsequent signups.
                    </p>
                </div>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Nothing on success (HTTP Status 200) or XML containing errors in the following format:
                    </p>
                    <pre>
&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;api&gt;
    &lt;error&gt;
        &lt;signup_form_id&gt;9&lt;/signup_form_id&gt;
        &lt;signup_form_field_id&gt;108&lt;/signup_form_field_id&gt;
        &lt;description&gt;The value for this radio button field is not valid.&lt;/description&gt;
    &lt;/error&gt;
    &lt;error&gt;
        &lt;signup_form_id&gt;9&lt;/signup_form_id&gt;
        &lt;signup_form_field_id&gt;93&lt;/signup_form_field_id&gt;
        &lt;description&gt;Required&lt;/description&gt;
    &lt;/error&gt;
&lt;/api&gt;

</pre>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <div>
                    <samp>http://XYZ/page/api/signup/process_signup?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b</samp>
                    <p>
                        POST: see the parameters section for an example of the POST data
                    </p>
                </div>
            </div>
            <div class="api_method">
                <h5>
                    <code>get_signups_by_email</code>
                </h5>
                <div class="description">
                    <p>
                        Returns the signup data associated with a given email
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/api/signup/get_signups_by_email
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                </div>
                <dl>
                    <dt>
                        <code>email</code> (required)
                    </dt>
                    <dd>
                        Email address to look up signups for.
                    </dd>
                </dl>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns zero or more stg_signup records (see Common Record Formats).
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <div>
                    <samp>http://XYZ/page/api/signup/get_signups_by_email?email=email%40email.com&amp;api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b</samp>
                </div>
            </div>
            <div class="api_method">
                <h5>
                    <code>get_signups_by_form_id</code>
                </h5>
                <div class="description">
                    <p>
                        Returns the signup data associated with a given signup form
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>GET</code>
                </p>
                <p class="url">
                    URL: /page/api/signup/get_signups_by_form_id
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                </div>
                <dl>
                    <dt>
                        <code>signup_form_id</code> (required)
                    </dt>
                    <dd>
                        Id of the signup form to look up signups for.
                    </dd>
                </dl>
                <div class="returns">
                    <p>
                        Returns:
                    </p>
                    <p>
                        Returns zero or more stg_signup records (see Common Record Formats).
                    </p>
                </div>
                <p class="deferred">
                    Deferred Results: Never
                </p>
                <p>
                    Example:
                </p>
                <p>
                    URL:
                </p>
                <div>
                    <samp>http://XYZ/page/api/signup/get_signups_by_id?signup_form_id=1&amp;api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b</samp>
                </div>
            </div>
            <div class="api_method">
                <h5>
                    <code>clone_form</code>
                </h5>
                <div class="description">
                    <p>
                        Makes a copy of a signup form
                    </p>
                </div>
                <p class="http_method">
                    Method: <code>POST</code>
                </p>
                <p class="url">
                    URL: /page/api/signup/clone_form
                </p>
                <div class="parameters">
                    <p>
                        Parameters:
                    </p>
                </div>
                <dl>
                    <dt>
                        <code>signup_form_id</code> (required)
                    </dt>
                    <dd>
                        The ID of the signup form to clone.
                    </dd>
                    <dt>
                        <code>title</code> (required)
                    </dt>
                    <dd>
                        The title of the new signup form.
                    </dd>
                    <dt>
                        <code>signup_form_name</code> (required)
                    </dt>
                    <dd>
                        The name of the new signup form.
                    </dd>
                    <dt>
                        <code>slug</code> (required)
                    </dt>
                    <dd>
                        The slug of the new signup form, used for url-based access.
                    </dd>
                </dl>
                <div class="returns">
                    <p>
                        The response is the ID of the newly created form:
                    </p>
                    <pre>
<samp>
            &lt;signup_form&gt;
                &lt;id&gt;1&lt;/id&gt;
            &lt;/signup_form&gt;
        </samp>
</pre>
                </div>
            </div>
            <p class="deferred">
                Deferred Results: Never
            </p>
            <p>
                Example:
            </p>
            <p>
                URL:
            </p>
            <div>
                <samp>http://XYZ/page/api/signup/clone_form?signup_form_id=1&amp;api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b</samp>
            </div>
        </div>
        <div class="api_method">
            <h5>
                <code>set_cons_group</code>
            </h5>
            <div class="description">
                <p>
                    Causes signups to get associated with a constituent group
                </p>
            </div>
            <p class="http_method">
                Method: <code>POST</code>
            </p>
            <p class="url">
                URL: /page/api/signup/set_cons_group
            </p>
            <div class="parameters">
                <p>
                    Parameters:
                </p>
            </div>
            <dl>
                <dt>
                    <code>signup_form_id</code>
                </dt>
                <dd>
                    The ID of the signup form to associate with a constituent group. Either this or signup_form_field_id is required
                </dd>
                <dt>
                    <code>signup_form_field_id</code>
                </dt>
                <dd>
                    The ID of the signup form field to associate with a constituent group. Either this or signup_form_id is required
                </dd>
            </dl>
            <div class="parameters">
                <p>
                    POST Parameters:
                </p>
            </div>
            <dl>
                <dt>
                    <code>cons_group_id</code> (required)
                </dt>
                <dd>
                    The ID of the constituent group to associated with the signup form or field.
                </dd>
            </dl>
            <div class="returns">
                Returns nothing.
            </div>
            <p class="deferred">
                Deferred Results: Never
            </p>
            <p>
                Example:
            </p>
            <p>
                URL:
            </p>
            <div>
                <samp>http://XYZ/page/api/signup/set_cons_group?signup_form_id=1&amp;api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b</samp>
            </div>
        </div>
        <h4>
            Event API Calls
        </h4><!-- search_events -->
        <div class="api_method">
            <h5>
                <code>search_events</code>
            </h5>
            <div class="description"></div>
            <p class="http_method">
                Method: <code>GET</code> or <code>POST</code>
            </p>
            <p class="url">
                URL: /page/api/event/search_events
            </p>
            <div class="parameters">
                <p>
                    Parameters:
                </p>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
            </div>
            <p class="deferred">
                Deferred Results: Never
            </p>
            <p>
                Example:
            </p>
            <p>
                URL:
            </p>
            <div>
                <samp>http://XYZ/page/api/event/search_events?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b</samp>
            </div>
        </div><!-- list_rsvps -->
        <div class="api_method">
            <h5>
                <code>list_rsvps</code>
            </h5>
            <div class="description">
                <p>
                    Returns list of all attendee's of a given <code>event_id</code>
                </p>
            </div>
            <p class="http_method">
                Method: <code>GET</code> or <code>POST</code>
            </p>
            <p class="url">
                URL: /page/api/event/list_rsvps
            </p>
            <div class="parameters">
                <p>
                    Parameters:
                </p>
                <dl>
                    <dt>
                        <code>event_id</code> (required)
                    </dt>
                    <dd>
                        The numeric id of the event.
                    </dd>
                </dl>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
                <p>
                    Returns an HTTP status code indicating success or failure and JSON blob with an array of all attendees.
                </p>
            </div>
            <p class="deferred">
                Deferred Results: Never
            </p>
            <p>
                Example:
            </p>
            <p>
                URL:
            </p>
            <div>
                <samp>http://XYZ/page/api/event/<strong>list_rsvps?event_id=11</strong>&amp;api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b</samp>
            </div>
        </div><!-- create_event -->
        <div class="api_method">
            <h5>
                <code>create_event</code>
            </h5>
            <div class="description"></div>
            <p class="http_method">
                Method: <code>GET</code> or <code>POST</code>
            </p>
            <p class="url">
                URL: /page/api/event/create_event
            </p>
            <div class="parameters">
                <p>
                    Parameters:
                </p>
                <dl>
                    <dt>
                        <code>event_api_version</code>
                    </dt>
                    <dd>
                        The event API input JSON version number, the docs below assume version 2
                    </dd>
                    <dt>
                        <code>values</code>
                    </dt>
                    <dd>
                        The event API input JSON with the fields as detailed below
                    </dd>
                </dl>
                <p>
                    Version 2 JSON Parameters:
                </p>
                <dl>
                    <dt>
                        <code>event_type_id</code> (required)
                    </dt>
                    <dt>
                        <code>creator_cons_id</code> (required)
                    </dt>
                    <dt>
                        <code>name</code> (required / static if specified by given event type)
                    </dt>
                    <dt>
                        <code>description</code> (required)
                    </dt>
                    <dt>
                        <code>creator_name</code>
                    </dt>
                    <dt>
                        <code>local_timezone</code>
                    </dt>
                    <dt>
                        <code>venue_name</code> (required)
                    </dt>
                    <dt>
                        <code>venue_addr1</code>
                    </dt>
                    <dt>
                        <code>venue_addr2</code>
                    </dt>
                    <dt>
                        <code>venue_zip</code> (required)
                    </dt>
                    <dt>
                        <code>venue_city</code> (required)
                    </dt>
                    <dt>
                        <code>venue_state_cd</code> (required)
                    </dt>
                    <dt>
                        <code>venue_country</code>
                    </dt>
                    <dt>
                        <code>venue_directions</code>
                    </dt>
                    <dt>
                        <code>host_addr_addressee</code>
                    </dt>
                    <dt>
                        <code>host_addr_addr1</code>
                    </dt>
                    <dt>
                        <code>host_addr_addr2</code>
                    </dt>
                    <dt>
                        <code>host_addr_zip</code>
                    </dt>
                    <dt>
                        <code>host_addr_city</code>
                    </dt>
                    <dt>
                        <code>host_addr_state_cd</code>
                    </dt>
                    <dt>
                        <code>host_addr_country</code>
                    </dt>
                    <dt>
                        <code>host_receive_rsvp_emails</code>
                    </dt>
                    <dt>
                        <code>contact_phone</code>
                    </dt>
                    <dt>
                        <code>public_phone</code>
                    </dt>
                    <dt>
                        <code>attendee_visibility</code> ('NONE'/'COUNT'/'FIRST')
                    </dt>
                    <dt>
                        <code>attendee_require_phone</code>
                    </dt>
                    <dt>
                        <code>attendee_volunteer_show</code> ('1'/'0'/'-1')
                    </dt>
                    <dt>
                        <code>attendee_volunteer_message</code>
                    </dt>
                    <dt>
                        <code>is_official</code>
                    </dt>
                    <dt>
                        <code>pledge_override_type</code>
                    </dt>
                    <dt>
                        <code>pledge_show</code>
                    </dt>
                    <dt>
                        <code>pledge_source</code>
                    </dt>
                    <dt>
                        <code>pledge_subsource</code>
                    </dt>
                    <dt>
                        <code>pledge_require</code>
                    </dt>
                    <dt>
                        <code>pledge_min</code>
                    </dt>
                    <dt>
                        <code>pledge_max</code>
                    </dt>
                    <dt>
                        <code>pledge_suggest</code>
                    </dt>
                    <dt>
                        <code>rsvp_use_default_email_message</code> ('1'/'0'/'-1')
                    </dt>
                    <dt>
                        <code>rsvrp_email_message</code>
                    </dt>
                    <dt>
                        <code>rsvp_email_message_html</code>
                    </dt>
                    <dt>
                        <code>rsvp_use_reminder_email</code>
                    </dt>
                    <dt>
                        <code>rsvp_reminder_email_sent</code>
                    </dt>
                    <dt>
                        <code>rsvp_reminder_hours</code>
                    </dt>
                    <dt>
                        <code>rsvp_allow</code>
                    </dt>
                    <dt>
                        <code>rsvp_require_signup</code> ('1'/'0'/'-1')
                    </dt>
                    <dt>
                        <code>rsvp_disallow_account</code> ('1'/'0'/'-1')
                    </dt>
                    <dt>
                        <code>is_searchable</code>
                    </dt>
                    <dt>
                        <code>chapter_id</code>
                    </dt>
                    <dt>
                        <code>flag_approval</code>
                    </dt>
                    <dt>
                        <code>outreach_page_id</code>
                    </dt>
                    <dt>
                        <code>days</code>
                    </dt>
                    <dd>
                        An array of days, each with the <code>start_datetime_system</code>, of the day and either a <code>duration</code> parameter or a <code>shifts</code> array as specified below. Example as follows:
                        <pre>
<code>[
    {
        'start_datetime_system': '2033-12-01 00:00:00',
        'duration': '120'
    },
    {
        'start_datetime_system': '2033-12-02 00:00:00',
        'shifts': [
            {
                'start_time':   '13:00:00',
                'end_time':     '14:00:00',
                'capacity'      '50'
            },
            {
                'start_time':   '13:00:00',
                'end_time':     '14:00:00',
                'capacity'      '50'
            }
        ]
    }
]
                    </code>
</pre>
                    </dd>
                    <dt>
                        <code>shifts (a parameter for 'days')</code>
                    </dt>
                    <dd>
                        An array of shifts, each with three params <code>start_time</code>, <code>end_time</code>, and <code>capacity</code> as follows:
                        <pre>
<code>[
    {
        'start_time':   '13:00:00',
        'end_time':     '14:00:00',
        'capacity'      '50'
    },
    {
        'start_time':   '13:00:00',
        'end_time':     '14:00:00',
        'capacity'      '50'
    }
]
                    </code>
</pre>
                    </dd>
                </dl>
            </div>
            <div class="returns">
                <p>
                    Returns: the same as get_event_details but for the created event.
                </p>
            </div>
            <p class="deferred">
                Deferred Results: Never
            </p>
            <p>
                Example:
            </p>
            <p>
                URL:
            </p>
            <div>
                <samp>http://XYZ/page/api/event/create_event?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b</samp>
            </div>
        </div><!-- update_event -->
        <div class="api_method">
            <h5>
                <code>update_event</code>
            </h5>
            <div class="description"></div>
            <p class="http_method">
                Method: <code>GET</code> or <code>POST</code>
            </p>
            <p class="url">
                URL: /page/api/event/update_event
            </p>
            <div class="parameters">
                <p>
                    Parameters:
                </p>
                <dl>
                    <dt>
                        <code>event_api_version</code>
                    </dt>
                    <dd>
                        The event API input JSON version number, the docs below assume version 2
                    </dd>
                    <dt>
                        <code>values</code>
                    </dt>
                    <dd>
                        The event API input JSON with the fields as detailed below
                    </dd>
                </dl>
                <p>
                    Version 2 JSON Parameters:
                </p>
                <dl>
                    <dt>
                        <code>event_id</code> (required)
                    </dt>
                    <dt>
                        <code>event_id_obfuscated</code>
                    </dt>
                    <dt>
                        <code>event_type_id</code> (required)
                    </dt>
                    <dt>
                        <code>creator_cons_id</code> (required)
                    </dt>
                    <dt>
                        <code>name</code> (required / static if specified by given event type)
                    </dt>
                    <dt>
                        <code>description</code> (required)
                    </dt>
                    <dt>
                        <code>creator_name</code>
                    </dt>
                    <dt>
                        <code>local_timezone</code>
                    </dt>
                    <dt>
                        <code>venue_name</code> (required)
                    </dt>
                    <dt>
                        <code>venue_addr1</code>
                    </dt>
                    <dt>
                        <code>venue_addr2</code>
                    </dt>
                    <dt>
                        <code>venue_zip</code> (required)
                    </dt>
                    <dt>
                        <code>venue_city</code> (required)
                    </dt>
                    <dt>
                        <code>venue_state_cd</code> (required)
                    </dt>
                    <dt>
                        <code>venue_country</code>
                    </dt>
                    <dt>
                        <code>venue_directions</code>
                    </dt>
                    <dt>
                        <code>host_addr_addressee</code>
                    </dt>
                    <dt>
                        <code>host_addr_addr1</code>
                    </dt>
                    <dt>
                        <code>host_addr_addr2</code>
                    </dt>
                    <dt>
                        <code>host_addr_zip</code>
                    </dt>
                    <dt>
                        <code>host_addr_city</code>
                    </dt>
                    <dt>
                        <code>host_addr_state_cd</code>
                    </dt>
                    <dt>
                        <code>host_addr_country</code>
                    </dt>
                    <dt>
                        <code>host_receive_rsvp_emails</code>
                    </dt>
                    <dt>
                        <code>contact_phone</code>
                    </dt>
                    <dt>
                        <code>public_phone</code>
                    </dt>
                    <dt>
                        <code>attendee_visibility</code> ('NONE'/'COUNT'/'FIRST')
                    </dt>
                    <dt>
                        <code>attendee_require_phone</code>
                    </dt>
                    <dt>
                        <code>attendee_volunteer_show</code> ('1'/'0'/'-1')
                    </dt>
                    <dt>
                        <code>attendee_volunteer_message</code>
                    </dt>
                    <dt>
                        <code>is_official</code>
                    </dt>
                    <dt>
                        <code>pledge_override_type</code>
                    </dt>
                    <dt>
                        <code>pledge_show</code>
                    </dt>
                    <dt>
                        <code>pledge_source</code>
                    </dt>
                    <dt>
                        <code>pledge_subsource</code>
                    </dt>
                    <dt>
                        <code>pledge_require</code>
                    </dt>
                    <dt>
                        <code>pledge_min</code>
                    </dt>
                    <dt>
                        <code>pledge_max</code>
                    </dt>
                    <dt>
                        <code>pledge_suggest</code>
                    </dt>
                    <dt>
                        <code>rsvp_use_default_email_message</code> ('1'/'0'/'-1')
                    </dt>
                    <dt>
                        <code>rsvrp_email_message</code>
                    </dt>
                    <dt>
                        <code>rsvp_email_message_html</code>
                    </dt>
                    <dt>
                        <code>rsvp_use_reminder_email</code>
                    </dt>
                    <dt>
                        <code>rsvp_reminder_email_sent</code>
                    </dt>
                    <dt>
                        <code>rsvp_reminder_hours</code>
                    </dt>
                    <dt>
                        <code>rsvp_allow</code>
                    </dt>
                    <dt>
                        <code>rsvp_require_signup</code> ('1'/'0'/'-1')
                    </dt>
                    <dt>
                        <code>rsvp_disallow_account</code> ('1'/'0'/'-1')
                    </dt>
                    <dt>
                        <code>is_searchable</code>
                    </dt>
                    <dt>
                        <code>chapter_id</code>
                    </dt>
                    <dt>
                        <code>flag_approval</code>
                    </dt>
                    <dt>
                        <code>outreach_page_id</code>
                    </dt>
                    <dt>
                        <code>days</code>
                    </dt>
                    <dd>
                        An array of days, each with the <code>start_datetime_system</code>, of the day and either a <code>duration</code> parameter or a <code>shifts</code> array as specified below. Example as follows:
                        <pre>
<code>[
    {
        'start_datetime_system': '2033-12-01 00:00:00',
        'duration': '120'
    },
    {
        'start_datetime_system': '2033-12-02 00:00:00',
        'shifts': [
            {
                'start_time':   '13:00:00',
                'end_time':     '14:00:00',
                'capacity'      '50'
            },
            {
                'start_time':   '13:00:00',
                'end_time':     '14:00:00',
                'capacity'      '50'
            }
        ]
    }
]
                    </code>
</pre>
                    </dd>
                    <dt>
                        <code>shifts (a parameter for 'days')</code>
                    </dt>
                    <dd>
                        An array of shifts, each with three params <code>start_time</code>, <code>end_time</code>, and <code>capacity</code> as follows:
                        <pre>
<code>[
    {
        'start_time':   '13:00:00',
        'end_time':     '14:00:00',
        'capacity'      '50'
    },
    {
        'start_time':   '13:00:00',
        'end_time':     '14:00:00',
        'capacity'      '50'
    }
]
                    </code>
</pre>
                    </dd>
                </dl>
            </div>
            <div class="returns">
                <p>
                    Returns: the same as get_event_details but for the updated event
                </p>
            </div>
            <p class="deferred">
                Deferred Results: Never
            </p>
            <p>
                Example:
            </p>
            <p>
                URL:
            </p>
            <div>
                <samp>http://XYZ/page/api/event/update_event?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b</samp>
            </div>
        </div><!-- get_event_details -->
        <div class="api_method">
            <h5>
                <code>get_event_details</code>
            </h5>
            <div class="description">
                <p>
                    Returns information about a given event
                </p>
            </div>
            <p class="http_method">
                Method: <code>GET</code> or <code>POST</code>
            </p>
            <p class="url">
                URL: /page/api/event/get_event_details
            </p>
            <div class="parameters">
                <p>
                    Parameters:
                </p>
                <dl>
                    <dt>
                        <code>event_api_version</code>
                    </dt>
                    <dd>
                        The event API input JSON version number, the docs below assume version 2
                    </dd>
                    <dt>
                        <code>values</code>
                    </dt>
                    <dd>
                        The event API input JSON with the fields as detailed below
                    </dd>
                </dl>
                <p>
                    Version 2 JSON Parameters:
                </p>
                <dl>
                    <dt>
                        <code>event_id_obfuscated</code> (required)
                    </dt>
                    <dd>
                        The event_id_obfuscated of the event.
                    </dd>
                </dl>
            </div>
            <div class="returns">
                <p>
                    Returns: A JSON object representing the event
                </p>
                <p>
                    Includes saved information about the event.
                </p>
                <p>
                    Special fields:
                </p>
                <dl>
                    <dt>
                        guest_count
                    </dt>
                    <dd>
                        The number of confirmed guests for each day of the event. If the event has shifts, then the guest_count parameter will appear on each shift object (and represent the confirmed guests for that shift) and not on their respective days.
                    </dd>
                </dl>
            </div>
            <p class="deferred">
                Deferred Results: Never
            </p>
            <p>
                Example:
            </p>
            <pre>
{
        "event_id_obfuscated": "rtt",
        "event_type_id": "1",
        "creator_cons_id": "12",
        "outreach_page_id": null,
        "name": "Our Awesome Event!",
        "description": "It's an awesome event. Come visit.",
        "creator_name": "",
        "venue_name": "The Awesome House",
        "venue_addr1": "123 Fake Street",
        "venue_addr2": "",
        "venue_zip": "02210",
        "venue_city": "Boston",
        "venue_country": "US",
        "venue_directions": "",
        "latitude": "20.000000",
        "longitude": "-20.000000",
        "host_addr_addressee": "",
        "host_addr_addr1": "",
        "host_addr_addr2": "",
        "host_addr_zip": "",
        "host_addr_city": "",
        "host_addr_state_cd": "MA",
        "host_addr_country": "US",
        "host_receive_rsvp_emails": "0",
        "contact_phone": "",
        "public_phone": "0",
        "attendee_visibility": null,
        "attendee_require_phone": "0",
        "attendee_volunteer_show": "0",
        "attendee_volunteer_message": "",
        "is_official": "0",
        "pledge_override_type": "0",
        "pledge_show": "1",
        "pledge_source": null,
        "pledge_subsource": null,
        "pledge_require": "0",
        "pledge_min": "0",
        "pledge_max": "0",
        "pledge_suggest": "0",
        "rsvp_use_default_email_message": "1",
        "rsvp_email_message_html": null,
        "rsvp_use_reminder_email": "1",
        "rsvp_reminder_email_sent": "1",
        "rsvp_allow": "-1",
        "rsvp_require_signup": "-1",
        "rsvp_disallow_account": "-1",
        "is_searchable": "-2",
        "flag_approval": "0",
        "chapter_id": "1",
        "days": [
        {
            "start_dt": "2033-12-31 12:00:00",
            "duration": "120",
            "capacity": "0",
            "guest_count": 1
        }
    ],
        "local_timezone": "North America",
        "venue_state_code": "Leicestershire"
}

</pre>
            <p>
                URL:
            </p>
            <div>
                <samp>http://XYZ/page/api/event/get_event_details?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b</samp>
            </div>
        </div><!-- get_event_parameter_details -->
        <div class="api_method">
            <h5>
                <code>get_event_parameter_details</code>
            </h5>
            <div class="description"></div>
            <p class="http_method">
                Method: <code>GET</code> or <code>POST</code>
            </p>
            <p class="url">
                URL: /page/api/event/get_event_parameter_details
            </p>
            <div class="parameters">
                <p>
                    Parameters:
                </p>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
            </div>
            <p class="deferred">
                Deferred Results: Never
            </p>
            <p>
                Example:
            </p>
            <p>
                URL:
            </p>
            <div>
                <samp>http://XYZ/page/api/event/get_event_parameter_details?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b</samp>
            </div>
        </div><!-- get_create_event_requirements -->
        <div class="api_method">
            <h5>
                <code>get_create_event_requirements</code>
            </h5>
            <div class="description"></div>
            <p class="http_method">
                Method: <code>GET</code> or <code>POST</code>
            </p>
            <p class="url">
                URL: /page/api/event/get_create_event_requirements
            </p>
            <div class="parameters">
                <p>
                    Parameters:
                </p>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
            </div>
            <p class="deferred">
                Deferred Results: Never
            </p>
            <p>
                Example:
            </p>
            <p>
                URL:
            </p>
            <div>
                <samp>http://XYZ/page/api/event/get_create_event_requirements?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b</samp>
            </div>
        </div><!-- get_update_event_requirements -->
        <div class="api_method">
            <h5>
                <code>get_update_event_requirements</code>
            </h5>
            <div class="description"></div>
            <p class="http_method">
                Method: <code>GET</code> or <code>POST</code>
            </p>
            <p class="url">
                URL: /page/api/event/get_update_event_requirements
            </p>
            <div class="parameters">
                <p>
                    Parameters:
                </p>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
            </div>
            <p class="deferred">
                Deferred Results: Never
            </p>
            <p>
                Example:
            </p>
            <p>
                URL:
            </p>
            <div>
                <samp>http://XYZ/page/api/event/get_update_event_requirements?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b</samp>
            </div>
        </div><!-- get_events_for_cons -->
        <div class="api_method">
            <h5>
                <code>get_events_for_cons</code>
            </h5>
            <div class="description"></div>
            <p class="http_method">
                Method: <code>GET</code> or <code>POST</code>
            </p>
            <p class="url">
                URL: /page/api/event/get_events_for_cons
            </p>
            <div class="parameters">
                <p>
                    Parameters:
                </p>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
            </div>
            <p class="deferred">
                Deferred Results: Never
            </p>
            <p>
                Example:
            </p>
            <p>
                URL:
            </p>
            <div>
                <samp>http://XYZ/page/api/event/get_events_for_cons?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b</samp>
            </div>
        </div><!-- get_available_types -->
        <div class="api_method">
            <h5>
                <code>get_available_types</code>
            </h5>
            <div class="description"></div>
            <p class="http_method">
                Method: <code>GET</code> or <code>POST</code>
            </p>
            <p class="url">
                URL: /page/api/event/get_available_types
            </p>
            <div class="parameters">
                <p>
                    Parameters:
                </p>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
            </div>
            <p class="deferred">
                Deferred Results: Never
            </p>
            <p>
                Example:
            </p>
            <p>
                URL:
            </p>
            <div>
                <samp>http://XYZ/page/api/event/get_available_types?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b</samp>
            </div>
        </div>
        <h4>
            VAN Campaign API Calls
        </h4><!-- load_campaign_data -->
        <div class="api_method">
            <h5>
                <code><a name="load_campaign_data" id="load_campaign_data"></a>load_campaign_data</code>
            </h5>
            <div class="description">
                <p>
                    This method allows creating or updating VAN campaign data in BSD's system. The data will be in the form of XML, which must be a valid instance of the VAN campaign schema, van_campaign.xsd. The XML data must be encoded in base64. The API will parse the data and insert or update records in the associated database tables.
                </p>
            </div>
            <p class="http_method">
                Method: <code>POST</code>
            </p>
            <p class="url">
                URL: /page/api/van_campaign/load_campaign_data
            </p>
            <div class="parameters">
                <p>
                    Parameters:
                </p>
                <dl>
                    <dt>
                        <code>xml_data</code> (required)
                    </dt>
                    <dd>
                        Must be base64-encoded. XML data must conform to van_campaign.xsd.
                    </dd>
                    <dt>
                        <code>refresh_universe</code>
                    </dt>
                    <dd>
                        If "<strong>1</strong>" is specified, then the universe is always refereshed. If "<strong>2</strong>" is specified, then the universe will only be refreshed if it is a new universe. If "<strong>3</strong>" is specified, then the universe will not be refreshed. If this option is ommitted, then 2 is assumed.
                    </dd>
                </dl>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
                <p>
                    Returns an HTTP status code indicating success or failure.
                </p>
            </div>
            <p class="deferred">
                Deferred Results: Never
            </p>
            <p>
                Example:
            </p>
            <p>
                URL:
            </p>
            <pre>
<samp>
http://XYZ/page/api/van_campaign/load_campaign_data?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171&amp;refresh_universe=1
</samp>
</pre>
            <p>
                POST:
            </p>
            <p>
                XML
            </p>
        </div><!-- update_universe -->
        <div class="api_method">
            <h5>
                <a name="update_universe" id="update_universe"></a><code>update_universe</code>
            </h5>
            <div class="description">
                <p>
                    This method allows the VAN to notify BSD when a universe file is ready for download. This call is intended for on-demand universe loading.
                </p>
            </div>
            <p class="http_method">
                Method: <code>GET</code> or <code>POST</code>
            </p>
            <p class="url">
                URL: /page/api/van_campaign/update_universe
            </p>
            <div class="parameters">
                <p>
                    Parameters:
                </p>
                <dl>
                    <dt>
                        <code>universe_id</code> (required)
                    </dt>
                    <dd>
                        The universe ID that has been updated.
                    </dd>
                    <dt>
                        <code>filename</code> (required)
                    </dt>
                    <dd>
                        The filename will specifiy the zip file (containing a tab delimited file) can be retrieved from the VAN FTP.
                    </dd>
                </dl>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
                <p>
                    Returns 200 Success except in the case of general API errors (e.g. authentication).
                </p>
            </div>
            <p class="deferred">
                Deferred Results: Never
            </p>
            <p>
                Example:
            </p>
            <p>
                URL:
            </p>
            <pre>
<samp>
http://XYZ/page/api/van_campaign/update_universe?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171&amp;universe_id=123456-12345678&amp;filename=file.ext
</samp>
</pre>
        </div><!-- file_ready_for_download -->
        <div class="api_method">
            <h5>
                <a name="file_ready_for_download" id="file_ready_for_download"></a><code>file_ready_for_download</code>
            </h5>
            <div class="description">
                <p>
                    This method allows the VAN to notify BSD when a file is ready for download. This call is intended to allow for the nightly/weekly transfers between VAN and BSD.
                </p>
            </div>
            <p class="http_method">
                Method: <code>GET</code> or <code>POST</code>
            </p>
            <p class="url">
                URL: /page/api/van_campaign/file_ready_for_download
            </p>
            <div class="parameters">
                <p>
                    Parameters:
                </p>
                <dl>
                    <dt>
                        <code>filename</code> (required)
                    </dt>
                    <dd>
                        The filename will specifiy the zip file (containing a tab delimited file) can be retrieved from the VAN FTP
                    </dd>
                </dl>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
                <p>
                    Returns 200 Success except in the case of general API errors (e.g. authentication).
                </p>
            </div>
            <p class="deferred">
                Deferred Results: Never
            </p>
            <p>
                Example:
            </p>
            <p>
                URL:
            </p>
            <pre>
<samp>
http://XYZ/page/api/van_campaign/file_ready_for_download?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171&amp;filename=file.ext
</samp>
</pre>
        </div><!-- get_van_campaign_id -->
        <div class="api_method">
            <h5>
                <a name="get_van_campaign_id" id="get_van_campaign_id"></a><code>get_van_campaign_id</code>
            </h5>
            <div class="description">
                <p>
                    This method allows the VAN to send BSD an unobfuscated van_campaign_id and receive the BSD obfucated id.
                </p>
            </div>
            <p class="http_method">
                Method: <code>GET</code> or <code>POST</code>
            </p>
            <p class="url">
                URL: /page/api/van_campaign/get_van_campaign_id
            </p>
            <div class="parameters">
                <p>
                    Parameters:
                </p>
                <dl>
                    <dt>
                        <code>van_campaign_id</code> (required)
                    </dt>
                    <dd>
                        The VAN campaign id to be obfuscated.
                    </dd>
                </dl>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
                <p>
                    Returns the obfuscated id with status code 200 Success except in the case of general API errors
                </p>
            </div>
            <p class="deferred">
                Deferred Results: Never
            </p>
            <p>
                Example:
            </p>
            <p>
                URL:
            </p>
            <pre>
<samp>
http://XYZ/page/api/van_campaign/get_van_campaign_id?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171&amp;van_campaign_id=99
</samp>
</pre>
        </div>
        <h4>
            Wrappers API Calls
        </h4><!-- list_wrappers -->
        <div class="api_method">
            <h5>
                <code>list_wrappers</code>
            </h5>
            <div class="description">
                <p>
                    This method lists all wrappers in the BSD system, ordered by internal id.
                </p>
            </div>
            <p class="http_method">
                Method: <code>GET</code>
            </p>
            <p class="url">
                URL: /page/api/wrappers/list_wrappers
            </p>
            <div class="parameters">
                <p>
                    Parameters:
                </p>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
                <p>
                    Returns zero or more wrapper records (see Common Record Formats).
                </p>
            </div>
            <p class="deferred">
                Deferred Results: Never
            </p>
            <p>
                Example:
            </p>
            <p>
                URL:
            </p>
            <pre>
<samp>
http://XYZ/page/api/wrappers/list_wrappers?api_id=my_custom_app&amp;api_ts=1179943123&amp;api_mac=7c2b2e1f825c8946ce9e3bb8607c58ac27cb5171
</samp>
</pre>
        </div>
        <h4>
            Contribution API Calls
        </h4><!-- add_external_contribution -->
        <div class="api_method">
            <h5>
                <code>add_external_contribution</code>
            </h5>
            <div class="description">
                <p>
                    Given a JSON blob that includes the necessary information to record a contribution, this API will process an external contribution the same way that the offline contribution upload tool works. If a constituent does not exist for the given information, one will be created. If a matching constituent already exists, standard de-duping will apply.
                </p>
            </div>
            <p class="http_method">
                Method: <code>POST</code>
            </p>
            <p class="url">
                URL: /page/api/contribution/add_external_contribution
            </p>
            <div class="parameters">
                <p>
                    Parameters: POST data
                </p>
                <p>
                    POST Data should be formatted as a JSON array of objects where each of the objects represents a single external contribution. The following parameters are defined for a single contribution object.
                </p>
                <dl>
                    <dt>
                        external_id (required)
                    </dt>
                    <dd>
                        A unique string associated with a given contribution. If a contribution with this external_id already exists in the system the contribution being submitted will be skipped and an error will be returned with an explanation. Uniqueness will only be enforced across external contributions.
                    </dd>
                    <dt>
                        firstname (required)
                    </dt>
                    <dd>
                        The first name of the contributor.
                    </dd>
                    <dt>
                        lastname (required)
                    </dt>
                    <dd>
                        The last name of the contributor.
                    </dd>
                    <dt>
                        transaction_dt (required)
                    </dt>
                    <dd>
                        The date and time of the contribution. Should be in 'YYYY-MM-DD HH:MM:SS' format. A timezone can also be specified at the end of the string. If a timezone is not specified the system's time zone setting will be used. A list of acceptable timezone names can be found on PHP's <a href="http://us3.php.net/manual/en/timezones.php">List of Supported Timezones</a> page.
                    </dd>
                    <dt>
                        transaction_amt (required)
                    </dt>
                    <dd>
                        A number representing the total amount of the contribution. Note: this should be a JSON number, not a string.
                    </dd>
                    <dt>
                        cc_type_cd (required)
                    </dt>
                    <dd>
                        A string abbreviation of the payment type. The supported codes and the payment type they represent are shown here:
                        <ul>
                            <li>'vs' - Visa
                            </li>
                            <li>'mc' - MasterCard
                            </li>
                            <li>'ax' - American Express
                            </li>
                            <li>'ds' - Discover
                            </li>
                            <li>'ma' - Maestro
                            </li>
                            <li>'ls' - Laser
                            </li>
                            <li>'vd' - Visa Debit
                            </li>
                            <li>'so' - Solo
                            </li>
                            <li>'ve' - Visa Electron
                            </li>
                            <li>'pp' - PayPal
                            </li>
                            <li>'ac' - Automated Clearing House
                            </li>
                            <li>'ck' - Check
                            </li>
                        </ul>
                    </dd>
                    <dt>
                        gateway_transaction_id
                    </dt>
                    <dd>
                        A unique string associated with a given contribution generated by the payment gateway. If a contribution with this gateway_transaction_id already exists in the system then the contribution being submitted will be skipped and an error will be returned with an explanation. This holds true for external contributions as well as contributions made through the Framework Fundraising module.
                    </dd>
                    <dt>
                        contribution_page_id
                    </dt>
                    <dd>
                        A Framework contribution_page_id. The contribution will be attributed to this contribution page. Make sure when using this optional parameter that you are associating the correct contribution page and that you are passing a valid contribution_page_id.
                    </dd>
                    <dt>
                        contribution_page_slug
                    </dt>
                    <dd>
                        A Framework contribution_page slug. The contribution will be attributed to this contribution page. Make sure when using this optional parameter that you are associating the correct contribution page and that you are passing a valid slug.
                    </dd>
                    <dt>
                        outreach_page_id
                    </dt>
                    <dd>
                        A Framework outreach_page_id. The contribution will be attributed to this outreach page including being added to fundraising goals for the given outreach page. Make sure when using this optional parameter that you are associating the correct outreach page and that you are passing a valid outreach_page_id.
                    </dd>
                    <dt>
                        source
                    </dt>
                    <dd>
                        A list of strings of all source codes associated with this contribution. Example: <code>['source1','source2','source3']</code>
                    </dd>
                    <dt>
                        opt_compliance
                    </dt>
                    <dd>
                        Determines whether the contributor's email address will be subscribed to the mailing list. Set to 0 to <b>not</b> subscribe donor. The default behavior when the opt_compliance flag is not sent will be to subscribe the donor.
                    </dd>
                    <dt>
                        addr1
                    </dt>
                    <dd>
                        The addr1 field of the contributor's address.
                    </dd>
                    <dt>
                        addr2
                    </dt>
                    <dd>
                        The addr2 field of the contributor's address.
                    </dd>
                    <dt>
                        city
                    </dt>
                    <dd>
                        The city of the contributor's address.
                    </dd>
                    <dt>
                        state_cd
                    </dt>
                    <dd>
                        The state field of the contributor's address.
                    </dd>
                    <dt>
                        zip
                    </dt>
                    <dd>
                        The zip field of the contributor's address.
                    </dd>
                    <dt>
                        country
                    </dt>
                    <dd>
                        The country field of the contributor's address.
                    </dd>
                    <dt>
                        phone
                    </dt>
                    <dd>
                        The contributor's phone number.
                    </dd>
                    <dt>
                        email
                    </dt>
                    <dd>
                        The contributor's email address
                    </dd>
                    <dt>
                        employer
                    </dt>
                    <dd>
                        The contributor's employer.
                    </dd>
                    <dt>
                        occupation
                    </dt>
                    <dd>
                        The contributor's occupation.
                    </dd>
                    <dt>
                        middlename
                    </dt>
                    <dd>
                        The contributor's middle name.
                    </dd>
                    <dt>
                        prefix
                    </dt>
                    <dd>
                        The contributor's name prefix.
                    </dd>
                    <dt>
                        suffix
                    </dt>
                    <dd>
                        The contributor's name suffix.
                    </dd>
                    <dt>
                        customFields
                    </dt>
                    <dd>
                        An array of key/values that represent the custom field (the key), and the value for the field.
                    </dd>
                </dl>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
                <p>
                    A JSON object containg two properties: 'summary' and 'errors'. The 'summary' property is an object containing three properties 'sucesses', 'failures', and 'missing_ids'. The 'sucesses' and 'failures' properties are counts of the number of contributions which either passed or failed validation. The 'missing_ids' property is the number of contribution which were missing the expected 'external_id' parameter. The 'failures' property contains the total number of failed inserts whether because of errors or missing external_ids. The 'errors' object contains all errors for each contribution which failed to insert. The property names (or keys) in 'errors' are the 'external_id' parameter passed in for each contribution. If a contribution is missing an external_id it will get counted in 'missing_ids' and 'failures' but its errors will not appear in the 'errors' object.
                </p>
            </div>
            <p class="deferred">
                Deferred Results: <strong>Sometimes</strong> (Depends on the number of contributions passed; at or below a configurable threshold, results will not be deferred. The default threshold is 1.)
            </p>
            <p>
                URL:
            </p>
            <pre>
<samp>
http://XYZ/page/api/contribution/add_external_contribution?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b
</samp>
</pre>
            <p>
                Example:
            </p><samp>[ { 'external_id':'UNIQUE_ID_111111111', 'firstname':'Jane', 'lastname':'Smith', 'transaction_dt':'2012-12-31 23:59:59', 'transaction_amt':20, 'cc_type_cd':'vs' }, { 'external_id':'UNIQUE_ID_222222222', 'firstname':'John', 'lastname':'Smith', 'transaction_dt':'2012-12-31 23:59:59', 'transaction_amt':10.50, 'cc_type_cd':'vs', 'gateway_reference_id':'GATEWAY_ID_123456789', 'addr1':'123 Fake Street', 'addr2':'Floor 7', 'city':'Boston', 'state_cd':'MA', 'zip':'02210', 'county':'Suffolk', 'country':'USA', 'phone':'5555551234', 'email':'jsmith@testperson.com', 'source':['page1','page2'], 'employer':'Acme Inc.', 'occupation':'Engineer', 'customFields':{'fund':'fund val','appeal':'appeal val','campaign"':'campaign val'} } ]</samp>
            <p>
                Response:
            </p><samp>{ 'summary': { 'sucesses':40, 'failures':1, 'missing_ids':3 }, 'errors': { 'UNIQUE_ID_1234567890':['Parameter source is expected to be a list of strings', 'Parameter email does not appear to be a valid email address.'] } }</samp>
        </div><!-- get_contributions -->
        <div class="api_method">
            <h5>
                <code>get_contributions</code>
            </h5>
            <div class="description">
                <p>
                    Given a set of filters, this call will return a list of contribution records that fall in the range of the specified filters.
                </p>
            </div>
            <p class="http_method">
                Method: <code>GET</code>
            </p>
            <p class="url">
                URL: /page/api/contribution/get_contributions
            </p>
            <div class="parameters">
                <p>
                    Parameters: GET data
                </p>
                <dl>
                    <dt>
                        filter (required)
                    </dt>
                    <dd>
                        An array of filters to apply to contribution records.
                    </dd>
                </dl>
                <p>
                    List of filters
                </p>
                <dl>
                    <dt>
                        date
                    </dt>
                    <dd>
                        Determines the date range of filters to display. Valid values are:
                        <dl>
                            <dt>
                                today
                            </dt>
                            <dd>
                                Show contributions for the current date
                            </dd>
                            <dt>
                                yesterday
                            </dt>
                            <dd>
                                Show contributions for yesterday
                            </dd>
                            <dt>
                                thisweek
                            </dt>
                            <dd>
                                Show contributions for this week
                            </dd>
                            <dt>
                                lastweek
                            </dt>
                            <dd>
                                Show contributions for last week
                            </dd>
                            <dt>
                                thismonth
                            </dt>
                            <dd>
                                Show contributions for this month
                            </dd>
                            <dt>
                                lastmonth
                            </dt>
                            <dd>
                                Show contributions for last month
                            </dd>
                            <dt>
                                thisquarter
                            </dt>
                            <dd>
                                Show contributions for this quarter
                            </dd>
                            <dt>
                                lastquarter
                            </dt>
                            <dd>
                                Show contributions for last quarter
                            </dd>
                            <dt>
                                ytd
                            </dt>
                            <dd>
                                Show contributions for the year to date
                            </dd>
                            <dt>
                                lastyear
                            </dt>
                            <dd>
                                Show contributions for last year
                            </dd>
                            <dt>
                                past24hours
                            </dt>
                            <dd>
                                Show contributions for the past 24 hours
                            </dd>
                            <dt>
                                past7days
                            </dt>
                            <dd>
                                Show contributions for the past 7 days
                            </dd>
                            <dt>
                                past30days
                            </dt>
                            <dd>
                                Show contributions for the past 30 days
                            </dd>
                            <dt>
                                past90days
                            </dt>
                            <dd>
                                Show contributions for the past 90 days
                            </dd>
                            <dt>
                                past12months
                            </dt>
                            <dd>
                                Show contributions for the past 12 months
                            </dd>
                            <dt>
                                custom
                            </dt>
                            <dd>
                                Use the date ranges specified in the query string params "start" and "stop." Date format is "YYYY-MM-DD HH:MM:SS." The timezone used for dates varies by client configuration, however, most clients will be UTC.
                            </dd>
                        </dl>
                    </dd>
                    <dt>
                        type
                    </dt>
                    <dd>
                        <dl>
                            <dt>
                                external
                            </dt>
                            <dd>
                                All external contributions
                            </dd>
                            <dt>
                                online
                            </dt>
                            <dd>
                                All online contributions (one-time and recurring)
                            </dd>
                            <dt>
                                one
                            </dt>
                            <dd>
                                Online one-time contributions
                            </dd>
                            <dt>
                                recur
                            </dt>
                            <dd>
                                Online recurring contributions
                            </dd>
                            <dt>
                                all
                            </dt>
                            <dd>
                                All contributions (online and external)
                            </dd>
                        </dl>
                    </dd>
                    <dt>
                        source[]
                    </dt>
                    <dd>
                        An array of eligible source codes
                    </dd>
                    <dt>
                        contribution_pages
                    </dt>
                    <dd>
                        A comma-delimited list of eligible contribution_page_ids
                    </dd>
                </dl>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
                <p>
                    A JSON object containg the an array of contribution objects with the following properties:
                </p>
                <ul>
                    <li>stg_contribution_id
                    </li>
                    <li>cons_id
                    </li>
                    <li>date
                    </li>
                    <li>contribution_key
                    </li>
                    <li>ip_address
                    </li>
                    <li>prefix
                    </li>
                    <li>first_name
                    </li>
                    <li>middle_name
                    </li>
                    <li>last_name
                    </li>
                    <li>suffix
                    </li>
                    <li>email
                    </li>
                    <li>amount
                    </li>
                    <li>tickets
                    </li>
                    <li>occupation
                    </li>
                    <li>employer
                    </li>
                    <li>phone
                    </li>
                    <li>addr1
                    </li>
                    <li>addr2
                    </li>
                    <li>city
                    </li>
                    <li>state_cd
                    </li>
                    <li>postal_code
                    </li>
                    <li>country
                    </li>
                    <li>card_type
                    </li>
                    <li>card_last_4
                    </li>
                    <li>custom1
                    </li>
                    <li>custom2
                    </li>
                    <li>custom3
                    </li>
                    <li>source
                    </li>
                    <li>custom_country_field_1
                    </li>
                    <li>custom_country_field_2
                    </li>
                    <li>bill_ref_num
                    </li>
                    <li>page_name
                    </li>
                    <li>note
                    </li>
                    <li>outreach
                    </li>
                    <li>ext_id
                    </li>
                    <li>facebook
                    </li>
                </ul>If the contribution was made in memory of somebody else, it will have the following extra fields:
                <ul>
                    <li>honoree_name
                    </li>
                    <li>honoree_addr
                    </li>
                    <li>honoree_email
                    </li>
                    <li>honoree_recipient_name
                    </li>
                    <li>honoree_type
                    </li>
                </ul>If membership is enabled, it will have the following extra fields:
                <ul>
                    <li>membership_interval
                    </li>
                    <li>membership_lifetime
                    </li>
                    <li>membership_join
                    </li>
                    <li>membership_expiration
                    </li>
                </ul>
            </div>
            <p class="deferred">
                Deferred Results: <strong>Always</strong>
            </p>
            <p>
                URL:
            </p>
            <pre>
<samp>
http://XYZ/page/api/contribution/get_contributions?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b
</samp>
</pre>
            <p>
                Example filters:
            </p>
            <dl>
                <dt>
                    ?filter[date]=today
                </dt>
                <dd>
                    Show contributions from today
                </dd>
                <dt>
                    ?filter[date]=?filter[date]=custom&amp;filter[start]=2012-07-26%2000%3A00%3A00&amp;filter[stop]=2012-07-26%2023%3A59%3A59
                </dt>
                <dd>
                    Show contributions from a specified time period (note: dates are url-encoded)
                </dd>
                <dt>
                    ?filter[date]=thisweek&amp;filter[type]=recur
                </dt>
                <dd>
                    Show all contributions from this week that were recurring
                </dd>
                <dt>
                    ?filter[date]=thisweek&amp;filter[source][]=banana
                </dt>
                <dd>
                    Show all contributions from this week that have a source of "banana"
                </dd>
                <dt>
                    ?filter[date]=thisweek&amp;filter[source][]=banana&amp;filter[source][]=apple
                </dt>
                <dd>
                    Show all contributions from this week that have a source of "banana" or "apple"
                </dd>
                <dt>
                    ?filter[source][]=apple&amp;filter[contribution_pages]=1,2,3
                </dt>
                <dd>
                    Show all contributions with a source of "apple" from contribution pages 1, 2 or 3
                </dd>
            </dl>
            <p>
                Sample Response:
            </p><samp>{ "contributions": [ { "stg_contribution_id": 1, "cons_id": 1, "date": "2012-07-19 00:05:55, "contribution_key": "GdAzDC9hHwYaQlszcY", "ip_address": "127.0.0.1", "prefix": null, "first_name": "John", "middle_name": null, "last_name": "Doe", "suffix": null, "email": "john@doe.com", "amount": "25.00", "tickets": null, "occupation": null, "employer": null, "phone": 1238675309, "addr1": "123 First Ave", "addr2": null, "city": "landville", "state_cd": "AA", "postal_code": "00000", "country": "AA", "card_type": "vs", "card_last_4": "0000", "custom1": null, "custom2": null, "custom3": null, "source": ["somepage"], "custom_country_field1": null, "custom_country_field2": null, "bill_ref_num": "74B49741TE455931Y", "page_name": "General donation page", "note": null, "outreach": null, "ext_id": null, "facebook": null } ] }</samp>
        </div><!-- quick_donate_import -->
        <div class="api_method">
            <h5>
                <code>quick_donate_import</code>
            </h5>
            <div class="description">
                <p>
                    Requires an external id &amp; type for each QuickDonate optin and uses the same logic as OpenID to match each import to a BSD cons record. Once a cons id matched or create this method will then either update or add a cons_charge_token record for that cons. All alert emails for new / updated QD enrollments are sent as if the cons opted in themselves.
                </p>
            </div>
            <p class="http_method">
                Method: <code>POST</code>
            </p>
            <p class="url">
                URL: /page/api/contribution/quick_donate_import
            </p>
            <div class="parameters">
                <p>
                    Parameters: POST data
                </p>
                <dl>
                    <dt>
                        <code>ext_type</code> (required)
                    </dt>
                    <dd>
                        The external ID type.
                    </dd>
                    <dt>
                        <code>ext_id</code> (required)
                    </dt>
                    <dd>
                        The external ID value.
                    </dd>
                    <dt>
                        <code>token_raw</code> (required)
                    </dt>
                    <dd>
                        The raw payment token as generated by your payment gateway.
                    </dd>
                    <dt>
                        <code>token_info</code> (required)
                    </dt>
                    <dd>
                        JSON encoded hash of billing info passed to the gateway for the given payment token. This should be in the same format as what is passed back by the /ctl/Contribution/Quick/GetToken call.
                    </dd>
                    <dt>
                        <code>mobile_optin</code> (optional)
                    </dt>
                    <dd>
                        When this is set to "1" it will trigger a MobileCommons QuickDonate SMS optin.
                    </dd>
                </dl>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
                <div>
                    A JSON object containg four properties:
                    <ul>
                        <li>
                            <code>version</code> - Version number of method call, this is currently "0.1"
                        </li>
                        <li>
                            <code>status</code> - Status of API call, 'success' or 'failure'
                        </li>
                        <li>
                            <code>message</code> - Extended message about the status code
                        </li>
                        <li>
                            <code>payload</code> - An array with extended info, currently will have 'cons_charge_token_id' set to the id of the new/updated charge token record.
                        </li>
                    </ul>It is also possible to get back a non 200 response if the request does not conform to the API, in those cases an error string will be returned.
                </div>
            </div>
            <p class="deferred">
                Deferred Results: <strong>Never</strong>
            </p>
            <p>
                URL:
            </p>
            <pre>
<samp>
http://XYZ/page/api/contribution/quick_donate_import?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b
</samp>
</pre>
        </div><!-- get_custom_form_fields -->
        <div class="api_method">
            <h5>
                <code>get_custom_form_fields</code>
            </h5>
            <div class="description">
                <p>
                    Gets a list of custom form fields for contribution pages
                </p>
            </div>
            <p class="http_method">
                Method: <code>POST</code>
            </p>
            <p class="url">
                URL: /page/api/contribution/get_custom_form_fields
            </p>
            <div class="parameters">
                <p>
                    None
                </p>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
                <div>
                    XML with the following fields
                    <ul>
                        <li>
                            <code>app_form_id</code> ID of the form that this is attached to
                        </li>
                        <li>
                            <code>type</code>The type of field, checkbox, text, etc.
                        </li>
                        <li>
                            <code>settings</code>Extended settings. Notable here is "label" which is treated as the name of the field for display purposes
                        </li>
                        <li>
                            <code>display_order</code>A numeric index for the order of display on this form.
                        </li>
                    </ul>
                </div>
            </div>
            <p class="deferred">
                Deferred Results: <strong>Never</strong>
            </p>
            <p>
                URL:
            </p>
            <pre>
<samp>
http://XYZ/page/api/contribution/get_custom_form_fields?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b
</samp>
</pre>
            <p>
                Response
            </p><samp>&lt;app_form_field id="1" modified_dt="1223651690"&gt; &lt;app_form_id&gt;1&lt;/app_form_id&gt; &lt;type&gt;text&lt;/test&gt; &lt;settings&gt; &lt;label&gt;&lt;![CDATA[Program]]&gt;&lt;/label&gt; &lt;valueIfBlank&gt;&lt;![CDATA[General]]&gt;&lt;/valueIfBlank&gt; &lt;/settings&gt; &lt;display_order&gt;1&lt;/display_order&gt; &lt;/app_form_field&gt;</samp>
        </div><!-- create_recurring -->
        <div class="api_method">
            <h5>
                <code>create_recurring</code>
            </h5>
            <div class="description">
                <p>
                    Creates a recurring contribution record with the info passed in.
                </p>
            </div>
            <p class="http_method">
                Method: <code>POST</code>
            </p>
            <p class="url">
                URL: /page/api/contribution/create_recurring
            </p>
            <div class="parameters">
                <p>
                    Parameters: POST data
                </p>
                <dl>
                    <dt>
                        <code>slug</code> (required)
                    </dt>
                    <dd>
                        The contribution page slug for the recurring contribution to associate this record to.
                    </dd>
                    <dt>
                        <code>transaction_amt</code> (required)
                    </dt>
                    <dd>
                        The contribution amount.
                    </dd>
                    <dt>
                        <code>next_bill_dt</code> (required)
                    </dt>
                    <dd>
                        The date in the format of 'YYYY-MM-DD' of the next charge to be run on this recurring contribution by BSD. If the transaction is monthly, the day needs to be between 1-28.
                    </dd>
                    <dt>
                        <code>token_raw</code> (required)
                    </dt>
                    <dd>
                        The raw payment token as generated by your payment gateway.
                    </dd>
                    <dt>
                        <code>token_info</code> (required)
                    </dt>
                    <dd>
                        JSON encoded hash of billing info passed to the gateway for the given payment token. This should be in the same format as what is passed back by the /ctl/Contribution/Quick/GetToken call.
                    </dd>
                </dl>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
                <div>
                    A JSON object containg four properties:
                    <ul>
                        <li>
                            <code>version</code> - Version number of method call, this is currently "0.1"
                        </li>
                        <li>
                            <code>status</code> - Status of API call, 'success' or 'failure'
                        </li>
                        <li>
                            <code>message</code> - Extended message about the status code
                        </li>
                        <li>
                            <code>payload</code> - An array with extended info, currently will have 'stg_contribution_recurring_id' set to the id of the new/updated stg_contribution_recurring record.
                        </li>
                    </ul>It is also possible to get back a non 200 response if the request does not conform to the API, in those cases an error string will be returned.
                </div>
            </div>
            <p class="deferred">
                Deferred Results: <strong>Never</strong>
            </p>
            <p>
                URL:
            </p>
            <pre>
<samp>
http://XYZ/page/api/contribution/create_recurring?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b
</samp>
</pre>
        </div><!-- quick_donate_sms_optin_get -->
        <div class="api_method">
            <h5>
                <code>quick_donate_sms_optin_get</code>
            </h5>
            <div class="description">
                <p>
                    Finds the QuickDonate record for a given cons GUID or cons_id and gets the MobileCommons (SMS) optin status.
                </p>
            </div>
            <p class="http_method">
                Method: <code>GET</code>
            </p>
            <p class="url">
                URL: /page/api/contribution/quick_donate_sms_optin_get
            </p>
            <div class="parameters">
                <p>
                    Parameters: GET data
                </p>
                <dl>
                    <dt>
                        <code>guid</code> (optional)
                    </dt>
                    <dd>
                        The cons GUID, required if no cons_id is passed in.
                    </dd>
                    <dt>
                        <code>cons_id</code> (optional)
                    </dt>
                    <dd>
                        The cons_id, required if no cons GUID is passed in.
                    </dd>
                </dl>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
                <div>
                    A JSON object containg four properties:
                    <ul>
                        <li>
                            <code>version</code> - Version number of method call, this is currently "0.1"
                        </li>
                        <li>
                            <code>status</code> - Status of API call, 'success' or 'failure'
                        </li>
                        <li>
                            <code>message</code> - Extended message about the status code
                        </li>
                        <li>
                            <code>payload</code> - An array with extended info, currently will return 3 elements:
                            <ul>
                                <li>
                                    <code>cons_charge_token_id</code> - The QuickDonate charge token record id
                                </li>
                                <li>
                                    <code>mobile_optin</code> - The new QuickDonate SMS optin status
                                </li>
                                <li>
                                    <code>phone</code> - The phone number associated with the SMS optin and token
                                </li>
                            </ul>
                        </li>
                    </ul>It is also possible to get back a non 200 response if the request does not conform to the API, in those cases an error string will be returned.
                </div>
            </div>
            <p class="deferred">
                Deferred Results: <strong>Never</strong>
            </p>
            <p>
                URL:
            </p>
            <pre>
<samp>
http://XYZ/page/api/contribution/quick_donate_sms_optin_get?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b
</samp>
</pre>
        </div><!-- quick_donate_sms_optin_set -->
        <div class="api_method">
            <h5>
                <code>quick_donate_sms_optin_set</code>
            </h5>
            <div class="description">
                <p>
                    Finds the QuickDonate record for a given cons GUID or cons_id and sets the MobileCommons (SMS) optin status to what is specified by the parameter 'mobile_optin'.
                </p>
            </div>
            <p class="http_method">
                Method: <code>POST</code>
            </p>
            <p class="url">
                URL: /page/api/contribution/quick_donate_sms_optin_set
            </p>
            <div class="parameters">
                <p>
                    Parameters: POST data
                </p>
                <dl>
                    <dt>
                        <code>guid</code> (optional)
                    </dt>
                    <dd>
                        The cons GUID, required if no cons_id is passed in.
                    </dd>
                    <dt>
                        <code>cons_id</code> (optional)
                    </dt>
                    <dd>
                        The cons_id, required if no cons GUID is passed in.
                    </dd>
                    <dt>
                        <code>mobile_optin</code> (required)
                    </dt>
                    <dd>
                        When this is set to "1" it will trigger a MobileCommons QuickDonate SMS optin. A setting of "0" will optout the matching cons from QuickDonate SMS.
                    </dd>
                    <dt>
                        <code>phone</code> (optional)
                    </dt>
                    <dd>
                        When set the phone number associated with the QuickDonate token will be updated to this otherwise the existing phone number in the token will be used for SMS optin.
                    </dd>
                </dl>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
                <div>
                    A JSON object containg four properties:
                    <ul>
                        <li>
                            <code>version</code> - Version number of method call, this is currently "0.1"
                        </li>
                        <li>
                            <code>status</code> - Status of API call, 'success' or 'failure'
                        </li>
                        <li>
                            <code>message</code> - Extended message about the status code
                        </li>
                        <li>
                            <code>payload</code> - An array with extended info, currently will return 3 elements:
                            <ul>
                                <li>
                                    <code>cons_charge_token_id</code> - The QuickDonate charge token record id
                                </li>
                                <li>
                                    <code>mobile_optin</code> - The new QuickDonate SMS optin status
                                </li>
                                <li>
                                    <code>phone</code> - The phone number associated with the SMS optin and token
                                </li>
                            </ul>
                        </li>
                    </ul>It is also possible to get back a non 200 response if the request does not conform to the API, in those cases an error string will be returned.
                </div>
            </div>
            <p class="deferred">
                Deferred Results: <strong>Never</strong>
            </p>
            <p>
                URL:
            </p>
            <pre>
<samp>
http://XYZ/page/api/contribution/quick_donate_sms_optin_set?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b
</samp>
</pre>
        </div><!-- get_quickdonate_token -->
        <div class="api_method">
            <h5>
                <code>get_quickdonate_token</code>
            </h5>
            <div class="description">
                <p>
                    Finds the QuickDonate record for a given cons_id or GUID and returns the response that the given cons would have gotten if they were authenticated and hits the QuickDonate get token URL. (/ctl/Contribution/Quick/GetToken)
                </p>
            </div>
            <p class="http_method">
                Method: <code>GET</code>
            </p>
            <p class="url">
                URL: /page/api/contribution/get_quickdonate_token
            </p>
            <div class="parameters">
                <p>
                    Parameters: GET data
                </p>
                <dl>
                    <dt>
                        <code>guid</code> (optional)
                    </dt>
                    <dd>
                        The cons GUID, required if no cons_id is passed in.
                    </dd>
                    <dt>
                        <code>cons_id</code> (optional)
                    </dt>
                    <dd>
                        The cons_id, required if no cons GUID is passed in.
                    </dd>
                </dl>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
                <p>
                    A JSON string that is a mirror or what is returned by /ctl/Contribution/Quick/GetToken
                </p>
            </div>
            <p class="deferred">
                Deferred Results: <strong>Never</strong>
            </p>
            <p>
                URL:
            </p>
            <pre>
<samp>
http://XYZ/page/api/contribution/get_quickdonate_token?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;api_mac=95ed2067d75a49943b851c88c84fccbf767
</samp>
</pre>
        </div>
        <h4>
            Mailer API Calls
        </h4><!-- get_stats -->
        <div class="api_method">
            <h5>
                <code>get_stats</code>
            </h5>
            <div class="description">
                <p>
                    Given a list of mailing IDs or a list of mailing send IDs, this method will return the mailing stats for each mailing.
                </p>
            </div>
            <p class="http_method">
                Method: <code>GET</code>
            </p>
            <p class="url">
                URL: /page/api/mailer/get_stats
            </p>
            <div class="parameters">
                <p>
                    Parameters: GET data
                </p>
                <dl>
                    <dt>
                        mailing_ids (required*)
                    </dt>
                    <dd>
                        A comma separated list of mailing IDs that stats are requested for
                    </dd>
                    <dt>
                        mailing_send_ids (required*)
                    </dt>
                    <dd>
                        A comma separated list of mailing send IDs that stats are requested for
                    </dd>
                    <dt>
                        stats_mode (optional)
                    </dt>
                    <dd>
                        Either 'unique' or 'total'. If not provided, both stats will be returned
                    </dd>
                    <dd>
                        <p>
                            * Only one of mailing_ids or mailing_send_ids is required
                        </p>
                    </dd>
                </dl>
            </div>
            <div class="returns">
                <p>
                    Returns:
                </p>
                <p>
                    An array of JSON objects for each matched mailing that has at least been prepped. The objects contain mailing stats data along with basic details about each mailing.
                </p>
            </div>
            <p>
                URL:
            </p>
            <pre>
<samp>
http://XYZ/page/api/mailer/get_stats?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;mailing_ids=2,3&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b
</samp>
</pre>
            <p>
                Response:
            </p><samp>[ { "mailing_id":2, "mailing_send_id":7, "mailing_name":"Testing Stats API", "subject":"Hi Friend!", "sent_dt":"2012-10-26 14:41:41", "from_name":"Test Mailer", "from_email":"user@example.com", "recipient_count":1, "number_unique_opens":1, "number_unique_clicks":0, "number_total_opens":1, "number_total_clicks":6, "number_unsubs":0 }, { "mailing_id":3, "mailing_send_id":2, "mailing_name":"Links2", "subject":"Hi Friend!", "sent_dt":"2012-10-18 16:52:53", "from_name":"Test Mailer", "from_email":"user@example.com", "recipient_count":234324, "number_unique_opens":12344, "number_unique_clicks":3452, "number_total_opens":14555, "number_total_clicks":4232, "number_unsubs":14 } ]</samp>
            <p>
                URL:
            </p>
            <pre>
<samp>
http://XYZ/page/api/mailer/get_stats?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1274298735&amp;mailing_ids=4&amp;stats_mode=unique&amp;api_mac=95ed2067d75a49943b851c88c84fccbf7673281b
</samp>
</pre>
            <p>
                Response:
            </p><samp>[ { "mailing_id":4, "mailing_send_id":7, "mailing_name":"Testing Stats API", "subject":"Hi Friend!", "sent_dt":"2012-10-26 14:41:41", "from_name":"Test Mailer", "from_email":"user@example.com", "recipient_count":1, "number_opens":1, "number_clicks":0, "number_unsubs":0 } ]</samp>
        </div>
        <div class="major_section">
            <h3>
                Understanding the api_mac
            </h3>
            <h4>
                Purpose
            </h4>
            <p>
                It is dangerous to send passwords and API keys over the internet. If they are intercepted in transit, they create an enormous security risk. To reduce this risk, the actual "secret code" (also known as the api_secret) is never transmitted during an API call. Instead, the calling party generates a message and uses the api_secret to create a cryptographically secure "signature" of the message. When the server receives the API call, it verifies that the signature was actually generated using the correct api_secret. If it was not, the request is rejected.
            </p>
            <h4>
                Generating an api_mac
            </h4>
            <p>
                There are three steps to generating an api_mac. The first is to create the "signing_string". This string is a reformatted version of the request you are making. The second step is to sign the "signing string" using the api_secret and the HMAC-SHA1 algorithm. The result is the api_mac. The third step is simply to assemble the request, including the newly generated api_mac.
            </p>
            <h5>
                Step 1: Create the signing string
            </h5>
            <p>
                The signing string consists of four pieces of data, each separated by new lines:
            </p>
            <ol>
                <li>
                    <p>
                        The api_id is the id created in the Blue State Digital Control Panel. Each api_id has a corresponding api_secret.
                    </p>
                    <p>
                        Example:
                    </p>
                    <pre>
<samp>
my_custom_app
</samp>
</pre>
                </li>
                <li>
                    <p>
                        The api_ts is a UNIX timestamp value (epoch seconds) that is sent along with the request. It is used to prevent replay attackes wherein someone resends a previously issued API call at a later date. The api_ts value used in the signing string must be identical to the api_ts sent along with the request.
                    </p>
                    <p>
                        Example:
                    </p>
                    <pre>
<samp>
1179962001
</samp>
</pre>
                </li>
                <li>
                    <p>
                        The URL of the requested API call, without the http:// or the host name.
                    </p>
                    <p>
                        Example:
                    </p>
                    <pre>
<samp>
/page/api/cons/get_constituents_by_ext_id
</samp>
</pre>
                </li>
                <li>
                    <p>
                        The parameters for the API call. These are the standard parameters (api_id, api_ts, and api_ver) as well as any call-specific parameters that will be sent in the URL (if the call uses GET) or as POST data (if the call uses POST). These parameters must not have URL encoding applied when building the signing string (However, they should be encoded in the actual API call). When generating the signing string, it is critical that the parameters be supplied in exactly the same order as they will be supplied in the actual request.
                    </p>
                    <p>
                        Example:
                    </p>
                    <pre>
<samp>
api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1179962001&amp;ext_type=van_id&amp;ext_ids=12772,1129,1983321
</samp>
</pre>
                </li>
            </ol>
            <p>
                Taking the four pieces above, we put them together into a single string, separated by newlines (note that your signing string should contain actual newline characters, not the text "\n"):
            </p>
            <pre>
<samp>
my_custom_app\n1179962001\n/page/api/cons/get_constituents_by_ext_id\napi_ver=1&amp;api_id=my_custom_app&amp;api_ts=1179962001&amp;ext_type=van_id&amp;ext_ids=12772,1129,1983321
</samp>
</pre>
            <p>
                The signing string will be the same regardless of whether the API call uses GET or POST.
            </p>
            <h5>
                Step 2: Use HMAC-SHA1 to get the api_mac
            </h5>
            <p>
                Once you have the signing string, you need to use the HMAC-SHA1 algorithm and the api_secret to generate the api_mac. Most languages have a simple function or class that implements the algorithm. The following examples show how to generate an api_secret in PHP, Perl, and Python. See the section Code Samples and Libraries for details of package requirements for various languages.
            </p>
            <dl>
                <dt>
                    PHP 5.1.2+:
                </dt>
                <dd>
                    <pre>
<samp>
$api_mac = hash_hmac('sha1', $signing_string, $api_secret);
</samp>
</pre>
                </dd>
                <dt>
                    PHP 5.1.1 and earlier:
                </dt>
                <dd>
                    <pre>
<samp>
require_once 'Crypt/HMAC.php';
$signer = new Crypt_HMAC($api_secret, "sha1");
$api_mac = $signer-&gt;hash($signing_string);
</samp>
</pre>
                </dd>
                <dt>
                    Perl:
                </dt>
                <dd>
                    <pre>
<samp>
use Digest::HMAC_SHA1;
$api_mac = hmac_sha1_hex($signing_string, $api_secret);
</samp>
</pre>
                </dd>
                <dt>
                    Python 3:
                </dt>
                <dd>
                    <pre>
<samp>
import hmac, hashlib
api_mac = hmac.new(api_secret.encode(), signing_string.encode(), hashlib.sha1).hexdigest()
</samp>
</pre>
                </dd>
                <dt>
                    Ruby:
                </dt>
                <dd>
                    <pre>
<samp>
require 'openssl'
api_mac = OpenSSL::HMAC.hexdigest('sha1', api_secret, signing_string)
</samp>
</pre>
                </dd>
            </dl>
            <p>
                Different HMAC-SHA1 implementations return the digest in different formats. The BSD API can support either hexadecimal (40 character hex string) or base64 encoding. If you use base64 encoding, be sure to escape any special characters before putting the string into a URL. Do not attempt to use a binary (unencoded) digest in a URL.
            </p>
            <h5>
                Step 3: Assemble the request
            </h5>
            <p>
                Finally, assemble the full request. The api_id, api_ts, and api_mac parameters ALWAYS go into the URL of the API request, even if the other API call parameters are sent via POST variables. For the purposes of our example, we'll assume that the api_mac generated by the above process was 955e6266d18921a25feb2bebeaa8d5e2bfb518c4. In this case, the final URL for the API call would be:
            </p>
            <pre>
<samp>
http://XYZ/page/api/cons/get_constituents_by_ext_id?api_ver=1&amp;api_id=my_custom_app&amp;api_ts=1179962001&amp;api_mac=955e6266d18921a25feb2bebeaa8d5e2bfb518c4&amp;ext_type=van_id&amp;ext_ids=12772,1129,1983321
</samp>
</pre>
            <h4>
                Code Samples and Libraries
            </h4>
            <p>
                The authentication system of the BSD API requires the use of the HMAC-SHA1 algorithm. This is available natively or with an add-on module in all major web programming environments.
            </p>
            <dl>
                <dt>
                    PHP
                </dt>
                <dd>
                    Use hash_hmac with PHP 5.1.2+, or the Crypt_HMAC PEAR module.
                </dd>
                <dt>
                    Perl
                </dt>
                <dd>
                    Use the Digest::HMAC CPAN module
                </dd>
                <dt>
                    .NET
                </dt>
                <dd>
                    Use the System.Security.Cryptography.HMACSHA1 class.
                </dd>
                <dt>
                    Ruby
                </dt>
                <dd>
                    Use the 'openssl' module.
                </dd>
                <dt>
                    Python
                </dt>
                <dd>
                    Use the 'hmac' and 'hashlib' modules.
                </dd>
            </dl>
            <p>
                <em>[TODO: Provide simple functions for generating an api_mac from the component pieces]</em>
            </p>
        </div>
        <div class="major_section">
            <h3>
                Common HTTP Status Codes
            </h3>
            <p>
                The following HTTP status codes may be returned by an API call:
            </p>
            <dl>
                <dt>
                    200 OK
                </dt>
                <dd>
                    The API call was successfully processed. The results of the call are contained in the body of the HTTP response.
                </dd>
                <dt>
                    202 Accepted
                </dt>
                <dd>
                    The API call was accepted, but results are not immediately available. The body of the HTTP response will contain a deferred_id which can be used to retrieve the results. See the Deferred Results section for details.
                </dd>
                <dt>
                    204 No Content
                </dt>
                <dd>
                    The deferred result API call succeeded but produced no output.
                </dd>
                <dt>
                    403 Forbidden
                </dt>
                <dd>
                    The api_key you are using does not have permission to access the requested API call. Use the Blue State Digital Control Panel to grant access to the desired API call to the api_key you are using.
                </dd>
                <dt>
                    404 Not Found
                </dt>
                <dd>
                    You attempted to access an unknown API call. This could be due to an invalid URL, or you may have attempted to access an API function that relates to software that is not installed in your Control Panel. If you receive this error and are certain that you are using the correct URL syntax, contact Blue State Digital for assistance.
                </dd>
                <dt>
                    405 Method Not Allowed
                </dt>
                <dd>
                    You attempted to access an API call that supports only GET using a POST request, or a call that supports only POST using a GET request.
                </dd>
                <dt>
                    409 Conflict
                </dt>
                <dd>
                    The parameters required by the method call are either missing, malformed, or otherwise rejected.
                </dd>
                <dt>
                    410 Gone
                </dt>
                <dd>
                    The results produced by the deferred result API call have already been retrieved and are no longer stored on the server.
                </dd>
                <dt>
                    500 Internal Server Error
                </dt>
                <dd>
                    The system encountered a fatal error while processing your API call. The body of the response may contain more detailed information as well as a Problem ID that can be used to report the issue to Blue State Digital.
                </dd>
            </dl>
        </div>
        <div class="major_section">
            <h3>
                Common API calling issues
            </h3>
            <ul>
                <li>thing 1
                </li>
                <li>thing 2
                </li>
            </ul>
        </div>
