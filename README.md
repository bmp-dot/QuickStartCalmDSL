Launch DevWorkstation from Calm Marketplace

![A screenshot of a cell phone Description automatically
generated](media/image1.png){width="2.2797200349956257in"
height="1.871411854768154in"}

-   Select the Credentials tab and enter desired User/Pass ((important))

-   Enter the name of Application "DevWorkstation-\<INITIALS\>

-   Press Create

-   Review audit log to see packages being deployed

![A screenshot of a cell phone Description automatically
generated](media/image2.png){width="1.7756703849518811in"
height="2.2632206911636046in"}

Once application is in running state SSH to the DevWorkstation VM

-   To get the IP address select to "Services" tab under the running
    application. Then select/highlight the service and the IP with show
    up on the right hand side

![A close up of a logo Description automatically
generated](media/image3.png){width="6.5in"
height="1.4270833333333333in"}

Start the virtual environment and connect to Prism Central

-   cd to the "calm-dsl" directory from your home

-   Run "source venv/bin/activate" to switch to the virtual environment.
    You will now be able to run calm commands via command line

-   **Optional:** This has been done automatically through the Blueprint
    Launch. Once you SSH into the DevWorkstation we can setup the
    connection to Prism Central by running "calm init dsl". We can also
    run "calm show config" to verify the connection settings

    -   Enter Prism Central IP:

    -   Enter Port:

    -   Enter Prism Central Username:

    -   Enter Prism Central Password:

    -   Enter Project name:

![A screenshot of a cell phone Description automatically
generated](media/image4.png){width="4.740780839895013in"
height="2.713286307961505in"}

List the current blueprints in Calm

-   Run ```calm get bps``` and we see all the blueprints in Calm with their
    UUID, description, application count, project, and state

![A screen shot of a computer Description automatically
generated](media/image5.png){width="4.6433562992125985in"
height="3.032568897637795in"}

-   Run ```calm get bps -q``` to display quiet output with only the BP names

![A screenshot of a cell phone Description automatically
generated](media/image6.png){width="4.916083770778653in"
height="2.0006944444444446in"}

For the next section we will review a python based blueprint and make a
modification

-   Change to the "HelloBlueprint" directory and do an "ls"

    -   This was automatically created during the blueprint launch.

-   You will see "blueprint.py" which is a python version of a blueprint

-   You will also see a "scripts" directory. This is where all
    bash/powershell/python is stored for the blueprint

![A screenshot of a cell phone Description automatically
generated](media/image7.png){width="6.111805555555556in"
height="1.7902099737532808in"}

-   Run "vi blueprint.py"

-   Review the blueprint and you will see some familiar constructs

    -   SSH Credentials (line 54-69)

    -   OS Image (line 62-65)

    -   Under class HelloPackage(Package) you will see references to the
        scripts in the script directory(line 139)

    -   Basic VM spec information (vCPU/memory/nics) (line 153-159)

    -   Guest Customization contains the cloud-init (line 161)

-   In the blueprint.py modify the number of vCPU

    -   Change the vCPU from 2 to 4 (line 154)

![A picture containing drawing, food Description automatically
generated](media/image8.png){width="6.5in"
height="1.1340277777777779in"}

-   Add a unique VM name using macros (line 185)

    -   provider\_spec.name = \"\<Initials\>-@@{calm\_unique}@@\"

![](media/image9.png){width="6.525223097112861in"
height="0.7223665791776028in"}

-   Write/quit ```:wq``` the .py blueprint file to save and close

For the next section we will modify ***pkg\_install\_task.sh***

-   Change to the scripts directory and run ```ls```. We will see 2 scripts that are being referenced inside the .py blueprint file

-   Run ```more pkg_install_task.sh``` to view the current contents of the
    install script.  What does this script do?

-   Run ```curl -Sks https://raw.githubusercontent.com/bmp-ntnx/prep/master/nginx > pkg_install_task.sh``` to replace the install script

-   Run ```more pkg_install_task.sh``` to view the changed script.  What does the script do now?

![A close up of a logo Description automatically
generated](media/image10.png){width="6.5in" height="1.03125in"}

Now we will upload the modified .py blueprint from Calm DSL to PC

-   Return to the "HelloBlueprint" directory

-   Run ```calm create bp --file blueprint.py --name FromDSL-<Initials>```

    -   This converts the .py file to json an sync it with Calm

-   Optional: Run ```calm compile bp -f blueprint.py``` to view the json format from DSL

![A screenshot of a cell phone Description automatically
generated](media/image11.png){width="6.5in"
height="0.8729166666666667in"}

-   Verify your new blueprint by running ```calm get bps -q```

![A screenshot of a cell phone Description automatically
generated](media/image12.png){width="5.67132874015748in"
height="2.4833333333333334in"}

The next step is to take your blueprint and to launch into an
application

-   Run ```calm get apps``` to verify all the current applications before
    launching your new app

    -   You can also run ```calm get apps -q``` to quiet the details like we did with blueprints earlier

![A screen shot of a computer Description automatically
generated](media/image13.png){width="6.5in"
height="2.6881944444444446in"}

-   Next launch your newly uploaded BP

-   Run ```calm launch bp <blueprint_name> --app_name AppFromDSL-<Initials> -i```

![A screenshot of a cell phone Description automatically
generated](media/image14.png){width="6.5in"
height="2.3027777777777776in"}

-   Run ```calm describe app AppFromDSL-<Initials>``` to see the application summary

![A screenshot of a cell phone Description automatically
generated](media/image15.png){width="6.5in"
height="3.9097222222222223in"}

-   Once the app status changes to "running" you will have a nginx server deployed from Calm DSL!

Log into Prism Central to verify

-   We can see the blueprint created from DSL

-   We can see the application launched from DSL

    -   On the application tab created from DSL select the "Services" tab and copy the IP

![A close up of a logo Description automatically
generated](media/image16.png){width="6.5in"
height="1.3416666666666666in"}

-   Enter the IP in a web browser and this will take you to the nginx ***"Welcome to DSL"*** web page

![A screenshot of a cell phone Description automatically
generated](media/image17.png){width="6.5in" height="3.8625in"}
