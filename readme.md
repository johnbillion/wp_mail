# WordPress Emails

This document lists all the situations where WordPress core sends an email, how and when they happen, and how to filter or disable each one.

This list was last updated for WordPress 6.0.

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

All emails sent by WordPress go through the pluggable <a href="https://developer.wordpress.org/reference/functions/wp_mail/"><code>wp_mail()</code></a> function. The following general-purpose filters and actions are used in this function:

<table width="100%" style="width:100%">
	<tr>
		<th valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/wp_mail/"><code>wp_mail</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/pre_wp_mail/"><code>pre_wp_mail</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/wp_mail_from/"><code>wp_mail_from</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/wp_mail_from_name/"><code>wp_mail_from_name</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/wp_mail_content_type/"><code>wp_mail_content_type</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/wp_mail_charset/"><code>wp_mail_charset</code></a><br>
		</td>
	</tr>
	<tr>
		<th valign="top" align="left">Actions</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/phpmailer_init/"><code>phpmailer_init</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/wp_mail_succeeded/"><code>wp_mail_succeeded</code></a> (WP 5.9+)<br>
			<a href="https://developer.wordpress.org/reference/hooks/wp_mail_failed/"><code>wp_mail_failed</code></a><br>
		</td>
	</tr>
</table>

## Comments

### Comment is awaiting moderation

Sent when a user or visitor submits a comment that gets held for moderation.

<table>
	<tr>
		<th valign="top" align="left">To</th>
		<td>
			Site Admin<br>
			Post author, if they have the ability to edit the comment
		</td>
	</tr>
	<tr>
		<th valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th valign="top" align="left">Subject</th>
		<td>[%1$s] Please moderate: "%2$s"</td>
	</tr>
	<tr>
		<th valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/wp_notify_moderator/"><code>wp_notify_moderator()</code></a></td>
	</tr>
	<tr>
		<th valign="top" align="left">Pluggable</th>
		<td>Yes</td>
	</tr>
	<tr>
		<th valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/comment_moderation_recipients/"><code>comment_moderation_recipients</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/comment_moderation_subject/"><code>comment_moderation_subject</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/comment_moderation_text/"><code>comment_moderation_text</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/comment_moderation_headers/"><code>comment_moderation_headers</code></a><br>
		</td>
	</tr>
	<tr>
		<th valign="top" align="left">Disable</th>
		<td>
			- Return false from <a href="https://developer.wordpress.org/reference/hooks/notify_moderator/"><code>notify_moderator</code></a> filter<br>
			- Remove <a href="https://developer.wordpress.org/reference/functions/wp_new_comment_notify_moderator/"><code>wp_new_comment_notify_moderator</code></a> action from <a href="https://developer.wordpress.org/reference/hooks/comment_post/"><code>comment_post</code></a> hook<br>
			- "Email me whenever" settings on Settings -> Discussion screen<br>
			- Overwrite the pluggable <a href="https://developer.wordpress.org/reference/functions/wp_notify_moderator/"><code>wp_notify_moderator()</code></a> function<br>
		</td>
	</tr>
</table>

### Comment is published

Sent when:

* A user or visitor submits a comment that gets automatically approved
* A comment previously held for moderation gets approved.

<table>
	<tr>
		<th valign="top" align="left">To</th>
		<td>Post author</td>
	</tr>
	<tr>
		<th valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th valign="top" align="left">Subject</th>
		<td>
			[%1$s] Comment: "%2$s"<br>
			[%1$s] Pingback: "%2$s"<br>
			[%1$s] Trackback: "%2$s"<br>
		</td>
	</tr>
	<tr>
		<th valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/wp_notify_postauthor/"><code>wp_notify_postauthor()</code></a></td>
	</tr>
	<tr>
		<th valign="top" align="left">Pluggable</th>
		<td>Yes</td>
	</tr>
	<tr>
		<th valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/comment_notification_recipients/"><code>comment_notification_recipients</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/comment_notification_subject/"><code>comment_notification_subject</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/comment_notification_text/"><code>comment_notification_text</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/comment_notification_headers/"><code>comment_notification_headers</code></a><br>
		</td>
	</tr>
	<tr>
		<th valign="top" align="left">Disable</th>
		<td>
			- Return false from <a href="https://developer.wordpress.org/reference/hooks/notify_post_author/"><code>notify_post_author</code></a> filter<br>
			- Remove <a href="https://developer.wordpress.org/reference/functions/wp_new_comment_notify_postauthor/"><code>wp_new_comment_notify_postauthor</code></a> action from <a href="https://developer.wordpress.org/reference/hooks/comment_post/"><code>comment_post</code></a> hook<br>
			- "Email me whenever" settings on Settings -> Discussion screen<br>
			- Overwrite the pluggable <a href="https://developer.wordpress.org/reference/functions/wp_notify_postauthor/"><code>wp_notify_postauthor()</code></a> function<br>
			- See also <a href="https://github.com/WordPress/wordpress-develop/blob/63a2a710680cf344dec9e75cec757ee377a304a9/src/wp-includes/comment.php#L2404">this hardcoded action</a> added to <a href="https://developer.wordpress.org/reference/hooks/wp_set_comment_status/"><code>wp_set_comment_status</code></a> in <a href="https://developer.wordpress.org/reference/functions/wp_set_comment_status/"><code>wp_set_comment_status()</code></a><br>
		</td>
	</tr>
</table>

## Change of Admin Email

### Change of site admin email address is attempted

Sent when a user attempts to change the Administration Email Address option on the General Settings screen.

<table>
	<tr>
		<th valign="top" align="left">To</th>
		<td>Proposed new email address</td>
	</tr>
	<tr>
		<th valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th valign="top" align="left">Subject</th>
		<td>[%s] New Admin Email Address</td>
	</tr>
	<tr>
		<th valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/update_option_new_admin_email/"><code>update_option_new_admin_email()</code></a></td>
	</tr>
	<tr>
		<th valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/new_admin_email_content/"><code>new_admin_email_content</code></a><br>
		</td>
	</tr>
	<tr>
		<th valign="top" align="left">Disable</th>
		<td>
			Remove <a href="https://developer.wordpress.org/reference/functions/update_option_new_admin_email/"><code>update_option_new_admin_email</code></a> action from <a href="https://developer.wordpress.org/reference/hooks/add_option_new_admin_email/"><code>add_option_new_admin_email</code></a> and <a href="https://developer.wordpress.org/reference/hooks/update_option_new_admin_email/"><code>update_option_new_admin_email</code></a> hooks
		</td>
	</tr>
</table>

### Site admin email address is changed

Sent when a user clicks the link in the email requesting confirmation of the change to the site admin email address (see above).

<table>
	<tr>
		<th valign="top" align="left">To</th>
		<td>Old site admin email address</td>
	</tr>
	<tr>
		<th valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th valign="top" align="left">Subject</th>
		<td>[%s] Admin Email Changed</td>
	</tr>
	<tr>
		<th valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/wp_site_admin_email_change_notification/"><code>wp_site_admin_email_change_notification()</code></a></td>
	</tr>
	<tr>
		<th valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/site_admin_email_change_email/"><code>site_admin_email_change_email</code></a><br>
		</td>
	</tr>
	<tr>
		<th valign="top" align="left">Disable</th>
		<td>
			Return false from <a href="https://developer.wordpress.org/reference/hooks/send_site_admin_email_change_email/"><code>send_site_admin_email_change_email</code></a> filter
		</td>
	</tr>
</table>

### Change of network admin email address is attempted

Multisite only. Sent when a user attempts to change the Network Admin Email option on the Network Settings screen.

<table>
	<tr>
		<th valign="top" align="left">To</th>
		<td>Proposed new email address</td>
	</tr>
	<tr>
		<th valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th valign="top" align="left">Subject</th>
		<td>[%s] Network Admin Email Change Request</td>
	</tr>
	<tr>
		<th valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/update_network_option_new_admin_email/"><code>update_network_option_new_admin_email()</code></a></td>
	</tr>
	<tr>
		<th valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/new_network_admin_email_content/"><code>new_network_admin_email_content</code></a><br>
		</td>
	</tr>
	<tr>
		<th valign="top" align="left">Disable</th>
		<td>
			Remove <a href="https://developer.wordpress.org/reference/functions/update_network_option_new_admin_email/"><code>update_network_option_new_admin_email</code></a> action from <a href="https://developer.wordpress.org/reference/hooks/add_site_option_new_admin_email/"><code>add_site_option_new_admin_email</code></a> and <a href="https://developer.wordpress.org/reference/hooks/update_site_option_new_admin_email/"><code>update_site_option_new_admin_email</code></a> hooks
		</td>
	</tr>
</table>

### Network admin email address is changed

Multisite only. Sent when a user clicks the link in the email requesting confirmation of the change to the network admin email (see above).

<table>
	<tr>
		<th valign="top" align="left">To</th>
		<td>Old network admin email address</td>
	</tr>
	<tr>
		<th valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th valign="top" align="left">Subject</th>
		<td>[%s] Notice of Network Admin Email Change</td>
	</tr>
	<tr>
		<th valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/wp_network_admin_email_change_notification/"><code>wp_network_admin_email_change_notification()</code></a></td>
	</tr>
	<tr>
		<th valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/network_admin_email_change_email/"><code>network_admin_email_change_email</code></a><br>
		</td>
	</tr>
	<tr>
		<th valign="top" align="left">Disable</th>
		<td>
			Return false from <a href="https://developer.wordpress.org/reference/hooks/send_network_admin_email_change_email/"><code>send_network_admin_email_change_email</code></a> filter
		</td>
	</tr>
</table>

## Change of User Email or Password

### User or Administrator requests a password reset

Sent when:

* A user clicks the "Lost your password?" link on the login screen and submits their email address
* An Administrator clicks the "Send password reset" link next to a user on the Users screen (WP 5.7+)
* An Administrator clicks the "Send Reset Link" from the user editing screen of another user (WP 5.7+)

<table>
	<tr>
		<th valign="top" align="left">To</th>
		<td>User</td>
	</tr>
	<tr>
		<th valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th valign="top" align="left">Subject</th>
		<td>[%s] Password Reset</td>
	</tr>
	<tr>
		<th valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/retrieve_password/"><code>retrieve_password()</code></a></td>
	</tr>
	<tr>
		<th valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/retrieve_password_title/"><code>retrieve_password_title</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/retrieve_password_message/"><code>retrieve_password_message</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/retrieve_password_notification_email/"><code>retrieve_password_notification_email</code></a> (WP 6.0+)<br>
		</td>
	</tr>
	<tr>
		<th valign="top" align="left">Disable</th>
		<td>
			- Return false from <a href="https://developer.wordpress.org/reference/hooks/send_retrieve_password_email/"><code>send_retrieve_password_email</code></a> filter (WP 6.0+)<br>
			- Return an empty message from <a href="https://developer.wordpress.org/reference/hooks/retrieve_password_message/"><code>retrieve_password_message</code></a> filter<br>
		</td>
	</tr>
</table>

### User resets their password

Sent when a user resets their password after clicking the confirmation link sent by the "Lost your password?" feature (see above).

<table>
	<tr>
		<th valign="top" align="left">To</th>
		<td>Site admin</td>
	</tr>
	<tr>
		<th valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th valign="top" align="left">Subject</th>
		<td>[%s] Password Changed</td>
	</tr>
	<tr>
		<th valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/wp_password_change_notification/"><code>wp_password_change_notification()</code></a></td>
	</tr>
	<tr>
		<th valign="top" align="left">Pluggable</th>
		<td>Yes</td>
	</tr>
	<tr>
		<th valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/wp_password_change_notification_email/"><code>wp_password_change_notification_email</code></a>
		</td>
	</tr>
	<tr>
		<th valign="top" align="left">Disable</th>
		<td>
			- Remove <a href="https://developer.wordpress.org/reference/functions/wp_password_change_notification/"><code>wp_password_change_notification</code></a> action from the <a href="https://developer.wordpress.org/reference/hooks/after_password_reset/"><code>after_password_reset</code></a> hook<br>
			- Overwrite the pluggable <a href="https://developer.wordpress.org/reference/functions/wp_password_change_notification/"><code>wp_password_change_notification()</code></a> function<br>
		</td>
	</tr>
</table>

### User changes their password

Sent when a logged in user changes their password from the user profile screen.

<table>
	<tr>
		<th valign="top" align="left">To</th>
		<td>User</td>
	</tr>
	<tr>
		<th valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th valign="top" align="left">Subject</th>
		<td>[%s] Password Changed</td>
	</tr>
	<tr>
		<th valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/wp_update_user/"><code>wp_update_user()</code></a></td>
	</tr>
	<tr>
		<th valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/password_change_email/"><code>password_change_email</code></a>
		</td>
	</tr>
	<tr>
		<th valign="top" align="left">Disable</th>
		<td>
			Return false from <a href="https://developer.wordpress.org/reference/hooks/send_password_change_email/"><code>send_password_change_email</code></a> filter
		</td>
	</tr>
</table>

### User attempts to change their email address

Sent when a logged in user attempts to change their email address from the user profile screen.

<table>
	<tr>
		<th valign="top" align="left">To</th>
		<td>Proposed new email address</td>
	</tr>
	<tr>
		<th valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th valign="top" align="left">Subject</th>
		<td>[%s] Email Change Request</td>
	</tr>
	<tr>
		<th valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/send_confirmation_on_profile_email/"><code>send_confirmation_on_profile_email()</code></a></td>
	</tr>
	<tr>
		<th valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/new_user_email_content/"><code>new_user_email_content</code></a>
		</td>
	</tr>
	<tr>
		<th valign="top" align="left">Disable</th>
		<td>
			Remove <a href="https://developer.wordpress.org/reference/functions/send_confirmation_on_profile_email/"><code>send_confirmation_on_profile_email()</code></a> action from <a href="https://developer.wordpress.org/reference/hooks/personal_options_update/"><code>personal_options_update</code></a> hook
		</td>
	</tr>
</table>

### User changes their email address

Sent when a user clicks the link in the email requesting confirmation of the change to their email address (see above).

<table>
	<tr>
		<th valign="top" align="left">To</th>
		<td>User</td>
	</tr>
	<tr>
		<th valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th valign="top" align="left">Subject</th>
		<td>[%s] Email Changed</td>
	</tr>
	<tr>
		<th valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/wp_update_user/"><code>wp_update_user()</code></a></td>
	</tr>
	<tr>
		<th valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/email_change_email/"><code>email_change_email</code></a>
		</td>
	</tr>
	<tr>
		<th valign="top" align="left">Disable</th>
		<td>
			Return false from <a href="https://developer.wordpress.org/reference/hooks/send_email_change_email/"><code>send_email_change_email</code></a> filter
		</td>
	</tr>
</table>

## Personal Data Requests

### Personal data export or erasure request is created or resent

Sent when:

* A request is created or resent from the Tools -> Export Personal Data screen
* A request is created or resent from the Tools -> Erase Personal Data screen

Details:

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
               user_request_confirmed_email_content (WP 5.8+)
               user_request_confirmed_email_headers
               user_confirmed_action_email_content (deprecated in 5.8)
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

Sent when:

* An administrator clicks the Erase Personal Data button next to a confirmed data erasure request
* An administrator clicks the Force Erase Personal Data button next to a data erasure request of any status

Details:

    To:        Requester email address
    From:      WordPress <wordpress@host>
    Subject:   [%s] Erasure Request Fulfilled
    Function:  _wp_privacy_send_erasure_fulfillment_notification()
    Pluggable: No
    Filters:   user_erasure_fulfillment_email_to
               user_erasure_fulfillment_email_subject (WP 5.8+)
               user_erasure_fulfillment_email_content (WP 5.8+)
               user_erasure_fulfillment_email_headers (WP 5.8+)
               user_erasure_complete_email_subject (deprecated in 5.8)
               user_confirmed_action_email_content (deprecated in 5.8)
               user_erasure_complete_email_headers (deprecated in 5.8)
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
    Subject:   New %1$s User: %2$s
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

Details:

    Function:  wp_new_user_notification()
    Pluggable: Yes
    Disable:   Remove wp_send_new_user_notifications action on register_new_user hook
               Remove wp_send_new_user_notifications action on edit_user_created_user hook
               Remove wp_send_new_user_notifications action on network_site_new_created_user hook
               Remove wp_send_new_user_notifications action on network_site_users_created_user hook
               Remove wp_send_new_user_notifications action on network_user_new_created_user hook
               Overwrite the pluggable wp_new_user_notification() function

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
    Disable:   Overwrite the pluggable wp_new_blog_notification() function
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

Copyright 2015 - 2022 John Blackbourn

This documentation is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either version 2 of the License, or
(at your option) any later version.

This documentation is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.
