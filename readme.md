# WordPress Emails

This document lists all the situations where WordPress sends an email, along with how to filter or disable each email.

This is accurate as of WordPress 5.2 beta 3.

Note that several email subjects were changed in WordPress 5.2.

## Table of Contents

- [Comments](#comments)
- [Change of Admin Email](#change-of-admin-email)
- [Change of User Email or Password](#change-of-user-email-or-password)
- [Personal Data Requests](#personal-data-requests)
- [Automatic Updates](#automatic-updates)
- [New User](#new-user)
- [New Site](#new-site)
- [Delete Site](#delete-site)
- [Other](#other)

## Comments

### Comment is awaiting moderation

    To:        Site Admin, plus post author if they can edit comments
    From:      WordPress <wordpress@host>
    Subject:   [%1$s] Please moderate: "%2$s"
    Function:  wp_notify_moderator()
    Pluggable: Yes
    Filters:   comment_moderation_subject
               comment_moderation_text
               comment_moderation_recipients
               comment_moderation_headers
    Disable:   Return false from notify_moderator filter
               Remove wp_new_comment_notify_moderator action on comment_post hook
               "Email me whenever" on Settings -> Discussion
               Overwrite the pluggable function

### Comment has been published

    To:        Post author
    From:      WordPress <wordpress@host>
    Subject:   [%1$s] Comment: "%2$s"
               [%1$s] Pingback: "%2$s"
               [%1$s] Trackback: "%2$s"
    Function:  wp_notify_postauthor()
    Pluggable: Yes
    Filters:   comment_notification_subject
               comment_notification_text
               comment_notification_recipients
               comment_notification_headers
    Disable:   Return false from notify_post_author filter
               Remove wp_new_comment_notify_postauthor action on comment_post hook
               Overwrite the pluggable function
               See also the hardcoded action added to wp_set_comment_status in wp_set_comment_status()

## Change of Admin Email

### Change of site admin email address is attempted

**Note:** Prior to WordPress 4.9, this was Multisite-only functionality.

    To:        Proposed new email address
    From:      WordPress <wordpress@host>
    Subject:   [%s] New Admin Email Address
    Function:  update_option_new_admin_email()
    Pluggable: No
    Filters:   new_admin_email_content
    Disable:   Remove action on add_option_new_admin_email and update_option_new_admin_email hooks

### Site admin email address is changed

    To:        Old site admin email address
    From:      WordPress <wordpress@host>
    Subject:   [%s] Admin Email Changed (WP 5.2+)
               [%s] Notice of Admin Email Change (WP < 5.2)
    Function:  wp_site_admin_email_change_notification()
    Pluggable: No
    Filters:   site_admin_email_change_email
    Disable:   Return false from send_site_admin_email_change_email filter

### Change of network admin email address is attempted

**Note:** Multisite only.

    To:        Proposed new email address
    From:      WordPress <wordpress@host>
    Subject:   [%s] Network Admin Email Change Request (WP 5.2+)
               [%s] New Network Admin Email Address (WP < 5.2)
    Function:  update_network_option_new_admin_email()
    Pluggable: No
    Filters:   new_network_admin_email_content
    Disable:   Remove action on add_site_option_new_admin_email and update_site_option_new_admin_email hooks

### Network admin email address is changed

**Note:** Multisite only.

    To:        Old network admin email address
    From:      WordPress <wordpress@host>
    Subject:   [%s] Notice of Network Admin Email Change
    Function:  wp_network_admin_email_change_notification()
    Pluggable: No
    Filters:   network_admin_email_change_email
    Disable:   Return false from send_network_admin_email_change_email filter

## Change of User Email or Password

### User requests a password reset via "Lost your password?"

    To:        User
    From:      WordPress <wordpress@host>
    Subject:   [%s] Password Reset
    Function:  retrieve_password()
    Pluggable: No
    Filters:   retrieve_password_title
               retrieve_password_message
    Disable:   Return an empty message from retrieve_password_message filter

### User resets their password from the password reset link

    To:        Site admin
    From:      WordPress <wordpress@host>
    Subject:   [%s] Password Changed
    Function:  wp_password_change_notification()
    Pluggable: Yes
    Filters:   wp_password_change_notification_email
    Disable:   Remove action on after_password_reset hook
               Overwrite the pluggable function

### User attempts to change their email address

**Note:** Prior to WordPress 4.9, this was Multisite-only functionality.

    To:        Proposed new email address
    From:      WordPress <wordpress@host>
    Subject:   [%s] Email Change Request (WP 5.2+)
               [%s] New Email Address (WP < 5.2)
    Function:  send_confirmation_on_profile_email()
    Pluggable: No
    Filters:   new_user_email_content
    Disable:   Remove action on personal_options_update hook

### User changes their password

    To:        User
    From:      WordPress <wordpress@host>
    Subject:   [%s] Password Changed (WP 5.2+)
               [%s] Notice of Password Change (WP < 5.2)
    Function:  wp_update_user()
    Pluggable: No
    Filters:   password_change_email
    Disable:   Return false from send_password_change_email filter

### User changes their email address

    To:        User
    From:      WordPress <wordpress@host>
    Subject:   [%s] Email Changed (WP 5.2+)
               [%s] Notice of Email Change (WP < 5.2)
    Function:  wp_update_user()
    Pluggable: No
    Filters:   email_change_email
    Disable:   Return false from send_email_change_email filter

## Personal Data Requests

### Personal data export or erasure request is created or resent from Tools -> Export Personal Data or Tools -> Erase Personal Data

    To:        Requester email address
    From:      WordPress <wordpress@host>
    Subject:   [%1$s] Confirm Action: %2$s
    Function:  wp_send_user_request()
    Pluggable: No
    Filters:   user_request_action_email_content
               user_request_action_email_subject
    Disable:   Unknown, may have to remove the admin pages entirely by removing _wp_privacy_hook_requests_page action from admin_menu hook

### User clicks confirmation link in personal data export or erasure request email

    To:        Site admin / Network admin
    From:      WordPress <wordpress@host>
    Subject:   [%1$s] Action Confirmed: %2$s
    Function:  _wp_privacy_send_request_confirmation_notification()
    Pluggable: No
    Filters:   user_request_confirmed_email_to
               user_confirmed_action_email_content
               user_request_confirmed_email_subject
    Disable:   Remove action on user_request_action_confirmed hook

### Site admin clicks Send Export Link button next to a confirmed data export request

    To:        Requester email address
    From:      WordPress <wordpress@host>
    Subject:   [%s] Personal Data Export
    Function:  wp_privacy_send_personal_data_export_email()
    Pluggable: No
    Filters:   wp_privacy_personal_data_email_content
    Disable:   Remove filter on wp_privacy_personal_data_export_page hook

### Site admin clicks Erase Personal Data button next to a confirmed data erasure request

    To:        Requester email address
    From:      WordPress <wordpress@host>
    Subject:   [%s] Erasure Request Fulfilled
    Function:  _wp_privacy_send_erasure_fulfillment_notification()
    Pluggable: No
    Filters:   user_erasure_fulfillment_email_to
               user_erasure_complete_email_subject
               user_confirmed_action_email_content
    Disable:   Remove filter on wp_privacy_personal_data_erased hook

## Automatic Updates

### Completion or failure of a background automatic core update

    To:        Site admin / Network admin
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

**Note:** Only sent when you are using a development version of WordPress.

    To:        Site admin / Network admin
    From:      WordPress <wordpress@host>
    Subject:   [%s] Background Update Failed (WP 5.2+)
               [%s] Background Update Finished (WP 5.2+)
               [%s] There were failures during background updates (WP < 5.2)
               [%s] Background updates have finished (WP < 5.2)
    Function:  WP_Automatic_Updater::send_debug_email()
    Pluggable: No
    Filters:   automatic_updates_debug_email
    Disable:   Return false from automatic_updates_send_debug_email filter

## New User

### An existing user is invited to a site from Users -> Add New -> Add Existing User

**Note:** Multisite only.

    To:        User being invited
    From:      WordPress <wordpress@host>
    Subject:   [%s] Joining Confirmation
    Function:  wp-admin/user-new.php
    Pluggable: No
    Filters:   None
    Disable:   Click the "Skip Confirmation Email" checkbox when adding the user

### A new user is invited to a site from Users -> Add New -> Add New User

**Note:** Multisite only.

    To:        User being invited
    From:      [Network Name] <[network admin]>
    Subject:   [%1$s] Activate %2$s
    Function:  wpmu_signup_user_notification()
    Pluggable: No
    Filters:   wpmu_signup_user_notification_subject
               wpmu_signup_user_notification_email
    Disable:   Click the "Skip Confirmation Email" checkbox when adding the user
               Return false from wpmu_signup_user_notification filter

### A new user account is created

**Note:** Multisite only.

    To:        Network Admin
    From:      WordPress <wordpress@host>
    Subject:   New User Registration: %s
    Function:  newuser_notify_siteadmin()
    Pluggable: No
    Filters:   newuser_notify_siteadmin
    Disable:   Filter registrationnotification option value
               Remove action on wpmu_new_user hook
               Toggle "Registration notification" in Network Admin -> Settings

### A user has been added, or their account activation has been successful

**Note:** Multisite only.

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

**TODO:** Needs a lot more information here.

When a new user is created, two emails are sent from the same function. One to the site admin:

    To:        Site Admin
    From:      WordPress <wordpress@host>
    Subject:   [%s] New User Registration

and one to the newly created user:

    To:        User being added
    From:      WordPress <wordpress@host>
    Subject:   [%s] Login Details (WP 5.2+)
               [%s] Your username and password info (WP < 5.2)

Details:

    Function:  wp_new_user_notification()
    Pluggable: Yes
    Filters:   wp_new_user_notification_email_admin
               wp_new_user_notification_email
    Disable:   Remove wp_send_new_user_notifications action on register_new_user hook
               Remove wp_send_new_user_notifications action on edit_user_created_user hook
               Remove wp_send_new_user_notifications action on network_site_new_created_user hook
               Remove wp_send_new_user_notifications action on network_site_users_created_user hook
               Remove wp_send_new_user_notifications action on network_user_new_created_user hook
               Overwrite the pluggable function

## New Site

### When WordPress is installed, and when a site is added to a Multisite network

    To:        Site Admin
    From:      WordPress <wordpress@host>
    Subject:   New WordPress Site
    Function:  wp_new_blog_notification()
    Pluggable: Yes
    Filters:   None
    Disable:   Overwrite the pluggable function

### New site created from Network Admin -> Sites -> Add New

**Note:** Multisite only.

    To:        Network Admin
    From:      Site Admin <[network admin]>
    Subject:   [%s] New Site Created
    Function:  wp-admin/network/site-new.php
    Pluggable: No
    Filters:   None
    Disable:   Not possible

### User registers for a new site

**Note:** Multisite only.

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

**Note:** Multisite only.

    To:        Network Admin
    From:      WordPress <wordpress@host>
    Subject:   New Site Registration: %s
    Function:  newblog_notify_siteadmin()
    Pluggable: No
    Filters:   newblog_notify_siteadmin
    Disable:   Filter registrationnotification option value
               Remove action on wpmu_new_blog hook
               Toggle "Registration notification" in Network Admin -> Settings

### User activates their new site, or site added from Network Admin -> Sites -> Add New

**Note:** Multisite only.

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

## Delete Site

### Site admin deletes site from Tools -> Delete Site

**Note:** Multisite only.

    To:        Site Admin
    From:      WordPress <wordpress@host>
    Subject:   [ %s ] Delete My Site
    Function:  wp-admin/ms-delete-site.php
    Pluggable: No
    Filters:   delete_site_email_content
    Disable:   Not possible

### A fatal error occurs in a plugin or theme and Recovery Mode is not active

    To:        Site Admin / Value of RECOVERY_MODE_EMAIL constant
    From:      WordPress <wordpress@host>
    Subject:   [%s] Your Site is Experiencing a Technical Issue
    Function:  send_recovery_mode_email()
    Pluggable: No
    Filters:   wp_fatal_error_handler_enabled
    Disable:   Define WP_DISABLE_FATAL_ERROR_HANDLER as true
               Return false from wp_fatal_error_handler_enabled filter
