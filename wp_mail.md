# WordPress Emails

This document lists all the situations where WordPress sends an email, along with how to filter or disable each email.

This is accurate as of WordPress 4.8, and includes some upcoming changes in WordPress 4.9.

There are a few TODOs left. Please bear with me.

- [Comments](#comments)
- [Change of Admin Email](#change-of-admin-email)
- [Change of User Email or Password](#change-of-user-email-or-password)
- [Automatic Updates](#automatic-updates)
- [New User](#new-user)
- [New Site](#new-site)
- [Other](#other)

## Comments

### Comment is awaiting moderation
    To:        Site Admin, plus post author if they can edit comments
    From:      WordPress <wordpress@host>
    Subject:   [%s] Please moderate: "%s"
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
    Subject:   [%s] Comment: "%s"
               [%s] Pingback: "%s"
               [%s] Trackback: "%s"
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

### Multisite only: Change of site admin email address is attempted
    To:        Proposed new email address
    From:      WordPress <wordpress@host>
    Subject:   [%s] New Admin Email Address
    Function:  update_option_new_admin_email()
    Pluggable: No
    Filters:   new_admin_email_content
    Disable:   Remove action on add_option_new_admin_email and update_option_new_admin_email hooks

### Site admin email address is changed (WordPress 4.9+)
    To:        Old site admin email address
    From:      WordPress <wordpress@host>
    Subject:   [%s] Notice of Admin Email Change
    Function:  wp_site_admin_email_change_notification()
    Pluggable: No
    Filters:   site_admin_email_change_email
    Disable:   Return false from send_site_admin_email_change_email filter

### Multisite only: Network admin email address is changed (WordPress 4.9+)
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
    Disable:   Not possible

### User resets their password
    To:        Site admin
    From:      WordPress <wordpress@host>
    Subject:   [%s] Password Changed
    Function:  wp_password_change_notification()
    Pluggable: Yes
    Filters:   wp_password_change_notification_email (since 4.9)
    Disable:   Remove action on after_password_reset hook
               Overwrite the pluggable function

### User attempts to change their email address (Prior to WordPress 4.9, this was Multisite-only)
    To:        Proposed new email address
    From:      WordPress <wordpress@host>
    Subject:   [%s] New Email Address
    Function:  send_confirmation_on_profile_email()
    Pluggable: No
    Filters:   new_user_email_content
    Disable:   Remove action on personal_options_update hook

### User changes their password
    To:        User
    From:      WordPress <wordpress@host>
    Subject:   [%s] Notice of Password Change
    Function:  wp_update_user()
    Pluggable: No
    Filters:   password_change_email
    Disable:   Return false from send_password_change_email filter

### User changes their email address
    To:        User
    From:      WordPress <wordpress@host>
    Subject:   [%s] Notice of Email Change
    Function:  wp_update_user()
    Pluggable: No
    Filters:   email_change_email
    Disable:   Return false from send_email_change_email filter

## Automatic Updates

### Completion or failure of a background automatic core update
    To:        Site admin / Network admin
    From:      WordPress <wordpress@host>
    Subject:   [%s] Your site has updated to WordPress %s
               [%s] WordPress %s is available. Please update!
               [%s] URGENT: Your site may be down due to a failed update
    Function:  WP_Automatic_Updater::send_email()
    Pluggable: No
    Filters:   auto_core_update_email
    Disable:   Return false from auto_core_update_send_email filter
               Return false from send_core_update_notification_email filter

### Full log of background update results, sent when you are using a development version of WordPress
    To:        Site admin / Network admin
    From:      WordPress <wordpress@host>
    Subject:   [%s] There were failures during background updates
               [%s] Background updates have finished
    Function:  WP_Automatic_Updater::send_debug_email()
    Pluggable: No
    Filters:   automatic_updates_debug_email
    Disable:   Return false from automatic_updates_send_debug_email filter

## New User

### Multisite only: An existing user is added to a site from Users -> Add New -> Add Existing User
    To:        User being added
    From:      WordPress <wordpress@host>
    Subject:   [%s] Joining confirmation
    Function:  wp-admin/user-new.php
    Pluggable: No
    Filters:   None
    Disable:   Click the "Skip Confirmation Email" checkbox when adding the user

### Multisite only: A new user is added to a site from Users -> Add New -> Add New User
    To:        User being added
    From:      [Network Name] <[network admin]>
    Subject:   [%s] Activate %s
    Function:  wpmu_signup_user_notification()
    Pluggable: No
    Filters:   wpmu_signup_user_notification_subject
               wpmu_signup_user_notification_email
    Disable:   Click the "Skip Confirmation Email" checkbox when adding the user
               Return false from wpmu_signup_user_notification filter

### Multisite only: A new user account is created
    To:        Network Admin
    From:      WordPress <wordpress@host>
    Subject:   New User Registration: %s
    Function:  newuser_notify_siteadmin()
    Pluggable: No
    Filters:   newuser_notify_siteadmin
    Disable:   Filter registrationnotification option value
               Remove action on wpmu_new_user hook
               Toggle "Registration notification" in Network Admin -> Settings

### Multisite only: A user has been added, or their account activation has been successful
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

### @TODO [description]
    To:        Site Admin
    From:      WordPress <wordpress@host>
    Subject:   [%s] New User Registration
    Function:  wp_new_user_notification()
    Pluggable: Yes
    Filters:   wp_new_user_notification_email_admin (since 4.9)
    Disable:   Remove action on register_new_user hook
               Remove action on edit_user_created_user hook
               Remove action on network_site_new_created_user hook
               Remove action on network_site_users_created_user hook
               Remove action on network_user_new_created_user hook
               Overwrite the pluggable function

### @TODO [description]
    To:        User being added
    From:      WordPress <wordpress@host>
    Subject:   [%s] Your username and password info
    Function:  wp_new_user_notification()
    Pluggable: Yes
    Filters:   wp_new_user_notification_email (since 4.9)
    Disable:   Remove action on register_new_user hook
               Remove action on edit_user_created_user hook
               Remove action on network_site_new_created_user hook
               Remove action on network_site_users_created_user hook
               Remove action on network_user_new_created_user hook
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

### Multisite only: New site created from Network Admin -> Sites -> Add New
    To:        Network Admin
    From:      Site Admin <[network admin]>
    Subject:   [%s] New Site Created
    Function:  wp-admin/network/site-new.php
    Pluggable: No
    Filters:   None
    Disable:   Not possible

### Multisite only: User registers for a new site
    To:        Site Admin
    From:      [Network Name] <[network admin]>
    Subject:   [%s] Activate %s
    Function:  wpmu_signup_blog_notification()
    Pluggable: No
    Filters:   wpmu_signup_blog_notification_subject
               wpmu_signup_blog_notification_email
    Disable:   Return false from wpmu_signup_blog_notification filter
               Remove action on after_signup_site hook

### Multisite only: User activates their new site, or site added from Network Admin -> Sites -> Add New
    To:        Network Admin
    From:      WordPress <wordpress@host>
    Subject:   New Site Registration: %s
    Function:  newblog_notify_siteadmin()
    Pluggable: No
    Filters:   newblog_notify_siteadmin
    Disable:   Filter registrationnotification option value
               Remove action on wpmu_new_blog hook
               Toggle "Registration notification" in Network Admin -> Settings

### Multisite only: User activates their new site, or site added from Network Admin -> Sites -> Add New
    To:        Site Admin
    From:      [Network Name] <[network admin]>
    Subject:   New %s Site: %s
    Function:  wpmu_welcome_notification()
    Pluggable: No
    Filters:   update_welcome_subject
               update_welcome_email
               See also "Welcome Email" setting in Network Admin -> Settings
    Disable:   Return false from wpmu_welcome_notification filter
               Remove action on wpmu_activate_blog hook

## Other

### Multisite only: Site admin attempts to delete site from the Tools -> Delete Site menu
    To:        Site Admin
    From:      WordPress <wordpress@host>
    Subject:   [ %s ] Delete My Site
    Function:  wp-admin/ms-delete-site.php
    Pluggable: No
    Filters:   delete_site_email_content
    Disable:   Not possible
