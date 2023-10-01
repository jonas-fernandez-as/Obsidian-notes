
This lab has an unprotected admin panel.

Solve the lab by deleting the user `carlos`.


This is a very easy lab, you need to go to the 
https://lab/robots.txt  file on the browser and then you  see that there is an admin-panel 
and then you just delete the carlos user.


# Unprotected admin functionality with unpredictable URL #




This lab has an unprotected admin panel. It's located at an unpredictable location, but the location is disclosed somewhere in the application.

Solve the lab by accessing the admin panel, and using it to delete the user `carlos`.


To solve this lab you must go to inspect the source code

The code is saying that if you're logged as admin the website will render another html labels created by javascript
 
You can see that the admin panel url is visible.
![[Screenshot_2023-09-18_16_52_53.png]]

You just need to go to the url and delete de user carlos.

https://0ae5008a03189ae1813bcbb600590063.web-security-academy.net/admin-2k3rbg


## Lab: User role controlled by request parameter ##

This lab has an admin panel at `/admin`, which identifies administrators using a forgeable cookie.

Solve the lab by accessing the admin panel and using it to delete the user `carlos`.

You can log in to your own account using the following credentials: `wiener:peter`


The only thing that you must do on this lab is just change the cookie where says admin=false to true, and then you will be able to delete the user carlos


first you must go to the url and intercept the petition, and change the cookie https://lab/admin


Select the user and delete it, rembember.. intercept the petition with burpsuit and change the cookie. Its pretty simple



## Lab: User ID controlled by request parameter, with unpredictable user IDs ##


This lab has a horizontal privilege escalation vulnerability on the user account page, but identifies users with GUIDs.

To solve the lab, find the GUID for `carlos`, then submit his API key as the solution.

You can log in to your own account using the following credentials: `wiener:peter`




This is an example of an insecure direct object reference (IDOR) vulnerability. This type of vulnerability arises where user-controller parameter values are used to access resources or functions directly.


On this lab we must go and watch the post of the people, then watch one of carlos. the id is on the url... just copy it go to my account and then replace your id for the id of carlos.

You will see the carlos token.

Finally submit the answer.



## Lab: User ID controlled by request parameter with password disclosure ##



This lab has user account page that contains the current user's existing password, prefilled in a masked input.

To solve the lab, retrieve the administrator's password, then use it to delete the user `carlos`.

You can log in to your own account using the following credentials: `wiener:peter`



On this lab, you must change the id on the browser for administrator (security-academy.net/my-account?id=administrator). Make a petition of password change and then intercept the petition with burpsuit, you will see that the password is lenf5zmep4e64q4vk6t.

Then log in as administrator with that password, and finally delete the user carlos.


