# Directory-Traversal-DVR

I found recently a vulnerability on some DVR. The flaw was simple to exploit: The authentication page tests if the password is correct or not and depending on the result it redirects you to "Camera.htm" or "Denied.htm" page.

The funny thing is that all the redirections are visible through GET method: "WAPLOGIN=admin&WAPPASSWORD=admin&PIC_SIZE=RES_0&FILEOK=camera.htm&FILEFAIL=denied.htm&Submit=OK"
As you noticed the parameters FILEOK & FILEFAIL contain the pages that the DVR will redirect to. So if we want to bypass the authentication, we can call directly the "camera.htm" like this: http://x.x.x.x/camera.htm since there is no mechanism to test if there is a valid session or not. Another method is to change the value of FILEFAIL from "denied.htm" to "camera.htm" and it will show you the desired page whether you enter a valid login/password or not.

The last part of this discovery is the directory traversal vulnerability. Calling the same url but now we will try to show an unauthorized content like /etc/passwd or /etc/shadow. To do the trick we will use ../ and we'll going back to parent directory till we found the file we wanted to show.

Since we're not giving the correct password, the DVR will redirect us to denied.htm, so we're going to use that parameter to point to the desired file.

curl -d "WAPLOGIN=admin&WAPPASSWORD=admin&PIC_SIZE=RES_0&FILEOK=camera.htm&FILEFAIL=../../etc/passwd&Submit=OK" http://x.x.x.x/cgi-bin/wappwd

This tutorial is only intended for education purpose not for any illegal activities.

Demo: https://www.youtube.com/watch?v=7OJrjUkdyuw


