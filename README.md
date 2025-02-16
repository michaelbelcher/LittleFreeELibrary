# LittleFreeELibrary


A quick How-To guide to getting a e-library up and running on a Raspberry Pi

## Background Talk

This idea started a few years ago with the publication of Hydroponic Trash's guide to an off-grid internet. They followed it up a little while later with thier guide to a little banned book library.

That idea, a focused way distribute information that the State doesn't want distributed, is immedietly compelling. 

And HydroponicTrash's ideas *work*. but I wanted to find an easier, non-technical way to do it. 

At that I failed, but I do think I came up with a *less* techy way to do it. 

My method is based **HEAVILY** on the internet-in-a-box concept, with everything not focuesd on delivaring and managing books turned off to keep things streamlined and replicatable. 

Once this is up an running on one Pi you can clone the microSD card and pop it into any Pi to replicate the library. With the low cost of Pi Zero 2 Ws (or Pis that you get second hand) you can set up multiple copies of the library at a fairly low cost. 

Since it doesn't matter what Pi you start with it is faster to set this up on a higher-specced pi (like a pi 4 or pi 5) to get started. 

## Getting Started

First off, you need to make some decisions. 

- What sort of Pi are you going to run this on? 

This eLibrary is designed to run on the older Raspberry Pi 3 and 3 B+, and the lower-speced Pi Zero W and Pi Zero 2 W models, but you can use any newer Pi as well. 

- How big of a library do you want to host?

This answer comes down to file formats. Books will generally be in two formats, epubs and PDFs. 
epub files are usually pretty small, and you can fit hundreds of epub books in a couple of GBs of space. 
PDFs can also be small, but can quickly balloon in size if there a lots of scanned images. 
Be aware that the IIAB install will take between 10 and 12 GB, so expect to use at least a 32GB microSD card for your libaray. 

### Step one:
 install rapsberry pi os on the fastest Pi you've got (this will make the install much faster).
 - use Raspberry Pi's installer on a Mac or PC to get the microSD card ready with the latest PiOS (64bit). 
 - make sure the Pi is connected to the internet through ethernet, don't set up WiFi at this time. 
 - when the install is done make sure the OS is fully up to date. 

    // ToDo: need to test on Debian install


### Install internet in a box

Use the folling curl command to start the install process. 
You can check the IIAB download website for the full instructions if you want more details. 
https://download.iiab.io/


    curl iiab.io/install.txt | bash

During the install be sure to select the Small option and make sure the following options are set to False, False

    kolibri, maps, matomo

and make sure these are set to true, true.

    calibre-web, calibre, Captive Portal, kiwix, awstats, 

- Why keep Kiwix? 
    - You have to keep Kiwix installed and enabled to use the iiab admin console, which you need to do some stuff later


set the iiab)home_url to /books/ to make sure it goes to calibre-web
And make sure you set stuff like the SSID to something you know, set the p/w, set the correct page "/books/" to load after captive portal. change the pi_swap_file_size size to 512 if planning to use pi zero/2 W. 
maybe change the hostname and domain to something clever (since it won't ever be on the internet anyway)
Set captiveportal_splsh_page to /books/ to make sure it goes to calibre-web

Let it run until it's done installing.

## Modifications to the systems
### Loading the library
    if you already have an existing library of books you're wanting to host, put them on a USB drive and put that drive into your Pi.
    #### if you already have them processed through Calibre then drop them in the /library/calibre directory. 
    #### if they need to be processed first then open up localhost:8080 on your Pi web browser and login as admin. (Default Admin pw is changeme, you can log into Calibre and change it here. ) Select the Calibre library button and you should see a copy of Franz Kakfa's Metamorphosis. click the + icon in the top banner and follow the insturctions on loading new books. 

### Changes to the IIAB Admin panel
Admin user is iiab-admin, and password is the one set up on initial installation. 
Go to Content Menus, then content item list. Drag over every option in Menu Items in Current Menu except "insert a usb flash drive here" and "calibre-web" into Available Menu Item Definitions. 
Then click Save Menu, *then* click Update Home Menu on the left side. This is for any user that makes it past the /books/ url onto the proper IIAB home screen. 
### Changes to the Calibre admin
    Go to http://banned.books:8080 to get to the Calibre app. Default Admin pw is changeme, you can log into Calibre and change it here. 
    This calibre app is where you can upload new books into the libaray. 

### Changes to the Calibre-Web panel
Click the Setting icon_Admin (it will look like a little speedomator in a car)
    [check what the pw/username is on default]
Change the database location: it's currently set to /library/calibre-web, and we need to change it to /library/calibre. click on the folder icon next to the .. at the top of the popup. this takes you up a level in the pi directory structure. 
then click on the folder icon labeled calibre. there should be a folder named Franz Kafka and a metadata.db file in this directory. Click the select button to change the location of the calibre-web database. Hit Save and then OK.

Click on the same Speedomator Admin button and click basic configuration. Scroll down to security settings and uncheck user password policy. This lets you set weak passwords for the admin user, if you want to chagne the username/pw. 
Click on the same Speedomator Admin button and click UI configuration. under View Configuration I change the name from Internet in a Box to Read Banned Books, change number of random books to 2, and change the theme to caliBlur! Dark Theme. If you plan on setting up individual users other than the Admin and Guest think about if you want them to be able to edit public bookshelves and to delete books. 
Once you hit Save on the theme change the layout will look vastly different, but all the options are still there, you may need to look around for them. 



## Code Changes
### Splash screen changes
### Home page changes



- Changes in the box Admin
    - change name and theme for calibre-web to "elibrary" or something similar
    - change the calibre-web directory to use the calibre directory of books
- Changes in Calibre 
    - move all the books into the calibre library
    - 



At this point it should be ready to be run, but there are a few things to make this setup work much better.




Sites of note:

    https://download.iiab.io/
    https://wiki.iiab.io/go/FAQ
    https://anarchosolarpunk.substack.com/p/offgridinternet
    https://anarchosolarpunk.substack.com/p/bannedbooklibrary


