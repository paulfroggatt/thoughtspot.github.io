---
title: [Authentication]
tags:
keywords: data,cluster,memory,import,tsload,SAML,LDAP,"active directory"
summary: "ThoughtSpot does all analysis against data in memory to help achieve
fast results across millions and billions of records of data."
sidebar: mydoc_sidebar
permalink: /:collection/:path.html
---
ThoughtSpot provides three ways to authenticate users  LDAP/AD, SAML, and
ThoughtSpot login. In general, ThoughtSpot recommends that you use LDAP/AD or
SAML if possible since ThoughtSpot provides only basic authentication with no
restrictions on passwords, timeouts, failed logins, etc.


The table below shows each of the options and the items to consider for each.


<table>
  <tr>
    <th>SAML</th>
    <th>LDAP/AD</th>
    <th>ThoughtSpot</th>
  </tr>
  <tr>
    <td>
    <ul>
    <li>Use SAML for single sign-on authentication.</li>
    <li>Can redirect from ThoughtSpot to SAML logins.</li>
    <li>Recommended for portal integration.</li>
    <li>Option to sync users and groups if stored in LDAP/AD.</li>
    </ul>
    </td>
    <td>
    <ul>
    <li>Configuration.</li>
    <li>Users authenticate against LDAP or AD.</li>
    <li>Option to sync users and groups with ThoughtSpot to manage group membership.</li>
    </ul>
    </td>
    <td>
    <ul>
    <li>User created and managed in ThoughtSpot.</li>
    <li>No enterprise password control (expiration, password strength, etc.).</li>
    <li>Only recommended when SAML and LDAP are not options.</li>
    </ul>
    </td>
  </tr>
</table>

All users and groups must be known to ThoughtSpot. If you are using LDAP/AD or
SAML and don’t create users in ThoughtSpot, a user is created when the user
first logs in. However, this user is assigned to the `All` group and will only
see content available for all users.

Groups are the primary way that security is managed. Groups are not
automatically created. You can create groups and users manually or you need to
automate the assignment from a source system. ThoughtSpot has an assignment
script that works with most LDAP / AD stores. It also has public APIs that you
can use to sync users and groups between source systems and your ThoughtSpot
appliance.
