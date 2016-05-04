# AvatolCVInstaller
Install script for AvatolCV

updateAvatolCV.py is the script that installs the latest version of each install bundle

###################################
#   REQUIREMENTS/DEPENDENCIES
###################################
    64 bit Windows / 64 bit OSX (Mac)
    python 2.7.x
    Java 8 64 bit
    MATLAB R2015b 
    8G RAM


###################################
#   GETTING THE INSTALLER
###################################
1. Got to https://github.com/AVATOL/AvatolCVInstaller

2. Click the button that says "Download ZIP"

3. unzip the file and then proceed with the instructions in the next section
###################################
#    INSTALLING AVATOLCV
###################################
To run it, open up a command shell and type:

%python updateAvatolCV.py install_root

...where install_root is replaced by your choice of what directory you want to install into.  The installer will create a subdirectory called 'avatol_cv' and place the system under that.  So, for example, you specify

%python updateAvatolCV.py C:\someDir

...then c:\someDir\avatol_cv will contain all the files for AvatolCV.  


###################################
#    RUNNING AVATOLCV
###################################
Once the files are in place, you would do:

%cd C:\someDir\avatol_cv\java\lib
%java -jar avatol_cv.jar

...to start the system

#####################################
#   WHAT THE INSTALLER DOES IN DETAIL
#####################################

AvatolCV is broken up into the following bundles:

    docs
    modules_3rdParty
    modules_osu
    java
      
usage:  python updateAvatolCV.py  <installRoot>

Here is the process updateAvatolCV.py uses:

1. ensures specified installation root dir exists
2. creates an avatol_cv dir under that installation root dir
3. determines a platform code - what system we are running on :  win | mac | unsupported
4. generates the name of the downloadManifest file that it needs to download (from http://web.engr.oregonstate.edu/~irvineje/AvatolCV/) using:

    downloadManifest_<platform_code>.txt

5. generates a temporary name that it will save that file under

    downloadManifest_<platform_code>_new.txt
 
6. downloads that file and saves as that name

    new_manifest_pathname = downloadFile(avatolcv_root, new_manifest_filename, manifest_truename)
    
7. if this is not the first installation maneuver, the prior maneuver would have saved the file as...

    downloadManifest_<platform_code>_old.txt
    
    ...so we look to see if that file exists
    
8. If that old file exists we dump it to the console, then dump the new one to the console for sanity comparison by user

9. we determine which bundle names need downloading by comparing old and new.  
   - If no old file exists, everything in new will be pulled.
   - If any bundle name represented in new is not represented in old, it is pulled
   - otherwise, we compare the datestamp (i.e. the version) associated with new to see if it is different than old. If so, we download.
   Note that we don't check for a more recent version, just difference with prior file.  This will simplify regressing back to prior version if need be.
   
   If no bundles are needed, we declare we are up to date and exit, otherwise we continue
  
10. foreach bundle we need to download, we first uninstall the prior-installed version of that bundle using the 

    allFiles_<bundle_name>.txt
    
    file that came with each bundle as a guide for which files to delete.
    
11. we delete the old manifest file and rename the new one to 

    downloadManifest_<platform_code>_old.txt
    
    ...so it can be cheacked at the later download
    
12. now we install each bundle by looking up the bundle filename from the downloadManifest and pulling from the distro site.
    This yields a .tgz bundle that we then unwrap underneath the avatolcv dir
    

