# LittleFreeELibrary


A quick How-To guide to getting an off-grid e-library up and running on a Raspberry Pi. 
This will build out a Pi to:
- host and serve ebooks from Calibre, 
- have a pleasant UX front end through Calibre-Web,
- broadcast a wifi network from the Pi that lets users access the front end to read books.


## Background Talk

This idea started a few years ago with the publication of [HydroponicTrash](https://kolektiva.social/@hydroponictrash#.)'s guide to an ["off-grid internet"](https://anarchosolarpunk.substack.com/p/offgridinternet). They followed it up a little while later with their [guide to a little banned book library](https://anarchosolarpunk.substack.com/p/bannedbooklibrary).

That idea, a focused way distribute information that the State doesn't want distributed, is immediately compelling. 

And HydroponicTrash's ideas *work*. but I wanted to find an easier, non-technical way to do it. 

I don't think I found a true "non-technical", plug and play method, but I do think I came up with a *less* techie way to do it. 

My method is based **HEAVILY** on [internet-in-a-box](https://internet-in-a-box.org/), which is designed to build local "internet" services for schools, hospitals and other places without consistent and reliable internet access. 

The goal with using IIAB is to get the e-library backend services as turnkey as possible to limit the coding needed to get it set up. With everything not focused on delivering and managing books turned off from the IIAB service it should fit on fairly small microSD cards. 

Once this is up an running on one Pi you can clone the microSD card and pop it into any Pi to replicate the library. With the low cost of Pi Zero 2 Ws (or Pis that you get second hand) you can set up multiple copies of the library at a fairly low cost. 

Since it doesn't matter what Pi you start with it is faster to set this up on a higher-specced pi (like a pi 4 or 5) to get started. 

## Getting Started

### What you need for setup
* microSD card (at least 32gb)
* Raspberry Pi (initial setup should be done on the fastest pi you have access to.)
* keyboard/mouse/monitor for setup
* Ethernet connection for initial IIAB setup
* Way to install Raspberry Pi OS onto an SD card (this can be another computer, your current Pi that is already set up, etc)
* ebooks (preferably epub, this is a BYOBooks project)


First off, you need to make some decisions. 

- What sort of Pi are you going to run this on? 

    - This e-Library is designed to run on the older Raspberry Pi 3 and 3 B+, and the lower-speced Pi Zero W and Pi Zero 2 W models, but you can use any newer Pi as well.

- How big of a library do you want to host?

    - This answer comes down to file formats. Books will generally be in two formats, epubs and PDFs. Epub files are usually pretty small, and you can fit hundreds of epub books in a couple of GBs of space. PDFs can also be small, but can quickly balloon in size if there a lots of scanned images. Be aware that the IIAB install will take between 10 and 12 GB, so use at least a 32GB microSD card for your library.

### Step one:
 Install the latest version of the Raspberry Pi OS on the fastest Pi you've got (this will make the install much faster).
1. Use the Raspberry Pi Imager on a Mac or PC to get the microSD card ready with the latest PiOS (64bit). 
2.  Do not set up wifi on the new installation. Trying to install IIAB over wifi can cause issues, so it's safer to plug the Pi into an ethernet cable during this setup. 
3. When the Raspberry Pi Imager is finished take the microSD card and put it in the Pi and plug it into start the install process. 
4. When the install is complete run `sudo apt-get update` and then `sudo apt-get upgrade` to make sure everything is up to date. This will keep you from potentially having to reboot as often during the IIAB install. 

    `// ToDo: need to test on Debian install`


### Install internet-in-a-box

You can check the [IIAB download website](https://download.iiab.io/) for the full instructions. You can also reference [the great FAQ page](https://wiki.iiab.io/go/FAQ) for more details on individual services. 

Use the following curl command to start the install process. 

`curl iiab.io/install.txt | bash`

During the install be sure to select the Small option. You'll be asked to run `sudo nano /etc/iiab/local_vars.yml` to set your preferences for the install.

 Make sure the following options are set to `_install: False` and `_enabled: False`.

    kolibri
    maps 
    matomo

and make sure these are set to `_install: True` and `_enabled: True`.

    calibre-web, 
    calibre, 
    Captive Portal, 
    kiwix, 
    awstats

> * Why keep Kiwix?  - You have to keep Kiwix installed and enabled to use the iiab admin console, which you need to do some stuff later


Set the `iiab/home_url` option to `/books/`. This is the default url for the `calibre-web` service and is to make sure users are sent to the "library" on joining the wifi network. 

Change the ``pi_swap_file_size`` setting to 512 if you are planning to use a Pi Zero W or Pi Zero 2 W. 

Change the ``captiveportal_splsh_page`` to `/books/` to make sure users are sent to the "library" after clicking through the splash screen.

#### WiFi setup
Since this server isn't being routed through the actual internet, you can change the hostname and domain to something clever. For this setup I'm going to set them to `iiab_hostname: ebook` and `iiab_domain: library`. 
This will make the URL of the service `ebook.library/books/`.

Change the SSID name to be more appropriate for the service you're building. I'm making it explicit that this is free library.

`host_ssid: LittleFreeE-Library`

hostapd is what IIAB uses to create password-protected wifi networks. Since we want this to be an open library we are going to leave `hostapd_secure: False`. But we are going to take this time to change the `hostapd_password:` to something simple that you can remember, because you don't want to leave anything, not even a service that you don't plan on using, with a default password. 

After you've made these changes hit `ctrl+s` and then `ctrl+x` to save and close nano. Click through the rest of the instructions to finish the install. 

## Modifications to the systems

Once the IIAB install is complete and the system is fully booted up you can access the `LittleFreeE-Library` wifi network from any device. If everything was set up correctly you should be sent to the `ebook.library/books/` page, although it will be an empty library. 

### Changes to the Calibre-Web panel

Click the Settings icon (it will look like a little speedometer in a car) and login as `Admin` with pw `changeme`.

#### Change the database location
The location of the calibre-web library is set to `/library/calibre-web`, and we need to change it to `/library/calibre`. 
From the Admin setting page select Database Configuration. 
Click in the text box and change it to `/library/calibre`. 

When you return to the /books/ page you'll find a copy of Franz Kafka's The Metamorphosis, which is included with Calibre. 

#### Change the default password

Click on the same Speedometer Admin button and select Basic Configuration. 

Scroll down to security settings and uncheck user password policy. This lets you set weak passwords for the admin user, if you want to change the username/pw. 

Go back to the setting screen and click Admin under the Users list. Change the Admin password here. 

#### Change the look of Calibre-Web

Click on the Speedometer Admin button and select UI Configuration. 
Under View Configuration change the name from `Internet in a Box` to something more appropriate, like `E-Book Library`.

I also change number of random books to 2, and change the theme to `caliBlur! Dark Theme`, but these are personal preferences. 

If you plan on setting up individual users other than the Admin and Guest think about if you want them to be able to edit public bookshelves and to delete books. 

Once you hit Save on the theme change the layout will look vastly different, but all the options are still there, you may need to look around for them. 


### Loading the library
    
If you already have an existing collection of books you're wanting to host, put them on a USB drive and put that drive into your Pi.

>If you're comfortable with Calibre and already have a Calibre library, you can copy that library over to the `/library/calibre` directory and be done. The directory may have permissions different from your logged-in Pi users, so you may need to use the command line MOVE command `sudo mv /path/fromUSBdrive/* /library/calibre`.

If you have a loose assortment of epubs and PDFs then the easiest method will be to load them directly into Calibre on the Pi.

Open the Pi web browser (the globe in the top left corner) and go to `ebook.library:8080`. Login as Admin. 
The default Admin pw is `changeme`, you need to change it in the settings here.

Select the Calibre library button and you should see a copy of Franz Kafka's Metamorphosis. click the + icon in the top banner and follow the instructions on loading new books. 

### Changes to the IIAB Admin panel
Open `ebook.library/admin` to go to the Admin page. 

The default Admin user is `iiab-admin`, and the default password is `g0adm1n`. 

Go to Content Menus, then content item list. Drag over every option in `Menu Items in Current Menu` except `Calibre-Web` into the `Available Menu Item Definitions` column.

Then click the Save Menu button, *then* click the Update Home Menu button on the left side. This is for any user that goes directly to the /home/ url.

---
---

At this point it should be ready to be run, but there are a few things to make this setup a little more streamlined.

## Code Changes
These code changes are best done if you have a little knowledge of HTML, CSS and JS, and are primarily done to remove IIAB branding to make a more unified user experience. 

### Splash Screen Changes
These are found in the `mac.template` and `simple.template` files in the `/opt/iiab/captiveportal/ directory`.

Here you can change the color scheme using the CSS at the top of the template.

You will also need to update the text being pulled in to the template pages from the `capture-wsgi.py` file, or you can just directly overwrite the text and hard code it into the template file. 

I've included examples from my setup in the [/splashScreenChanges/](splashScreenChanges) directory. 

### Home Page Changes
These changes are done in case anyone goes to the `/home/` screen.

Customize `/library/www/html/home/index.html` and the CSS files in `/library/www/html/js-menu/menu-files/css/js-menu.css` and `js-menu-item.css`. I've provided examples of the files in the [/homePageChanges/](homePageChanges) directory. 

I also swap out the homepage image with something more appropriate. The header image to change is found on line 51 of [index.html](homePageChanges/index.html), `iiab119.png`.

You may need to use the Move CLI command to move an image into the /js-menu/menu-files/images/ folder.

Remember the syntax 

    sudo mv <source> <destination>

In this case you will need to use 

    sudo mv /filename/of/newImage.png/ /js-menu/menu-files/images/


Sites of note:

    https://download.iiab.io/
    https://wiki.iiab.io/go/FAQ
    https://anarchosolarpunk.substack.com/p/offgridinternet
    https://anarchosolarpunk.substack.com/p/bannedbooklibrary


