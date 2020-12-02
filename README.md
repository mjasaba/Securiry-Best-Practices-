# Securiry-Best-Practices-
Security best practices for Python Language

Security best practices for Python Language
When developing software, writing secure code is essential for protecting sensitive data and maintaining correct behaviour of software. Writing secure code can be hard though and even the best developers can’t always be 100% sure of the security of their code. 
Python is a high-level programming language and offers a lot of flexibility and liberty to structure your code base the way you want it which might open up some loopholes if security practices are not being followed.
No matter how small the application and or how good the developers who built it are, there’s always a possibility of a security vulnerability that could be exploited to steal money from users or be the next known Cyber-Incident. The followings are some basic practices every developers should follow in order to reduce the security issues.

01.	Use the latest version of Python
02.	Use a virtual environment
03.	Import packages in correct way
04.	Vigilant on exploited and malicious packages
05.	Maintain up to date open source vulnerabilities in Python packages
06.	String formatting in Python
07.	Handle Python HTTP requests safely
08.	Scan your code

 
01.	Use the latest version of Python

Many companies and developers are still running old versions of Python for their projects and even in production like Python 2.6 or 2.7. These versions are way outdated and won’t receive any more security updates after April 2020.
Python 3 was released date back in 2008 and starting from Jan 1, 2020 the Python Foundation announced that Python 2 will stop receiving security updates or support from the community.
If you are still using old versions of Python below Python 3, then you should start considering how to migrate your codebase to Python 3. Start using Python 3 for your new projects or you leave yourself open to security vulnerabilities.
Furthermore, the transition from one version to the next is not easy. Many functions and libraries are incompatible and a lot of tools have changed between the versions. You should begin this process ASAP.

    python –version
 

02.	Use a virtual environment

When building any Python projects, it’s always advisable to use a virtual environment as it helps to prevent conflict in Python modules and as well as have the same modules both on local and production environments. 
Using a virtual environment prevents having malicious Python dependencies in your projects. If you have malicious packages in your Python environments, using a virtual environment will prevent having the same packages in your Python codebase since it’s isolated.
To create a virtual environment you can either use Virtualenv or Pipenv which help create isolated virtual environments. Pipenv helps to manage, have a predictable and up-to-date environment. 
With Pipenv, you can manage your installations, virtual environments, look through your dependency tree, and scan your dependencies for known vulnerabilities.
You can set up Virtualenv:

    pip install virtualenv
    virtualenv -p /path/to/python <env_name>
 

03.	 Import packages in correct way

When working with external or internal Python modules, you should always ensure you are importing them the right way and using the right paths. We have two types of import paths in Python and they are absolute, relative.
Absolute imports specifies the path of the resource to be imported using its full path from the project’s root folder while relative import specifies the resource to be imported relative to the current location of the project where the import statement is.
 
    /* Absolute Import */
    from package1 import module1
    from package1.module2 import function1
    /* Relative Import */

    from .some_module import some_class
    from ..some_package import some_function

Now there are two types of relative imports: implicit and explicit. 
Implicit imports does not specify the resource path relative to the current module while Explicit imports specify the exact path of the module you want to import relative to the current module. 
Implicit import has been disapproved and removed from Python 3, because if the module specified is found in the system path, it will be imported and that could be very dangerous. 
Since it’s possible for a malicious module with the same name to be in a popular open source library and find its way to the system path. If the malicious module is found before the real module it will be imported and could be used to exploit applications that has it in their dependency tree.
To prevent this, ensure you use either absolute import or explicit relative imports as it guarantees you import the real and intended module.

    from safe_module import package, function, class

Or

    from  ..relative_module import package, function, class

If you are still using Python 2, ensure you remove the use of implicit relative imports as this as been removed in Python 3.
It is easy to install Python packages. Typically developers use the standard package installer for Python (pip), although Pipenv as discussed above is a great alternative. Regardless of whether you use pip or Pipenv, it is important to understand how packages are added to PyPI.
PyPI has a procedure for reporting security concerns. If someone reports a malicious package or a problem within PyPI it is addressed, but packages added to PyPI do not undergo review—this would be an unrealistic expectation of the volunteers who maintain PyPI.
Therefore it is wise to assume that there are malicious packages within PyPI and behave accordingly. Reasonable steps include doing a bit of research on the package you want to install and ensuring that you carefully spell out the package name (a package named for a common misspelling of a popular package could execute malicious code).
 

04.	Vigilant on exploited and malicious packages

Packages can be very helpful and save you time as you don’t have to re-invent the wheel. Packages can be easily installed through the Pip package.  They offer various benefits like saving time, making your codebase compact and smaller, easier application design and better performance.
Most Python Packages are published to PyPI which serves as a code repository for Python Packages and does not go through any form of security review or check.
This means that anyone out there with malicious thought can easily build and publish a package to PyPI with a malicious code or sometimes publish a package with a similar name to a popular package and imitate the package features.
Double-check each Python packages you are installing and importing to prevent having exploited packages in your code. Also, you can use security tools to scan your Python dependencies to screen out exploited packages.
 

05.	Maintain up to date open source vulnerabilities in Python packages

One of the simplest ways to prevent and get rid of open source vulnerabilities is having the latest updates of the open source that already fixed the vulnerability. Open source is a good way for developers and communities with one interest in mind to build, contribute and publish software openly for better use of the community. 
However, sometimes there’s a possibility that a security loophole might pop up that could be very dangerous as any software or application using the project may be open up for attacks. 
For this reason, open source vulnerabilities are always published as soon as they are discovered and a fix and prevention method are usually rolled out in the next version usually security patch release which should end up in the next major release. 
The sooner you have the latest update of the open source package, the better you are secured. Always ensure you are updated with vulnerabilities of the open source package you are using, so as to know when to upgrade to the next version.
 

06.	String formatting in Python

Python has one of the most powerful and flexible methods to format strings and if you are not careful enough while using, you might end up opening up a security vulnerability in your code.
Python3 introduced f-strings  and str.format() as a flexible way to format strings and its actually very interesting. However, this opens up a way for data exploit when dealing with user inputs. 
If the application built on Python allows users control of the format string, they can be misused to leak sensitive data. For instance, let’s take a look at the exploit code below:


    CONFIG = {
        “API_KEY”: “secret_key”
    }
    class User:
        name = “”
        email = “”
        def __init__(self, name, email):
            self.name = name
            self.email = email
        def __str__(self):
            return self.name
    name = “Toby”
    email = “oyetoketoby80@gmail.com”
    user = User(name, email)
    print(f”{user.__init__.__globals__[‘CONFIG’][‘API_KEY’]}”)
    /* secret_key */

With this, sensitive global data from a CONFIG dictionary can be accessed via the argument.
However, Python has a built-in string module that can be used to fix and prevent this. Using the Template class from the string module:

    from string import Template
    name_template = Template("Hello, my name is $name.")
    greeting = name_template.substitute(name="Tobi")
    /* Hello, my name is Toby */

The string module is good for handling user inputs and generated data.
Python offers four flexible string formatting approaches. However, flexible formatting syntax like the f-strings can be vulnerable to exploits. This is why developers should pay attention when formatting user-generated strings.
The Python built-in string module can help you overcome this problem. Built-in string modules are based on the template class that enables you to create template strings. For instance, the code below asks users to enter their name and then displays the name:

    from string import Template
    name_template = Template(“Hello, my name is $name.”)
    greeting = name_template.substitute(name=”James”)

The output is a string of “Hello, my name is James”. This string module is not as flexible as f-string. This is why string modules are a good choice for handling user inputs.
Another quick note on string formatting: Be extra careful with raw SQL. Make your queries with object-relational mapping (ORM) if at all possible.
 

07.	Handle Python HTTP requests safely

When building Python project that requires sending HTTP requests, it’s always advisable to do it safely and know the library you are using handles security to prevent security issues.
When using HTTP requests library like Requests, you should not pin the versions down in your requirements.txt has that might install outdated and vulnerable version of the module.  
For instance, Requests uses Certifi for handling SSL verification, ensure you are sending it to a non-exploited site. By default, Requests handles the SSL certificate verification and can be disabled if you trust the source. 

    url = “http://trusted_url”
    requests.get(url, safe=False) 

This ensures you are not sending requests to an exploited source that could send back exploited code in the Response headers or body. 
So to prevent this ensure you are using the latest version of your HTTP requests library, confirm if the library is handling the SSL verification of the source you sent requests to, if you are using standard library urllib,  you should follow best practices to prevent request smuggling.
 

08.	Scan your code

A simple way to find security vulnerabilities within your Python code is to run a scan with Bandit.
Bandit is an open source project that is available through the Python Packaging Index (PyPI). Bandit scans each .py file and builds a corresponding abstract syntax tree (AST). Bandit then runs a number of plugins against the AST to find common software security problems. For example, one plugin can detect whether you are using Flask (a micro-framework for Python) with the debug setting equal to True.
Bandit works either as a local tool to be used as you develop, or as part of your CI/CD (continuous integration/ continuous delivery) pipeline. You can create a YAML configuration file to control how Bandit behaves in these different scenarios. In this file you can also indicate a list of tests to skip. This functionality should be used with caution.
There is no guarantee that Bandit will catch all security problems—there are a finite number of plugins that it runs, and you could potentially have an issue in your code that doesn’t register against any of the available plugins. However, it is easy to use and an excellent screen for common issues.
Key features include:
•	Test plugins—supports various tests that help you detect security issues in Python code. You can create these tests as plugins to extend the functionality of Bandit.
•	Blacklist plugins—you can blacklist imports and function calls. This functionality is an integrated part of one of the Bandit tests. You can filter this test according to normal plugin filtering rules.
•	Report formatters—supports various formatters that can output Python security issues. You can create these formatters as plugins and to extend the functionality of Bandit.
Also there are some other security tools and scanners such Pyntch, Spaghetti, and Requires are available for secure development of Python software.
 

References

    https://www.securecoding.com/blog/python-security-practices-you-should-maintain/
    https://dev.to/leahfb/python-security-top-5-best-practices-2of3
    https://snyk.io/blog/python-security-best-practices-cheat-sheet/
    https://codehandbook.org/secure-coding-in-python/
    https://medium.com/@felsen88/python-secure-coding-guidelines-73c7ce1db86c
    https://www.python.org/success-stories/securing-python-runtimes/
