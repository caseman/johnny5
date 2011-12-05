******************
Johnny5 Design Doc
******************

Johnny5 is a tool for creating html5 applications. It consists of two main
parts:

#. A base framework for client-side application code.

#. An creator application that allows users to construct their own J5 apps.

The J5 creator application is itself written using the base framework, thus
J5 itself could be described as the very first J5 application.

Core Philosophy
===============

#. J5 shall promote the development of fun, simple, and beautiful apps.

#. Development using J5 should be both fun and productive.

#. The tools should be accessible to non-programmers, but still highly useful
   to skilled programmers alike.

#. J5 shall promote the best practices of html5 development.

#. J5 will provide access to state of the art tools and services for web
   development.

#. J5 will provide friendly and fun tools for use in development.

#. J5 will not require the use of any particular tools, and will be friendly
   to standard development tools (e.g., text editors and vcs).

#. J5 is provided under a permissive open source license, that does not
   dictate the license of apps developed in it.

#. The development of J5 itself is open and transparent.

#. Complete documentation shall be provided with each release, however as much
   as possible, the J5 app creator should be usable without documentation.

#. Add-on components to J5 will be treated as first-class citizens, allowing
   others to contribute wholly new functionality that can be fully integrated
   with the J5 toolkit.

#. It shall be possible to deploy J5 apps using a wide variety of industry 
   standard tools and servers.

Architecture
============

The base architecture of J5 is intended to be simple and utimately familiar to
web application developers. At the same time it introduces modularity and
pluggability to greatly enhance developer efficiency.

In the most general sense, a J5 application is simply a collection of
components. These components are either provided by J5, provided by a third
party developer, or developed specifically by the application developer.

Components provide specific functionality once they are configured for an
application. Some example components for an application might include:

- Storage components that provide application data persistence.
- An auth component that allows users to sign in.
- A registration component the allows new users to register.
- Custom view components that describe the app's user interface.
- Widgets that enhance the app's views.
- Scripts that contain custom application logic.

Those components are very general, however. Let's get a bit more specific:

- A *local storage* component that allows the app to store data persistently
  on the client (i.e., browser).

- A *REST storage* that allows the app to store data on the server using
  a standard HTTP interface. The server could store data in files, or in
  a database, such as CouchDB, or PostgreSQL.

- An *LDAP storage* that allows users and groups to be queried for use
  in authentication and authorization.

- A *web auth* component that allows users to use their Facebook, Twitter,
  or Google account to register, or log in.

- An *avatar* widget that allows users to create or use an existing
  profile picture.

- Custom *home page*, and *profile* views designed using the J5 view creator.

The above is a small sampling of the components that might be used to create
a social media app in J5. Many of these components might work together as
well. For instance:

- The *web auth* component could use *local storage* to remember the user's
  auth credentials.

- The *profile* view could use the *avatar* widget, which in turn depends on
  the *web auth* component.

These dependencies are automatically managed by J5.

If you are already a web developer, you may be wondering how this all boils
down in html5 terms. Obviously the browser does not understand components
at this level, and thus we must bridge this gap somehow. The idea
is that J5 acts as a lever, allowing the developer to think and design
in these high level terms, and largely let J5 handle the details of how the
rubber meets the road. 

Component Structure
-------------------

At this level, components are like the *atoms* of an application. To get a
more detailed understanding, let's look closer at how components themselves
are constructed. The *subatomic* structure, as it were.

Components consist of:

- HTML structure.
- CSS styles.
- Dependancies on other components.
- Scripts.
- JSON Configuration.

So you can see that the component level is not far removed from the standard
toolkit that web developers are already familiar with. The idea is that a
components are like LEGO&reg; bricks that can be plugged together to make
something. J5 provides a few simple things to facilitate this:

- A standard component definition.
- A mechanism to assemble components into an html5 app.

These two things are the essence of what J5 is. Of course this is only
a skeleton, the real meat comes from components themselves. But, before
we can make all these great components, and make them useful, we must know
both how to create them, and how they fit together.

A component by itself is really just a potential piece of functionality. It
doesn't actually do anything until it is plugged into an application. When
a component is installed, it can be married with a configuration. This
combination of the component definition and the configuration create
a *component instance* in that application.

That process involves selecting the component, and then configuring it. The
configuration is provided inside the application. It might be provided
by the human designer, or derived from the application itself. There is
therefore a process to adding a component to an app. This process punctuated
by a series of *events* that the component can handle in its scripts. The
sequence of event's and their purpose is listed below:

- **onSelect** This is invoked when the component is selected, but has
  not yet been loaded. The component has the opportunity here to
  introspect the application, provide information to the designer,
  and dynamically reconfigure itself if necessary for the specific
  application. It could also check if there are conflicting components
  already installed in the app, and cancel the installation at this point.

- **onLoad** This is called when a component is loaded and all of its
  dependencies are present, but the component is not installed yet. This 
  is typically where any initial human configuration is performed. The
  component can provide a view for its configuration here.

- **onConfigure** This is called after the configuration view is
  completed successfully. The component is still not yet installed,
  and this is the final opportunity for the component to modify
  or cancel the installation.
  
- **onCancel** This is called anytime the component installation is
  cancelled before completion. The component can perform any extra 
  cleanup (such as for remote resources) here.

- **onInstall** This is invoked once the component is successfully
  installed.

Note that components do not need to handle any of these events unless
they require custom logic that differs from that provided by J5. J5
also provides a declarative way to define component configuration
views. The events above happen on every component installation
regardless, however.



