# WordPress Emails

This document lists all the situations where WordPress core sends an email, how and when they happen, and how to filter or disable each one.

This list was last updated for WordPress 5.8.

## Table of Contents

- [Information Applicable to All Emails](#information-applicable-to-all-emails)
- [Comments](#comments)
- [Change of Admin Email](#change-of-admin-email)
- [Change of User Email or Password](#change-of-user-email-or-password)
- [Personal Data Requests](#personal-data-requests)
- [Automatic Updates](#automatic-updates)
- [New User](#new-user)
- [New Site](#new-site)
- [Other](#other)

## Information Applicable to All Emails

All emails sent by WordPress go through the pluggable `wp_mail()` function. The following general-purpose filters and actions are used in this function:

    Filters:   wp_mail
               pre_wp_mail
               wp_mail_from
               wp_mail_from_name
               wp_mail_content_type
               wp_mail_charset
    Actions:   phpmailer_init
               wp_mail_failed

## Comments

### Comment is awaiting moderation

Sent when a user or visitor submits a comment that gets held for moderation.

    To:        Site Admin
               Post author, if they have the ability to edit the comment
    From:      WordPress <wordpress@host>
    Subject:   [%1$s] Please moderate: "%2$s"
    Function:  wp_notify_moderator()
    Pluggable: Yes
    Filters:   comment_moderation_recipients
               comment_moderation_subject
               comment_moderation_text
               comment_moderation_headers
    Disable:   Return false from notify_moderator filter
               Remove wp_new_comment_notify_moderator action on comment_post hook
               "Email me whenever" on Settings -> Discussion
               Overwrite the pluggable function

### Comment is published

Sent when a user or visitor submits a comment that gets automatically approved, or when a comment previously held for moderation gets approved.

    To:        Post author
    From:      WordPress <wordpress@host>
    Subject:   [%1$s] Comment: "%2$s"
               [%1$s] Pingback: "%2$s"
               [%1$s] Trackback: "%2$s"
    Function:  wp_notify_postauthor()
    Pluggable: Yes
    Filters:   comment_notification_recipients
               comment_notification_subject
               comment_notification_text
               comment_notification_headers
    Disable:   Return false from notify_post_author filter
               Remove wp_new_comment_notify_postauthor action on comment_post hook
               Overwrite the pluggable function
               See also the hardcoded action added to wp_set_comment_status in wp_set_comment_status()

## Change of Admin Email

### Change of site admin email address is attempted

Sent when a user attempts to change the Administration Email Address option on the General Settings screen.

    To:        Proposed new email address
    From:      WordPress <wordpress@host>
    Subject:   [%s] New Admin Email Address
    Function:  update_option_new_admin_email()
    Pluggable: No
    Filters:   new_admin_email_content
    Disable:   Remove action on add_option_new_admin_email and update_option_new_admin_email hooks
    Note:      Prior to WordPress 4.9 this was Multisite-only functionality

### Site admin email address is changed

Sent when a user clicks the link in the email requesting confirmation of the change to the site admin email address (see above).

    To:        Old site admin email address
    From:      WordPress <wordpress@host>
    Subject:   [%s] Admin Email Changed
    Function:  wp_site_admin_email_change_notification()
    Pluggable: No
    Filters:   site_admin_email_change_email
    Disable:   Return false from send_site_admin_email_change_email filter
    Note:      Prior to WordPress 5.2 the subject was: [%s] Notice of Admin Email Change

### Change of network admin email address is attempted

Multisite only. Sent when a user attempts to change the Network Admin Email option on the Network Settings screen.

    To:        Proposed new email address
    From:      WordPress <wordpress@host>
    Subject:   [%s] Network Admin Email Change Request
    Function:  update_network_option_new_admin_email()
    Pluggable: No
    Filters:   new_network_admin_email_content
    Disable:   Remove action on add_site_option_new_admin_email and update_site_option_new_admin_email hooks
    Note:      Prior to WordPress 5.2 the subject was: [%s] New Network Admin Email Address

### Network admin email address is changed

Multisite only. Sent when a user clicks the link in the email requesting confirmation of the change to the network admin email (see above).

    To:        Old network admin email address
    From:      WordPress <wordpress@host>
    Subject:   [%s] Notice of Network Admin Email Change
    Function:  wp_network_admin_email_change_notification()
    Pluggable: No
    Filters:   network_admin_email_change_email
    Disable:   Return false from send_network_admin_email_change_email filter

## Change of User Email or Password

### User or Administrator requests a password reset

Sent when:

* A user clicks the "Lost your password?" link on the login screen and submits their email address
* An Administrator clicks the "Send password reset" link next to a user on the Users screen (WP 5.7+)
* Ad Administrator clicks the "Send Reset Link" from the user editing screen of another user (WP 5.7+)

Details:

    To:        User
    From:      WordPress <wordpress@host>
    Subject:   [%s] Password Reset
    Function:  retrieve_password()
    Pluggable: No
    Filters:   retrieve_password_title
               retrieve_password_message
    Disable:   Return an empty message from retrieve_password_message filter

### User resets their password

Sent when a user resets their password after clicking the confirmation link sent by the "Lost your password?" feature (see above).

    To:        Site admin
    From:      WordPress <wordpress@host>
    Subject:   [%s] Password Changed
    Function:  wp_password_change_notification()
    Pluggable: Yes
    Filters:   wp_password_change_notification_email
    Disable:   Remove action on after_password_reset hook
               Overwrite the pluggable function

### User changes their password

Sent when a logged in user changes their password from the user profile screen.

    To:        User
    From:      WordPress <wordpress@host>
    Subject:   [%s] Password Changed
    Function:  wp_update_user()
    Pluggable: No
    Filters:   password_change_email
    Disable:   Return false from send_password_change_email filter
    Note:      Prior to WordPress 5.2 the subject was: [%s] Notice of Password Change

### User attempts to change their email address

Sent when a logged in user attempts to change their email address from the user profile screen.

    To:        Proposed new email address
    From:      WordPress <wordpress@host>
    Subject:   [%s] Email Change Request
    Function:  send_confirmation_on_profile_email()
    Pluggable: No
    Filters:   new_user_email_content
    Disable:   Remove action on personal_options_update hook
    Note:      Prior to WordPress 5.2 the subject was: [%s] New Email Address
    Note:      Prior to WordPress 4.9 this was Multisite-only functionality

### User changes their email address

Sent when a user clicks the link in the email requesting confirmation of the change to their email address (see above).

    To:        User
    From:      WordPress <wordpress@host>
    Subject:   [%s] Email Changed
    Function:  wp_update_user()
    Pluggable: No
    Filters:   email_change_email
    Disable:   Return false from send_email_change_email filter
    Note:      Prior to WordPress 5.2 the subject was: [%s] Notice of Email Change

## Personal Data Requests

### Personal data export or erasure request is created or resent

Sent when a request is created or resent from the Tools -> Export Personal Data or Tools -> Erase Personal Data screen.

    To:        Requester email address
    From:      WordPress <wordpress@host>
    Subject:   [%1$s] Confirm Action: %2$s
    Function:  wp_send_user_request()
    Pluggable: No
    Filters:   user_request_action_email_subject
               user_request_action_email_content
               user_request_action_email_headers
    Disable:   Unknown, may have to remove the admin pages entirely by removing _wp_privacy_hook_requests_page action from admin_menu hook

### User confirms personal data export or erasure request

Sent when a user clicks the link in the personal data export or erasure request confirmation email (see above).

    To:        Site admin on a single site installation, Network admin on Multisite
    From:      WordPress <wordpress@host>
    Subject:   [%1$s] Action Confirmed: %2$s
    Function:  _wp_privacy_send_request_confirmation_notification()
    Pluggable: No
    Filters:   user_request_confirmed_email_to
               user_request_confirmed_email_subject
               user_request_confirmed_email_content (WP 5.9+)
               user_request_confirmed_email_headers
               user_confirmed_action_email_content (deprecated in 5.9)
    Disable:   Remove action on user_request_action_confirmed hook

### Site admin sends link to a personal data export

Sent when a site admin clicks the Send Export Link button next to a confirmed data export request.

    To:        Requester email address
    From:      WordPress <wordpress@host>
    Subject:   [%s] Personal Data Export
    Function:  wp_privacy_send_personal_data_export_email()
    Pluggable: No
    Filters:   wp_privacy_personal_data_email_to
               wp_privacy_personal_data_email_subject
               wp_privacy_personal_data_email_content
               wp_privacy_personal_data_email_headers
    Disable:   Remove filter on wp_privacy_personal_data_export_page hook

### Site admin erases personal data to fulfill a data erasure request

Sent when a site admin clicks the Erase Personal Data button next to a confirmed data erasure request, or clicks the Force Erase Personal Data button next to a data erasure request of any status.

    To:        Requester email address
    From:      WordPress <wordpress@host>
    Subject:   [%s] Erasure Request Fulfilled
    Function:  _wp_privacy_send_erasure_fulfillment_notification()
    Pluggable: No
    Filters:   user_erasure_fulfillment_email_to
               user_erasure_fulfillment_email_subject (WP 5.9+)
               user_erasure_fulfillment_email_content (WP 5.9+)
               user_erasure_fulfillment_email_headers (WP 5.9+)
               user_erasure_complete_email_subject (deprecated in 5.9)
               user_confirmed_action_email_content (deprecated in 5.9)
               user_erasure_complete_email_headers (deprecated in 5.9)
    Disable:   Remove filter on wp_privacy_personal_data_erased hook

## Automatic Updates

### Automatic plugin or theme updates

Sent when a background automatic update to plugins and/or themes completes or fails.

    To:        Site admin on a single site installation, Network admin on Multisite
    From:      WordPress <wordpress@host>
    Subject:   [%s] Some plugins and themes have automatically updated
               [%s] Some plugins were automatically updated
               [%s] Some themes were automatically updated
               [%s] Some plugins and themes have failed to update
               [%s] Some plugins have failed to update
               [%s] Some themes have failed to update
    Function:  WP_Automatic_Updater::after_plugin_theme_update()
    Pluggable: No
    Filters:   auto_plugin_theme_update_email
    Disable:   Return false from auto_plugin_update_send_email filter
               Return false from auto_theme_update_send_email filter

### Automatic core update

Sent when a background automatic update to WordPress core completes or fails.

    To:        Site admin on a single site installation, Network admin on Multisite
    From:      WordPress <wordpress@host>
    Subject:   [%1$s] Your site has updated to WordPress %2$s
               [%1$s] WordPress %2$s is available. Please update!
               [%1$s] URGENT: Your site may be down due to a failed update
    Function:  WP_Automatic_Updater::send_email()
    Pluggable: No
    Filters:   auto_core_update_email
    Disable:   Return false from auto_core_update_send_email filter
               Return false from send_core_update_notification_email filter

### Full log of background update results

Only sent when you are using a development version of WordPress.

    To:        Site admin on a single site installation, Network admin on Multisite
    From:      WordPress <wordpress@host>
    Subject:   [%s] Background Update Failed
               [%s] Background Update Finished
    Function:  WP_Automatic_Updater::send_debug_email()
    Pluggable: No
    Filters:   automatic_updates_debug_email
    Disable:   Return false from automatic_updates_send_debug_email filter
    Note:      Prior to WordPress 5.2 the subjects were:
               [%s] There were failures during background updates
               [%s] Background updates have finished

## New User

### An existing user is invited to a site

Multisite only. Sent when an existing user is added to a site from Users -> Add New -> Add Existing User.

    To:        User being invited
    From:      WordPress <wordpress@host>
    Subject:   [%s] Joining Confirmation
    Function:  wp-admin/user-new.php
    Pluggable: No
    Filters:   invited_user_email (WP 5.6+)
    Disable:   Click the "Skip Confirmation Email" checkbox when adding the user
    Note:      Prior to WordPress 5.6 there was no filter to alter this email

### A new user is invited to join a site

Multisite only. Sent when a new user is invited to join a site from Users -> Add New -> Add New User.

    To:        User being invited
    From:      [Network Name] <[network admin]>
    Subject:   [%1$s] Activate %2$s
    Function:  wpmu_signup_user_notification()
    Pluggable: No
    Filters:   wpmu_signup_user_notification_subject
               wpmu_signup_user_notification_email
    Disable:   Click the "Skip Confirmation Email" checkbox when adding the user
               Return false from wpmu_signup_user_notification filter
    Note:      There is a default filter on wpmu_signup_user_notification_email: admin_created_user_email()

### A new user account is created

Multisite only. Sent when a new user account is created via `wpmu_create_user()`:

* From Network Admin -> Sites -> Add New and the email address doesn't already exist
* From Network Admin -> Sites -> [Edit] -> Users -> Add New User
* From Network Admin -> Users -> Add New
* From Users -> Add New -> Add New User and the "Skip Confirmation Email" checkbox is checked
* When a user activates their new account on `wp-activate.php`
* Via a REST API request to create a new user (`POST` to `/wp/v2/users`)

Details:

    To:        Network Admin
    From:      WordPress <wordpress@host>
    Subject:   New User Registration: %s
    Function:  newuser_notify_siteadmin()
    Pluggable: No
    Filters:   newuser_notify_siteadmin
    Disable:   Filter registrationnotification option value
               Remove action on wpmu_new_user hook
               Toggle "Registration notification" in Network Admin -> Settings

### A user is added, or their account activation is successful

Multisite only.

    To:        User being added
    From:      [Network Name] <[network admin]>
    Subject:   New %s User: %s
    Function:  wpmu_welcome_user_notification()
    Pluggable: No
    Filters:   update_welcome_user_subject
               update_welcome_user_email
               See also "Welcome User Email" setting in Network Admin -> Settings
    Disable:   Click the "Skip Confirmation Email" checkbox when adding the user
               Return false from wpmu_welcome_user_notification filter
               Remove action on wpmu_activate_user hook

### A new user is created

When a new user is created, two emails are sent from the same function. One to the site admin:

    To:        Site Admin
    From:      WordPress <wordpress@host>
    Subject:   [%s] New User Registration
    Filters:   wp_new_user_notification_email_admin

and one to the newly created user:

    To:        New user
    From:      WordPress <wordpress@host>
    Subject:   [%s] Login Details
    Filters:   wp_new_user_notification_email
    Note:      Prior to WordPress 5.2 the subject was: [%s] Your username and password info

Details:

    Function:  wp_new_user_notification()
    Pluggable: Yes
    Disable:   Remove wp_send_new_user_notifications action on register_new_user hook
               Remove wp_send_new_user_notifications action on edit_user_created_user hook
               Remove wp_send_new_user_notifications action on network_site_new_created_user hook
               Remove wp_send_new_user_notifications action on network_site_users_created_user hook
               Remove wp_send_new_user_notifications action on network_user_new_created_user hook
               Overwrite the pluggable function

## New Site

### A new site is created

Multisite only. Sent when a new site is created from Network Admin -> Sites -> Add New.

    To:        Network Admin
    From:      Site Admin <[network admin]>
    Subject:   [%s] New Site Created
    Function:  wpmu_new_site_admin_notification()
    Pluggable: No
    Filters:   new_site_email (WP 5.6+)
    Disable:   Return false from send_new_site_email filter (WP 5.6+)
    Note:      Prior to WordPress 5.6 there were no filters to alter or disable this email

### User registers for a new site

Multisite only, with site registration allowed. Sent when a visitor registers a new user account and site from wp-signup.php.

    To:        Site Admin
    From:      [Network Name] <[network admin]>
    Subject:   [%1$s] Activate %2$s
    Function:  wpmu_signup_blog_notification()
    Pluggable: No
    Filters:   wpmu_signup_blog_notification_subject
               wpmu_signup_blog_notification_email
    Disable:   Return false from wpmu_signup_blog_notification filter
               Remove action on after_signup_site hook

### User activates their new site, or site added from Network Admin -> Sites -> Add New

Multisite only.

    To:        Network Admin
    From:      WordPress <wordpress@host>
    Subject:   New Site Registration: %s
    Function:  newblog_notify_siteadmin()
    Pluggable: No
    Filters:   newblog_notify_siteadmin
    Disable:   Filter registrationnotification option value
               Remove action on wpmu_new_blog or wp_initialize_site hooks
               Toggle "Registration notification" in Network Admin -> Settings

### User activates their new site, or site added from Network Admin -> Sites -> Add New

Multisite only.

    To:        New Site Admin
    From:      [Network Name] <[network admin]>
    Subject:   New %1$s Site: %2$s
    Function:  wpmu_welcome_notification()
    Pluggable: No
    Filters:   update_welcome_subject
               update_welcome_email
               See also "Welcome Email" setting in Network Admin -> Settings
    Disable:   Return false from wpmu_welcome_notification filter
               Remove action on wpmu_activate_blog hook

## Other

### Installation

Sent when WordPress is initially installed.

    To:        Site Admin
    From:      WordPress <wordpress@host>
    Subject:   New WordPress Site
    Function:  wp_new_blog_notification()
    Pluggable: Yes
    Filters:   wp_installed_email (WP 5.6+)
    Disable:   Overwrite the pluggable function
    Note:      Prior to WordPress 5.6 there was no filter to alter this email

### A fatal error occurs

Sent when a fatal error occurs in a plugin or theme and Recovery Mode is not active.

**Important:** The `wp_fatal_error_handler_enabled` filter cannot be used by plugins as it runs too early. [Information about using this filter can be found here](https://core.trac.wordpress.org/browser/trunk/src/wp-includes/error-protection.php?rev=49489&marks=114-131#L97).

    To:        Site Admin / Value of RECOVERY_MODE_EMAIL constant
    From:      WordPress <wordpress@host>
    Subject:   [%s] Your Site is Experiencing a Technical Issue
    Function:  send_recovery_mode_email()
    Pluggable: No
    Filters:   wp_fatal_error_handler_enabled
               recovery_email_support_info
               recovery_email_debug_info
               recovery_mode_email
    Disable:   Define WP_DISABLE_FATAL_ERROR_HANDLER as true
               Return false from wp_fatal_error_handler_enabled filter
    Note:      Does not currently apply to Multisite

### Site admin requests to delete site

Multisite only. Sent when an Administrator requests to delete their site from the Tools -> Delete Site screen.

    To:        Site Admin
    From:      WordPress <wordpress@host>
    Subject:   [%s] Delete My Site
    Function:  wp-admin/ms-delete-site.php
    Pluggable: No
    Filters:   delete_site_email_content
    Disable:   Not possible

## License: GPLv2

Copyright 2015 - 2021 John Blackbourn

This documentation is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either version 2 of the License, or
(at your option) any later version.

This documentation is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.
