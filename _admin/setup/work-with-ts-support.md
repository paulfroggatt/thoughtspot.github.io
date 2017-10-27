---
title: [Configure support services]
tags: [SysAdm_tscli_Config, Supportability_End_User]
keywords: tbd
last_updated: tbd
summary: "Set up and configure ThoughtSpot support services for your installation."
sidebar: mydoc_sidebar
permalink: /:collection/:path.html
---

There are several configurations you can set up in your installation to ensure your company's support from ThoughtSpot works smoothly.

## Set up a reverse tunnel for support

You can set up a reverse tunnel to allow ThoughtSpot Support to get access to your ThoughtSpot instance, to perform support-related activities. This setup is a much simpler, more secure, and scalable than the alternative option of using a virtual meeting room.

Granting remote support access can streamline troubleshooting activities, since it enables your support agent to work directly in a secure setting. The remote tunnel enables SSH and HTTP access to your by ThoughtSpot Support. This access can be granted and revoked easily, so you can enable it for a troubleshooting session, and then disable it again. Before doing this procedure, make sure your company's security policies allow reverse tunneling.

Before you can do this procedure, your networking team needs to open port `22` in your firewall outgoing rules.

To enable remote support:

1. [Contact ThoughtSpot]({{ site.baseurl }}/admin/misc/contact.html#) to open a support ticket for making the appropriate reverse tunnel settings on our end. Provide the cluster name of the cluster for which you want to enable remote support.
2. When the ticket has been completed, continue with the remaining steps in this procedure to make the settings on your side.
3. Log in to the Linux shell using SSH.
4. Issue the command to configure the destination for the remote tunnel. You only need to do this once, when you are enabling the tunnel for the very first time. After that, this setting persists when you start and stop the remote tunnel.

    ```
    $ tscli support set-remote --addr tunnel.thoughtspot.com --user ubuntu
    ```

5. Test that the setting has been applied:

    ```
    $ tscli support show-remote
    ```

6. Enable the remote tunnel:

    ```
    $ tscli support start-remote
    ```

7. [Contact ThoughtSpot]({{ site.baseurl }}/admin/misc/contact.html#) again, so you can test the setup with your ThoughtSpot Support contact.
8. After your remote session with ThoughtSpot Support, you should turn the remote tunnel off, until you need to use it again:

    ```
    $ tscli support stop-remote
    ```

    You can repeat the steps to start and stop the remote tunnel as needed for future support operations.

9. Test that the remote tunnel has been disabled:

    ```
    $ tscli support show-remote
    ```

## Configure a secure file server

ThoughtSpot Support uses a secure file server to provide new releases and to receive logs and troubleshooting files that you upload. The secure server connection is also required if you want to enable the optional statistics collection using the call home feature.

Before you can upload a file to the secure file server, obtain your user name and password for logging in to the secure file server. You can get these from ThoughtSpot Support.

Configuring the connection to the file server is a one time operation. You do not need to reconfigure the connection unless your password changes. Note that you can do a one time override of the user and password you used to configure the connection. This is done by passing a different user and password on the command line when uploading or downloading a file.

To configure the connection to the secure file server:

1. Log in to the Linux shell using SSH.
2. Issue the command to configure the file server:

    ```
    $ tscli fileserver configure --user <user_name> [--password <password>]
    ```

    If you do not supply the `--password` parameter, you are prompted to enter it.


## Enable call home capability

The optional "call home" capability sends usage statistics to ThoughtSpot Support once a day via a secure file server.

Before you can enable the call home feature:

1.  [Configure the connection to the file server](work-with-ts-support.html#).
2.  Obtain the customer name as recognized by the file server.

The customer name is formatted like this example: `Shared/<customer_name>/stats`. If you do not know the customer name, [contact ThoughtSpot Support]({{ site.baseurl }}/admin/misc/contact.html#).

This can be helpful when troubleshooting problems with ThoughtSpot Support, because they will be able to see basic usage information over time for your ThoughtSpot instance.

To set up the call home feature:

1. Log in to the Linux shell using SSH.
2. Enable the call home feature by issuing:

    ```
    $ tscli callhome enable --customer_name <customer_name>
    ```

3. If you want to disable call home in the future, you can do so by issuing:

    ```
    $ tscli callhome disable
    ```

## Designate a support contact

A support contact person can answer questions for about data and searching at your company. If the person can't answer a question that person should submit  system and software-related questions to ThoughtSpot Support. Your designated support contact should have an available email and phone number.


To designate the custom support contact:

1. Log in to the Linux shell using SSH.
2. Issue the tscli command to set the email address:

    ```
    $ tscli support set-admin-email <email_address>
    ```

3. Issue the tscli command to set the phone number:

    ```
    $ tscli support set-admin-phone <phone_number>
    ```

4. If you need to reset both of these to the default (ThoughtSpot Support), issue:

    ```
    $ tscli support rm-admin-email

    $ tscli support rm-admin-phone
    ```

## How users find your company's support contact

After you set the custom support contact information, here's where your users will see it:

-   In the Help Center, when they click **Contact Support**.

     ![]({{ site.baseurl }}/images/help_center_support_contact.png "Help Center support contact")

-   In error messages, when they click **What Happened?**

     ![]({{ site.baseurl }}/images/trace_log.png "Error message support contact")
