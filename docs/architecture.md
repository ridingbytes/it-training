# SENAITE Architecture

SENAITE is built on the application server [ZOPE](http://www.zope.org)
and the CMS [Plone](http://plone.org) using the object oriented database
[ZODB](http://www.zodb.org).


## ZOPE

ZOPE is a web framework which handles basically data persistence, data integrity
and access control. The version SENAITE (actually Plone) requires is ZOPE 2.

ZOPE uses the "Zope Object Database" (ZODB) or even a ORM & SQL/Postgres/Oracle.

### Resources

- https://zope.readthedocs.io/en/latest/
- http://www.zodb.org/en/latest/tutorial.html


## Plone

Plone is an open source Content Management System (CMS) built in Python.

For SENAITE it provides the base for

- Workflow-driven, collaborative management of content
- Industrial Strength Security and Access-Control
- Limitless Extensibility


## Object Orientation

Zope’s object structure is hierarchical, which means that a typical Zope site is
composed of objects that contain other objects (which may contain other objects,
ad infinitum).

For example, the URL "/clients/client-1/contact-1" could be used to access the
Contact object named "contact-1" located in the folder "client-1" which is
located in the folder "clients".

URLs map naturally to objects in the hierarchical Zope environment based on
their names. While Web servers like Apache and Microsoft’s IIS translate URLs
into files and directories on a file system, Zope maps URLs to objects in its
object database.
