# timelineproject

The most difficult part of this task was the handling of the API. Following my prototype, I needed to render an image, the year and the title of the data items. This was a complicated process, in which I struggled to isolate the specific properties.

Firstly, I wanted to automatically call the API. I did this by using the vue mounting event handler to fetch the data using;

mounted() { this.fetchData(); },

After inspecting the JSON I found that not all rows had an image preview which was envisioned in my prototype. The Detail and Main components use a different API call and hence needed different fixes.

On the Main component, I had to use advanced API GET parameters hasMedia = yes;
"hasMedia=yes & year=1970-2000 & query="
With the searchString as 'women'

On the Details component, I could not use the API GET parameters to easily get the image, so I had to create a custom function to find get children of the 'media' node which had a 'preview' and a 'file path'; basically look into the JSON to get an image.

Now that the images had rendered and I had the information I wanted the users to see. On the Main component I used a W3 schools timeline as a base for the timeline used as I was too unexperienced to work out the components through VUE (SEE REFERENCES). Once I had my CSS I changed the look and size of the containers and timeline to fit my design. It was challenging for me to create my design following the prototype I had previously created, because of this reason. There were little to no resources to enable me to make the timeline more fluid and feature a curved, not straight line. Completing to the best of my abilities, the timeline ended up being one long straight line but this has media queries to optimise the view for large, medium and small form factors, i.e. 3 media queries are in my CSS.

Once I had finalised the media responses and look of the first page, I then focused on the rendering of the second version of the page. I created a skeleton via the first page, Home.vue and named it Details.vue. Connecting the data from the first page to the second page was the next task, and took me lots of debugging to figure out. I ended up gathering the data from the detail API call that relied on fetching via id. Figuring out the data set from the API was the most challenging on this front, as it took me multiple tries to figure out the correct param to call on. Using the VUE router I then made the timeline cards clickable, navigate to the details VUE and render the content.

Creatively, I wasn't able to explore all the options I wanted from the prototype because the learning of VUE and API's took significant time. For instance, I was unable to create the dramatic headers and footers of my original design. Resulting in the addition of only the header and no footer, as well as no NFSA logo.

I was really challenged by this assessment piece and it took me a lot of debugging and interaction with the API documentation to set up a site that physically worked. This frustrated me and I was often caught up in the aesthetics or appearance of the web page rather than its ability to do its job.

Overall I'm very happy with the result and the fluidity that the media queries provide between devices. It is definitely usable and achieved everything I needed. In the future I would dedicate more time to the CSS with a more planned out timeline of when things needed to be completed. Allowing me to create a design that’s as accessible as it is beautiful. I would also like to go into VUE deeper and explore the reuse of public components.

[LINK TO WEBSITE](http://localhost:5173/timelineproject/)

# References

[LINK TO W3 SCHOOLS TIMELINE EX](https://www.w3schools.com/howto/howto_css_timeline.asp)
