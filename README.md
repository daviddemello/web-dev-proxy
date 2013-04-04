Web-Dev-Proxy
=============

# Browse a remote web site with the option to supply local versions for css files and their dependencies.

This is a Node.js application that sets up two local servers on the machine running it. 

The first server (Proxy) runs on port 8080 and makes proxied calls to a remote web host and serves a modified result locally. 

The second server (Local Dev) runs on port 9090 and serves local versions of css files and their dependencies. It collects these local versions based on what css files get included in the pages returned by Proxy.

Proxy's manipulations of the pages it serves include:

* Adding a BASE tag to the file's HEAD tag (assuming it's an HTML file) so that images and other resources referenced by realtive URLs will be brought in properly from the remote host by the browser (and won't require separate requests through Proxy).
* Including jQuery in the HEAD if it is not found to have already been added to the page.
* Appending Javascript to the bottom of the page that uses jQuery to rewrite any relative links in the page to the Proxy. This way a user can navigate the site and stay on the Proxy rather than constantly being kicked back up to the remote server. 
* Modifying the source attributes of stylesheet links so that they are pulled from the Local Dev server rather than from their actual location on the remote web server.

When a request is made to the Local Dev server, it first looks for a version of the requested file in the local file sysytem. if it finds one, it uses it. If it doesn't, it gets the file from the remote server and writes it to local storage before returning it.

Currently this is hard coded to connect by proxy to http://www.ilr.cornell.edu. 

To run the app, navigate to its installed directory and type 'node webdevproxy.js'. Then fire up a browser and go to http://localhost:8080. As you browse pages, local versions of remote css files will accumulate in htdocs. Edit these files for fun and profit.

This app requires the Node.js module mkdirp. If you get an error that it's missing, run 'npm install mkdirp' to get it.
