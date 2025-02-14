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

    kolibri, maps

and make sure these are set to true, true.

    calibre-web, calibre, Captive Portal

- Why keep Kiwix? 
    - you can turn this off if you really want to.


And make sure you set stuff like the SSID to something you know, set the p/w, set the correct page "/books/" to load after captive portal.

Let it run until it's done installing.

## Modifications to the systems

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


