gpxpy -- GPX file parser
========================

This is a simple python library for parsing and manipulating GPX files. GPX is an XML based format for GPS tracks.

You can see it in action on [my online GPS track editor and organizer](http://www.trackprofiler.com).

Usage
-----

    import gpxpy
    import gpxpy.gpx

    # Parsing an existing file:
    # -------------------------

    gpx_file = open('test_files/cerknicko-jezero.gpx', 'r')

    gpx = gpxpy.parse(gpx_file)

    for track in gpx.tracks:
        for segment in track.segments:
            for point in segment.points:
                print 'Point at ({0},{1}) -> {2}'.format(point.latitude, point.longitude, point.elevation)

    for waypoint in gpx.waypoints:
        print 'waypoint {0} -> ({1},{2})'.format(waypoint.name, waypoint.latitude, waypoint.longitude)
        
    for route in gpx.routes:
        print 'Route:'
        for point in route.points:
            print 'Point at ({0},{1}) -> {2}'.format(point.latitude, point.longitude, point.elevation)

    # There are many more utility methods and functions:
    # You can manipulate/add/remove tracks, segments, points, waypoints and routes and
    # get the GPX XML file from the resulting object:

    print 'GPX:', gpx.to_xml()

    # Creating a new file:
    # --------------------

    gpx = gpxpy.gpx.GPX()

    # Create first track in our GPX:
    gpx_track = gpxpy.gpx.GPXTrack()
    gpx.tracks.append(gpx_track)

    # Create first segment in our GPX track:
    gpx_segment = gpxpy.gpx.GPXTrackSegment()
    gpx_track.segments.append(gpx_segment)

    # Create points:
    gpx_segment.points.append(gpxpy.gpx.GPXTrackPoint(2.1234, 5.1234, elevation=1234))
    gpx_segment.points.append(gpxpy.gpx.GPXTrackPoint(2.1235, 5.1235, elevation=1235))
    gpx_segment.points.append(gpxpy.gpx.GPXTrackPoint(2.1236, 5.1236, elevation=1236))

    # You can add routes and waypoints, too...

    print 'Created GPX:', gpx.to_xml()

GPX Version:
------------

gpx.py can parse and generate GPX 1.0 and 1.1 files. Note that the generated file will always be a valid XML document, but it may not be (strictly speaking) a valid GPX document. For example, if you set gpx.email to "my.email AT mail.com" the generated GPX tag won't confirm to the regex pattern. And the file won't be valid. Most applications will ignore such errors, but... Be aware of this!

WARNING: The only part of the GPX standard which is not completely implenmented are GPX extensions. The API for GPX extensions will change in future versions!!!

XML parsing
-----------

If lxml is available, then it will be used for XML parsing.
Otherwise minidom is used.
Note that lxml is 2-3 times faster so, if you can choose -- use it :)

Pull requests
-------------

OK, so you found a bug and fixed it. Before sending a pull request -- check that all tests are OK with python 2.6+ and python 3+.

Run all tests with:

    $ python -m unittest test
    $ python3 -m unittest test

Run only minidom parser tests with:

    $ python -m unittest test.MinidomTests
    $ python3 -m unittest test.MinidomTests

Run only lxml parser tests with:

    $ python -m unittest test.LxmlTests
    $ python3 -m unittest test.LxmlTests

Run a single test with:

    $ python -m unittest test.LxmlTests.test_method
    $ python3 -m unittest test.LxmlTests.test_method

GPXInfo
-------

The repository contain a little command line utility to extract basic statistics from a file.
Example usage:

    $ gpxinfo voznjica.gpx 
    File: voznjica.gpx
      Length 2D: 63.6441229018
      Length 3D: 63.8391428454
      Moving time: 02:56:03
      Stopped time: 00:21:38
      Max speed: 14.187909492m/s = 51.0764741713km/h
      Total uphill: 1103.1626183m
      Total downhill: 1087.7812703m
      Started: 2013-06-01 06:46:53
      Ended: 2013-06-01 10:23:45

See also
--------

See also [srtm.py](https://github.com/tkrajina/srtm.py) if your track lacks elevation data.

License
-------

GPX.py is licensed under the [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0)

