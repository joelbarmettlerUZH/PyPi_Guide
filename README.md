# Submit your package to PyPi - A complete guide!

The PyPi package index is one of the properties that makes python so powerfull: With just a simple command, you get access to thousands of cool libraries, ready for you to use. In this short introduction, I am going to explain how you can upload upload your own code to PyPi and let others profit from your inventions. On the following pages, I am going to show you the following steps:

  - Make your code publish-ready
  - Create a python package
  - Create the files PyPi needs
  - Create a PyPi account
  - Upload your package to github.com
  - Uplaod your package to PyPi
  - Install your own package using pip
  - Change your package

## But what is PyPi?

Well, according to their [Website](https://pypi.org/):
> The Python Package Index (PyPI) is a repository of software for the Python programming language.
>PyPI helps you find and install software developed and shared by the Python community. Learn about installing packages.
>Package authors use PyPI to distribute their software. Learn how to package your Python code for PyPI.

## How to use PyPi

I am sure most of you have already installed PyPi *(pip)* and worked with it. If you have not, you definitely should! It's as simple as downloading [this file](https://bootstrap.pypa.io/get-pip.py). Now, open your command promt and navigate wie "cd" into the directory where the downloaded file is located. Then, run the command "python get-pip.py".

```sh
cd "C://PATH//TO//YOUR//DOWNLOADED//FILE"
python get-pip.py
```

To use pip, type "pip insall *packagename*" into the console. When you want to install *scrapeasy*, you type the following command:
```sh
pip install scrapeasy
```

Did you get an error that says **'pip' is not recognized as an internal or external command,
operable program or batch file.**? Don't panic, we got this. Your pip script is not in the windows environment variables, so you can not access it from everywhere on your system. Let's fix this with the following command:

```sh
setx PATH "%PATH%;C://WHERE//PYTHON//IS//INSTALLED//Scripts"
```

Or, as an example

```sh
setx PATH "%PATH%;C:\Python36\Scripts"
```

It should work now. 

# Make your code publish-ready

Let's prepare your code for the uploading. First, you should remove all "print" statements from your code. It's annoying when you work with a library and your command promt is flooded with print messages that are not yours - therefore remove all of them. If you want to inform the user about certain activities, use [logging](https://docs.python.org/3/library/logging.html).

Also make sure to not include code that exists outside of a class, otherwise this code will run every time somebody imports your library. If you want to include example code into the classes (which is legit), wrap it into the "\_\_main\_\_" method.

```python
if __name__ == "__main__":
    your example code goes here
``` 

# Create a python package

To create a package, create a folder that is named exactly how you want your package to be named. Place all the classes that you want to ship into this folder. 

Now, create a file called \_\_init.py\_\_ (again two underscores). Open the file with your text editor of choice. In this file, you write nothing but import statements that have the following schema: 

```python
from packagename.Filename import Classname
```

The \_\_init.py\_\_ file is used to mark which classes you want the user to access through the package interface. 
Let's make an example. Assume you want to upload a library named "MyLib" to PyPi. First, create a Folder called "MyLib" and place your classes inside of it. 

> MyLib  
> -Class1.py  
> -Class2.py  

Then, remove all print-statements and all the code that does not sit inside of a class. 

Finally, create the \_\_init.py\_\_-file and import all the methods that the user should access. 

> MyLib  
> -\_\_init.py\_\_  
> -File1.py  
> -File1.py  

```python
# Inside of __init__.py
from MyLib.File1 import ClassA, ClassB, ClassC
from MyLib.File2 import ClassX, ClassY, ClassZ
```


# Create a PyPi account
You can register yourself for a PyPi account [here](https://pypi.org/account/register/). Remember your username (not the Name, not the E-Mail Address) and your password, you will need it later for the upload process. 

# Upload your package to github.com
Create a github repo including all the files we are going to create in a second, as well as your package folder. Name the repo exactly as the package. You find an example of such a repo [here](https://github.com/joelbarmettlerUZH/Scrapeasy)

# Create the files PyPi needs
PyPi needs three files in order to work:
- setup.py
- setup.cfg
- LICENSE.txt
- README.md (optinal but highly recommended)

Place all these files outside of your package folder:
> MyLib  
> setup.py  
> setup.cfg  
> LICENSE.txt  
> README.md  

Let's go through this files one-by-one  

## setup.py
The setup.py file contains information about your package that pip needs, like its name, a description, the current version etc. Copy and Paste the following Code and replace the Strings with your matching content:

```python
from distutils.core import setup
setup(
  name = 'YOURPACKAGENAME',         # How you named your package folder (MyLib)
  packages = ['YOURPACKAGENAME'],   # Chose the same as "name"
  version = '0.1',      # Start with a small number and increase it with every change you make
  license='MIT',        # Chose a license from here: https://help.github.com/articles/licensing-a-repository
  description = 'TYPE YOUR DESCRIPTION HERE',   # Give a short description about your library
  author = 'YOUR NAME',                   # Type in your name
  author_email = 'your.email@domain.com',      # Type in your E-Mail
  url = 'https://github.com/user/reponame',   # Provide either the link to your github or to your website
  download_url = 'https://github.com/user/reponame/archive/v_01.tar.gz',    # I explain this later on
  keywords = ['SOME', 'MEANINGFULL', 'KEYWORDS'],   # Keywords that define your package best
  install_requires=[            # I get to this in a second
          'validators',
          'beautifulsoup4',
      ],
  classifiers=[
    'Development Status :: 3 - Alpha',      # Chose either "3 - Alpha", "4 - Beta" or "5 - Production/Stable" as the current state of your package

    'Intended Audience :: Developers',      # Define that your audience are developers
    'Topic :: Software Development :: Build Tools',

    'License :: OSI Approved :: MIT License',   # Again, pick a license

    'Programming Language :: Python :: 3',      #Specify which pyhton versions that you want to support
    'Programming Language :: Python :: 3.4',
    'Programming Language :: Python :: 3.5',
    'Programming Language :: Python :: 3.6',
  ],
)
```

Let me explain the download_url and the install_requires a bit further. 

### download_url
You have previously uploaded your project to your github repository. Now, we create a new release version of your project on github. This release will then be downloaded by anyone that runs the "pip install YourPackage" command.

First, go to github.com and navigate to your repository. 
Next, Click on the tab "releases" and then on "Create a new release". Now, define a Tag verion (it is best to use the same number as you used in your setup.py version-field: *v_01*. Add a release title and a description (not that important), then click on "publish release". 
Now you see a new release and under **Assets**, there is a link to **Source Code (tar.gz)**. Right-click on this link and chose *Copy Link Address*. Paste this link-address into the *download_url* field in the setup.py file. Every time you want to update your package later on, upload a new version to github, create a new release as we just discussed, specify a new release tag and copy-paste the link to Source into the setup.py file (do not forget to also increment the version number). 

### Install_requires
Here, you define all the dependencies your package has - all the pip packages that you are importing. Simply speaking: Go through all your Classes and watch what you have imported. Every package that you have imported and downloaded via pip (most propably all, except the standard libraries) must be listed here as an install_requires by adding the package name. 

Let me give you an example
```python
# In the Class1 File
import numpy as np
from scrapeasy import Webpage
import time
```

The packages "numpy" and "scrapeasy" were downloaded via pip, so you need to add "numpy" and "scrapeasy" in your dependencies. But how do you know whether a package is a standard library or downloaded via pip? Well, simply open your cmd and type *pip install packagename*. If you get **Requirement already satisfied: **, you are good to go. If you get an error stating "Could not find a version that satisfies the requirement**, you know that this package is either not installed via pip or a standard library.

The resulting *install_requres* field would look like this:
```python
  install_requires=[
          'numpy',
          'scrapeasy',
      ],
```

## setup.cfg
This is a short one. Create a new file called "setup.cfg". If you have a description file (and you definitely should!), you can specify it here:
```python
# Inside of setup.cfg
[metadata]
description-file = README.md
```

## LICENSE.txt
Use this file to define all license details. Most propably, just copy-paste the license text from [this](https://opensource.org/licenses/) website into the LICENSE.txt file. I always use the MIT license for my projects, but feel free to use your own. 

```sh
MIT License

Copyright (c) 2018 YOUR NAME

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

```

## README.md (Optional)
Create a markdown file with [this](https://dillinger.io/) beautiful website, then download it and place it into your directory. 

# Upload your package to PyPi
Now, the final step has come: uploading your project to PyPi. 
First, open the command prompt and navigate into your the folder where you have all your files and your package located:
```sh
cd "C://PATH//TO//YOUR//FOLDER"
```

Now, we create a source distribution with the following command:
```sh
python setup.py sdist
```

You might get a warning stating "Unknown distribution option: 'install_requires'. Just ignore it. 

We will need *twine* for the upload process, so first install twine via pip:
```sh 
pip install twine
```

Then, run the following command:
```sh 
twine upload dist/*
```

You will be asked to provide your username and password. Provide the credentials you used to register to PyPi earlier. 

**Congratulations, your package is now uploaded!**
Visit *https://pypi.org/project/YOURPACKAGENAME/* to see your package online!

# Install your own package using pip
Okay, now let's test this out. Open your console and type the following command:
```sh 
pip install YOURPACKAGENAME
```

It works! Now open the python SHELL/IDLE and import your package. 

# Change your package
If you maintain your package well, you will need to change the source code form time to time. This is easy. Simply upload your new code to github, create a new release, then adapt the setup.py file (new download_url - according to your new release tag, new version), then run the twin command again (navigate to your folder first!)
```sh 
twine upload dist/*
```
Finally, update your package via pip to see whether your changes worked:

```sh 
pip install YOURPACKAGE --upgrade
```

License
----

MIT License

Copyright (c) 2018 Joel Barmettler

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.


By the way, I am an [AI Engineer from Zurich](https://joelbarmettler.xyz/) and do [AI research](https://joelbarmettler.xyz/research/), [AI Keynote Speaker](https://joelbarmettler.xyz/auftritte/) and [AI Webinars](https://joelbarmettler.xyz/auftritte/webinar-2024-rewind-2025-ausblick/) in Zurich, Switzerland! I also have an [AI Podcast in swiss german](https://joelbarmettler.xyz/podcast/)!
