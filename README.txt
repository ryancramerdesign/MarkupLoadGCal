ProcessWire Google Calendar Loader
==================================

Given a Google Calendar XML feed URL, this module will pull it, cache it, 
and let you foreach() it or render it. 


USAGE
=====

The MarkupLoadRSS module is used from your template files. 

To use it, you get a copy of the module, tell it what URL to load from (with 
the load method), and then execute a find() query with a selector string. 
It works basically the same as a $pages->find("selector string"). 

    $cal = $modules->get("MarkupLoadGCal"); 
    $cal->load('http://calendar XML feed'); 
    $items = $cal->find('from=2011-12-1, to=2011-12-31'); 
    foreach($items as $item) echo "<p>{$item->title}</p>";

To get a Google calendar URL, you would go into your (or another) calendar on
Google and click to the calendar's settings. At the bottom of the screen, 
you will see a section called "Calendar Address" with XML, ICAL, and HTML. 
Copy the XML address, and use that with this module. 

The selector properties you may provide to find() are: 

- from: Starting date in any common date/time format (or unix timestamp). 
- to: Ending date in any common date/time format (or unix timestamp).
- keywords: Keyword(s) to search for in the Google calendar events.
- limit: Max number of items to load (default = 100).
- sort: May be 'date', '-date', 'modified' or '-modified'.
- html: Set to '0' if you don't want the event description in HTML.

The find() method will return the found items. You can then foreach() these
items to generate your output. A render() method is also included with the
items which provides some default output, if you want it. 

Each calendar item has the following properties:

- title: The title of the event
- description: Detailed description of the event (in HTML, unless disabled)
- location: Where the event will take place
- author: Author of this item
- from: Timestamp of when the event begins
- to: Timestamp of when the event ends
- dateFrom: Formatted date string of when the event begins*
- dateTo: Formatted date string of when the event ends*

*The dateFrom and dateTo properties may be PHP date() or strftime() format.

See the module file for additional configuration options. 


EXAMPLES
========

Example #1: Simplest example
----------------------------------

    $cal = $modules->get("MarkupLoadGCal"); 

    // may be any google calendar XML feed URL
    $cal->load('http://www.google.com/calendar/feeds/3icgo6ucgvsf6bi5orld9moqqc%40group.calendar.google.com/public/full'); 

    // find all items for December, 2011
    $items = $cal->find('from=2011-12-1, to=2011-12-31'); 

    // render items using built-in rendering
    echo $items->render();


Example #2: Rendering your own items
------------------------------------

    $cal = $modules->get("MarkupLoadGCal"); 
    $cal->load('http://www.google.com/calendar/feeds/3icgo6ucgvsf6bi5orld9moqqc%40group.calendar.google.com/public/full'); 
    $items = $cal->find('from=2011-12-1, to=2011-12-31'); 

    foreach($items as $item) {

        echo "

        <h2>{$item->title}</h2>

        <p>
        <b>Date From:</b> {$item->dateFrom} (Timestamp: {$item->from}) <br />
        <b>Date To:</b> {$item->dateTo} (Timestamp: {$item->to}) <br />
        <b>Location:</b> {$item->location} <br />
        <b>Author:</b> {$item->author}
        </p>

        {$item->description}

	";
        
    }


Example #3: Finding Keywords
----------------------------------------

    $cal = $modules->get("MarkupLoadGCal"); 
    $cal->load('http://www.google.com/calendar/feeds/3icgo6ucgvsf6bi5orld9moqqc%40group.calendar.google.com/public/full'); 
    $items = $cal->find("from=Aug 1 2011, to=Dec 1 2011, keywords=Eddie Owen");
    echo $items->render();



OPTIONS
=======

See the module's class file (MarkupLoadGCal) for a description of all options
in the comments of the $options and $markup arrays at the top of the class file.


HANDLING ERRORS
===============

If an error occurred when loading the feed, the $cal will have an 'error'
property populated with a message of what error occurred: 

    $items = $cal->find("..."); 
    if(empty($items) && $cal->error) {
        // an error occurred
        echo "Error! " . $cal->error; 
    }

$items will be blank if an error occurs, but it will always be of the same type,
so it's up to you whether you want to detect errors. If you don't, then your
calendar output will just indicate that nothing was found. 


SUPPORT
=======

Visit the ProcessWire forum at http://processwire.com/talk/

------

Copyright 2014 by Ryan Cramer
