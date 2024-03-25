---
URL: https://tryhackme.com/room/ssrfqi
---
From content discovery we found that there are two new endpoints. 

The first one is `/private`, which gives us an error message explaining that the contents cannot be viewed from our IP address. 

The second is a new version of the customer account page at `/customers/new-account-page` with a new feature allowing customers to choose an avatar for their account.

Viewing the page source of the avatar form, we can see the avatar form field value contains the path to the image. The background-image style can confirm this in the above DIV element as per the screenshot below:

![[TMH_SSRF_image_path.png]]

If we choose one of the avatars and then click the Update Avatar button, we see the form change and, above it, displays our currently selected avatar.

![[SSRF_avatar_selection.png]] 

Viewing the page source will show our current avatar is displayed using the data URI scheme, and the image content is base64 encoded as per the screenshot below. 

![[SSRF_URI_image.png]]

We can try making the request again but changing the avatar value to `private` in hopes that the server will access the resource and *get past the IP address block*. To do this, firstly, right-click on one of the radio buttons on the avatar form and select Inspect:

![[SSRF_change_avatar_value.png]]

Unfortunately, it looks like the web application has a [[Server Side Request Forgery#^226f5b|deny list]] in place and has blocked access to the /private endpoint.

![[SSRF_deny_private.png]]

As we can see from the error message, the *path cannot start with* `/private`. We can use a directory traversal trick to reach our desired endpoint. Try setting the avatar value to `x/../private`. 

![[SSRF_change_URL.png]]

This trick works because when the web server receives the request for `x/../private`, it knows that the `../` string means to move up a directory that now translates the request to just `/private`.

Viewing the page source of the avatar form, we see that the currently set avatar now contains the contents from the /private directory in base64 encoding.

![[SSRF_private_successful.png]]

Decoding the string provides us with `THM{YOU_WORKED_OUT_THE_SSRF}`.

[[Server Side Request Forgery|SSRF]] 
