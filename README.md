# MenuSystem-1.3

## MenuSystem

	Author Daniel Mikusa dan at mikusa dot com
	Date 2006-04-26
	License LGPL

## WHAT YOU GET

In case you are wondering "What does this package do?", wonder no more.  It creates
simple menuing systems.  Here's an example of what a menu might look like.

	Menu Name
		1.) Do Something
		2.) Do Something Else
		3.) Do Another Something Else
	Enter your choice .>

As you can see basic text menus aren't the prettiest things in the world, but they are
easy and effective.  Here is what is included with this module.

	Menu & Choice classes
	MenuGenie Interface
	XMLMenuGenie Implementation

The Menu & Choice classes represent menu and choice objects in our systems.  Each menu 
contains a name, one or more choices, and a prompt.  Each choice contains a selector,
name, value, and a function to execute when the menu has been chosen.

The MenuGenie interface is a way that you can create you own menu storage classes.  For
instance you may want to save menus a MySQL database (or any other database), XML, or
anything else might like.

Done for you already is XML storage.  The XMLMenuGenie allows you to save and load menu
systems from XML.  This will alow you to keep you display code seperate from you controller
code.


PURE PYTHON

Menu systems can simply be built by writing a Python script to create the object that
you need.  Here's an example.

menu.py
	# Selector Functions
	def print_ok(val):
		print 'OK: %s' % val
		
	def print_bad(val):
		print 'BAD: %s' % val
	
	def done(val):
		return False
		
	def submenu_handler(val):
		print 'Going to submenu'

	# Create Sub Menu
	lst = []
	lst.append(MenuSystem.Choice(selector=1, value=1, handler=print_ok, description='Sub Menu Do Noting Some'))
	lst.append(MenuSystem.Choice(selector=2, value=2, handler=print_ok, description='Sub Menu Do Noting More'))
	lst.append(MenuSystem.Choice(selector=3, value=3, handler=print_bad, description='Sub Menu Do Noting Alot'))
	lst.append(MenuSystem.Choice(selector=4, value=4, handler=done, description='Return to Main Menu'))
	sub = MenuSystem.Menu(title='Sub Menu', choice_list=lst, prompt='Select Choice.> ')
	
	# Create Main Menu
	lst = []
	lst.append(MenuSystem.Choice(selector=1, value=1, handler=print_ok, description='Do Noting Some'))
	lst.append(MenuSystem.Choice(selector=2, value=2, handler=None, description='Do Noting More'))
	lst.append(MenuSystem.Choice(selector=3, value=3, handler=print_bad, description='Do Noting Alot'))
	lst.append(MenuSystem.Choice(selector=4, value=4, handler=None, description='Sub Menu', subMenu=sub))
	lst.append(MenuSystem.Choice(selector=5, value=5, handler=done, description='Exit'))
	
	# Creat Menu & Begin Execution
	head = MenuSystem.Menu(title='Main Menu', choice_list=lst, prompt='Select Choice.> ')

After you create your menu system all you need to do is call the waitForInput member function
of you top level menu object, and it will handle the rest.

	head.waitForInput()


XML

You can create you menu system from an XML file.  Each menu and choice are represented by XML
tags.  Here's an example.

file.xml
	<?xml version="1.0" ?>
	<menu prompt="Select Choice.&gt; " title="Main Menu">
		<choice description="Do Noting Some" handler="print_ok" selector="1" value="1"/>
		<choice description="Do Noting More" handler="None" selector="2" value="2"/>
		<choice description="Do Noting Alot" handler="print_bad" selector="3" value="3"/>
		<choice description="Sub Menu" handler="None" selector="4" value="4">
			<menu prompt="Select Choice.&gt; " title="Sub Menu">
				<choice description="Sub Menu Do Noting Some" handler="print_ok" selector="1" value="1"/>
				<choice description="Sub Menu Do Noting More" handler="print_ok" selector="2" value="2"/>
				<choice description="Sub Menu Do Noting Alot" handler="print_bad" selector="3" value="3"/>
				<choice description="Return to Main Menu" handler="done" selector="4" value="4"/>
			</menu>
		</choice>
		<choice description="Exit" handler="done" selector="5" value="5"/>
	</menu>

Design your menu system's layout in the XML file, and then write the choice object's handler
functions.

handlers.py
	def print_ok(val):
		print 'OK: %s' % val
		
	def print_bad(val):
		print 'BAD: %s' % val
	
	def done(val):
		return False
		
	def submenu_handler(val):
		print 'Going to submenu'

The create an XMLMenuGenie object and call it's load function.  The load function will
return the top level menu object representing your menu system.

	xml = MenuSystem.XMLMenuGenie('file.xml', 'choice_handlers')
	head = xml.load()

Then just like the Pure Python method, call the top level menu object's waitForInput
member function to start the menu system.

	head.waitForInput()

The second part of the XMLMenuGenie class is that you can save menu objects to XML files.  So
if you create a sweet menu in Python and want to save it to XML, just create you XMLMenuGenie
object, and call it's save function.

	xml = MenuSystem.XMLMenuGenie('file.xml', 'choice_handlers')
	xml.save(head)

## MORE INFORMATION

If you need more information I suggest you take a look at the code.  There are loads of
comments and, of course, there's the code.

Conceptually the module isn't very tough.  The Menu class contains a list of choices.
Each choice is passed a handler.  The handler is called when a choice is selected.  To
give your menu system depth, you can give a choice object a submenus.  The submenu is 
just like any other menu and contains it's own choices which can also be submenus.  This
can continue on to any depth that you like.

The MenuGenie class is a generic interface that can be sub-classed to create
any number of XXXMenuGenie sub classes.  As long as you implement the MenuGenie interface
correctly, all of these sub classes will be interchangable.  The XMLMenuGenie sub class
has already been created, if you are trying to make your own sub class I suggest you
use that as an example.

Questions and comments are always welcome, shoot me an email.

LICENSE

Copyright (C) 2006  Daniel Mikusa

This library is free software; you can redistribute it and/or
modify it under the terms of the GNU Lesser General Public
License as published by the Free Software Foundation; either
version 2.1 of the License, or (at your option) any later version.

This library is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
Lesser General Public License for more details.

You should have received a copy of the GNU Lesser General Public
License along with this library; if not, write to the Free Software
Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA  02110-1301  USA
