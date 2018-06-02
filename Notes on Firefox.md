# Hacking Firefox

The Firefox source code consists of a combination of C/C++ code and JavaScript codes which executes inside the Mozilla framework.

## Pre-reqs

- Multi-threading programming (OS) (mutexes, locks, semaphores)

## NSPR

https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSPR

**Netscape Portable Runtime** (NSPR) is a technology that Mozilla developed to cope with the cross-platform requirements of the Mozilla software. It allows a common API for OS-specific functionalities like threads.
When you are tempted to use a C library function, you should check first whether NSPR offers a cross-platform implementation.
 
## Component Object Model (XCOM) and OOP

https://developer.mozilla.org/en-US/docs/Mozilla/Tech/XPCOM

The source code is divided into components that allow freedom of use between classes inside of the component but can only be used through very well defined APIs that are exposed between components.

**XPCOM** is Mozilla’s own implementation of COM – the component object model. XP is from cross-platform. 

Mozilla provides a technology called XPConnect that enables communication between interpreted JavaScript and compiled C++ at runtime.


## Interfaces

Mozilla uses XPIDL (Interface Definition Language) that is used in the CORBA environment. Interface definition files are automatically translated into C/C++ header files through Mozilla's IDL compiler, `xidpl`.


## Strings in C++

While many application frameworks or class libraries decided to use just a single string class, the Mozilla developers have decided they need something more powerful. They have implemented a hierarchy of several string classes, which allows the dynamic runtime behaviour to be optimized for different situations. Sometimes you just need a fixed size string; sometimes you need a large string that grows over time. Therefore, for example, not only flat strings, but also segmented string types are available.

An additional requirement is that Mozilla has to be fully multi-language. All strings that deal with information shown to a user are therefore using multi-byte Unicode character strings.

The string types are template-based, with the character type as the variable type, to allow the same logic to be used with regular and Unicode strings.

While that approach of having many string classes means a lot of flexibility, the drawback is that learning Mozilla’s string classes is not trivial.


## GUI

Mozilla developed their own Graphical User Interface (GUI) library which is an XML-based description of how the UI should look based on available real estate and system fonts. This library is called **XUL** (eXtensible user-interface language, pronounced 'zool').

## L13n

When defining the UI, there are two kinds of strings. Some strings are known at the time the application is compiled and packaged, like labels for input fields, or the text that appears within the help system. Other text is assembled dynamically at runtime.

Whenever you define text that does not need to be accessed at runtime, you define it in DTD files. You can refer to that text directly in XUL files.

If you need to work with text at runtime, for example if your text contains a placeholder for a user name that needs to be filled at runtime, you define your text in properties files.


https://developer.mozilla.org/en-US/docs/Mozilla
https://developer.mozilla.org/en-US/docs/Mozilla/An_introduction_to_hacking_Mozilla
https://developer.mozilla.org/en-US/docs/Mozilla/Tech/XPCOM
https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSPR



