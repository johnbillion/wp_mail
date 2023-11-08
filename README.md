# WordPress Email Documentation

This document lists all the situations where WordPress core sends an email, how and when they happen, and how to filter or disable each one.

This list was last updated for WordPress 6.4.

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

<table>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
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
		<th scope="row" valign="top" align="left">Actions</th>
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
		<th scope="row" valign="top" align="left">To</th>
		<td>
			- Site Admin<br>
			- Post author, if they have the ability to edit the comment
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>[%1$s] Please moderate: "%2$s"</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/wp_notify_moderator/"><code>wp_notify_moderator()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>Yes</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/comment_moderation_recipients/"><code>comment_moderation_recipients</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/comment_moderation_subject/"><code>comment_moderation_subject</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/comment_moderation_text/"><code>comment_moderation_text</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/comment_moderation_headers/"><code>comment_moderation_headers</code></a><br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
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
		<th scope="row" valign="top" align="left">To</th>
		<td>Post author</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>
			[%1$s] Comment: "%2$s"<br>
			[%1$s] Pingback: "%2$s"<br>
			[%1$s] Trackback: "%2$s"<br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/wp_notify_postauthor/"><code>wp_notify_postauthor()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>Yes</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/comment_notification_recipients/"><code>comment_notification_recipients</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/comment_notification_subject/"><code>comment_notification_subject</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/comment_notification_text/"><code>comment_notification_text</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/comment_notification_headers/"><code>comment_notification_headers</code></a><br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
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
		<th scope="row" valign="top" align="left">To</th>
		<td>Proposed new email address</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>[%s] New Admin Email Address</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/update_option_new_admin_email/"><code>update_option_new_admin_email()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/new_admin_email_content/"><code>new_admin_email_content</code></a><br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>
			Remove <a href="https://developer.wordpress.org/reference/functions/update_option_new_admin_email/"><code>update_option_new_admin_email</code></a> action from <a href="https://developer.wordpress.org/reference/hooks/add_option_new_admin_email/"><code>add_option_new_admin_email</code></a> and <a href="https://developer.wordpress.org/reference/hooks/update_option_new_admin_email/"><code>update_option_new_admin_email</code></a> hooks
		</td>
	</tr>
</table>

### Site admin email address is changed

Sent when a user clicks the link in the email requesting confirmation of the change to the site admin email address (see above).

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>Old site admin email address</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>[%s] Admin Email Changed</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/wp_site_admin_email_change_notification/"><code>wp_site_admin_email_change_notification()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/site_admin_email_change_email/"><code>site_admin_email_change_email</code></a><br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>
			Return false from <a href="https://developer.wordpress.org/reference/hooks/send_site_admin_email_change_email/"><code>send_site_admin_email_change_email</code></a> filter
		</td>
	</tr>
</table>

### Change of network admin email address is attempted

Multisite only. Sent when a user attempts to change the Network Admin Email option on the Network Settings screen.

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>Proposed new email address</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>[%s] Network Admin Email Change Request</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/update_network_option_new_admin_email/"><code>update_network_option_new_admin_email()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/new_network_admin_email_content/"><code>new_network_admin_email_content</code></a><br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>
			Remove <a href="https://developer.wordpress.org/reference/functions/update_network_option_new_admin_email/"><code>update_network_option_new_admin_email</code></a> action from <a href="https://developer.wordpress.org/reference/hooks/add_site_option_new_admin_email/"><code>add_site_option_new_admin_email</code></a> and <a href="https://developer.wordpress.org/reference/hooks/update_site_option_new_admin_email/"><code>update_site_option_new_admin_email</code></a> hooks
		</td>
	</tr>
</table>

### Network admin email address is changed

Multisite only. Sent when a user clicks the link in the email requesting confirmation of the change to the network admin email (see above).

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>Old network admin email address</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>[%s] Notice of Network Admin Email Change</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/wp_network_admin_email_change_notification/"><code>wp_network_admin_email_change_notification()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/network_admin_email_change_email/"><code>network_admin_email_change_email</code></a><br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
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
		<th scope="row" valign="top" align="left">To</th>
		<td>User</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>[%s] Password Reset</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/retrieve_password/"><code>retrieve_password()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/retrieve_password_title/"><code>retrieve_password_title</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/retrieve_password_message/"><code>retrieve_password_message</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/retrieve_password_notification_email/"><code>retrieve_password_notification_email</code></a> (WP 6.0+)<br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
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
		<th scope="row" valign="top" align="left">To</th>
		<td>Site admin</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>[%s] Password Changed</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/wp_password_change_notification/"><code>wp_password_change_notification()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>Yes</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/wp_password_change_notification_email/"><code>wp_password_change_notification_email</code></a>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
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
		<th scope="row" valign="top" align="left">To</th>
		<td>User</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>[%s] Password Changed</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/wp_update_user/"><code>wp_update_user()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/password_change_email/"><code>password_change_email</code></a>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>
			Return false from <a href="https://developer.wordpress.org/reference/hooks/send_password_change_email/"><code>send_password_change_email</code></a> filter
		</td>
	</tr>
</table>

### User attempts to change their email address

Sent when a logged in user attempts to change their email address from the user profile screen.

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>Proposed new email address</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>[%s] Email Change Request</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/send_confirmation_on_profile_email/"><code>send_confirmation_on_profile_email()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/new_user_email_content/"><code>new_user_email_content</code></a>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>
			Remove <a href="https://developer.wordpress.org/reference/functions/send_confirmation_on_profile_email/"><code>send_confirmation_on_profile_email()</code></a> action from <a href="https://developer.wordpress.org/reference/hooks/personal_options_update/"><code>personal_options_update</code></a> hook
		</td>
	</tr>
</table>

### User changes their email address

Sent when a user clicks the link in the email requesting confirmation of the change to their email address (see above).

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>User</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>[%s] Email Changed</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/wp_update_user/"><code>wp_update_user()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/email_change_email/"><code>email_change_email</code></a>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
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

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>Requester email address</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>[%1$s] Confirm Action: %2$s</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/wp_send_user_request/"><code>wp_send_user_request()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/user_request_action_email_subject/"><code>user_request_action_email_subject</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/user_request_action_email_content/"><code>user_request_action_email_content</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/user_request_action_email_headers/"><code>user_request_action_email_headers</code></a><br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>
			Unknown, may have to remove the admin pages entirely by removing <code>_wp_privacy_hook_requests_page</code> action from <code>admin_menu</code> hook
		</td>
	</tr>
</table>

### User confirms personal data export or erasure request

Sent when a user clicks the link in the personal data export or erasure request confirmation email (see above).

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>
			- Site admin on a single site installation<br>
			- Network admin on a Multisite installation
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>[%1$s] Action Confirmed: %2$s</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/_wp_privacy_send_request_confirmation_notification/"><code>_wp_privacy_send_request_confirmation_notification()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/user_request_confirmed_email_to/"><code>user_request_confirmed_email_to</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/user_request_confirmed_email_subject/"><code>user_request_confirmed_email_subject</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/user_request_confirmed_email_content/"><code>user_request_confirmed_email_content</code></a> (WP 5.8+)<br>
			<a href="https://developer.wordpress.org/reference/hooks/user_request_confirmed_email_headers/"><code>user_request_confirmed_email_headers</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/user_confirmed_action_email_content/"><code>user_confirmed_action_email_content</code></a> (deprecated in 5.8)<br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>
			Remove <a href="https://developer.wordpress.org/reference/functions/_wp_privacy_send_request_confirmation_notification/"><code>_wp_privacy_send_request_confirmation_notification()</code></a> action from <a href="https://developer.wordpress.org/reference/hooks/user_request_action_confirmed/"><code>user_request_action_confirmed</code></a> hook
		</td>
	</tr>
</table>

### Site admin sends link to a personal data export

Sent when a site admin clicks the Send Export Link button next to a confirmed data export request.

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>Requester email address</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>[%s] Personal Data Export</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/wp_privacy_send_personal_data_export_email/"><code>wp_privacy_send_personal_data_export_email()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/wp_privacy_personal_data_email_to/"><code>wp_privacy_personal_data_email_to</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/wp_privacy_personal_data_email_subject/"><code>wp_privacy_personal_data_email_subject</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/wp_privacy_personal_data_email_content/"><code>wp_privacy_personal_data_email_content</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/wp_privacy_personal_data_email_headers/"><code>wp_privacy_personal_data_email_headers</code></a><br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>
			Remove <a href="https://developer.wordpress.org/reference/functions/wp_privacy_send_personal_data_export_email/"><code>wp_privacy_send_personal_data_export_email()</code></a> action from <a href="https://developer.wordpress.org/reference/hooks/wp_privacy_personal_data_export_page/"><code>wp_privacy_personal_data_export_page</code></a> hook
		</td>
	</tr>
</table>

### Site admin erases personal data to fulfill a data erasure request

Sent when:

* An administrator clicks the Erase Personal Data button next to a confirmed data erasure request
* An administrator clicks the Force Erase Personal Data button next to a data erasure request of any status

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>Requester email address</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>[%s] Erasure Request Fulfilled</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/_wp_privacy_send_erasure_fulfillment_notification/"><code>_wp_privacy_send_erasure_fulfillment_notification()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/user_erasure_fulfillment_email_to/"><code>user_erasure_fulfillment_email_to</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/user_erasure_fulfillment_email_subject/"><code>user_erasure_fulfillment_email_subject</code></a> (WP 5.8+)<br>
			<a href="https://developer.wordpress.org/reference/hooks/user_erasure_fulfillment_email_content/"><code>user_erasure_fulfillment_email_content</code></a> (WP 5.8+)<br>
			<a href="https://developer.wordpress.org/reference/hooks/user_erasure_fulfillment_email_headers/"><code>user_erasure_fulfillment_email_headers</code></a> (WP 5.8+)<br>
			<a href="https://developer.wordpress.org/reference/hooks/user_erasure_complete_email_subject/"><code>user_erasure_complete_email_subject</code></a> (deprecated in 5.8)<br>
			<a href="https://developer.wordpress.org/reference/hooks/user_confirmed_action_email_content/"><code>user_confirmed_action_email_content</code></a> (deprecated in 5.8)<br>
			<a href="https://developer.wordpress.org/reference/hooks/user_erasure_complete_email_headers/"><code>user_erasure_complete_email_headers</code></a> (deprecated in 5.8)<br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>
			Remove <a href="https://developer.wordpress.org/reference/functions/_wp_privacy_send_erasure_fulfillment_notification/"><code>_wp_privacy_send_erasure_fulfillment_notification()</code></a> action from <a href="https://developer.wordpress.org/reference/hooks/wp_privacy_personal_data_erased/"><code>wp_privacy_personal_data_erased</code></a> hook
		</td>
	</tr>
</table>

## Automatic Updates

### Automatic plugin or theme updates

Sent when a background automatic update to plugins and/or themes completes or fails.

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>Site admin on a single site installation<br>Network admin on a Multisite installation</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>
			[%s] Some plugins and themes have automatically updated<br>
			[%s] Some plugins were automatically updated<br>
			[%s] Some themes were automatically updated<br>
			[%s] Some plugins and themes have failed to update<br>
			[%s] Some plugins have failed to update<br>
			[%s] Some themes have failed to update<br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/classes/wp_automatic_updater/after_plugin_theme_update/"><code>WP_Automatic_Updater::after_plugin_theme_update()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/auto_plugin_theme_update_email/"><code>auto_plugin_theme_update_email</code></a><br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>
			- Return false from <a href="https://developer.wordpress.org/reference/hooks/auto_plugin_update_send_email/"><code>auto_plugin_update_send_email</code></a> filter<br>
			- Return false from <a href="https://developer.wordpress.org/reference/hooks/auto_theme_update_send_email/"><code>auto_theme_update_send_email</code></a> filter<br>
		</td>
	</tr>
</table>

### Automatic core update

Sent when a background automatic update to WordPress core completes or fails.

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>Site admin on a single site installation<br>Network admin on a Multisite installation</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>
			[%1$s] Your site has updated to WordPress %2$s<br>
			[%1$s] WordPress %2$s is available. Please update!<br>
			[%1$s] URGENT: Your site may be down due to a failed update<br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/classes/wp_automatic_updater/send_email/"><code>WP_Automatic_Updater::send_email()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/auto_core_update_email/"><code>auto_core_update_email</code></a><br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>
			- Return false from <a href="https://developer.wordpress.org/reference/hooks/auto_core_update_send_email/"><code>auto_core_update_send_email</code></a> filter<br>
			- Return false from <a href="https://developer.wordpress.org/reference/hooks/send_core_update_notification_email/"><code>send_core_update_notification_email</code></a> filter<br>
		</td>
	</tr>
</table>

### Full log of background update results

Only sent when you are using a development version of WordPress and it's not under version control.

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>Site admin on a single site installation<br>Network admin on a Multisite installation</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>
			[%s] Background Update Failed<br>
			[%s] Background Update Finished<br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/classes/wp_automatic_updater/send_debug_email/"><code>WP_Automatic_Updater::send_debug_email()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/auto_core_update_email/"><code>auto_core_update_email</code></a><br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>
			Return false from <a href="https://developer.wordpress.org/reference/hooks/automatic_updates_send_debug_email/"><code>automatic_updates_send_debug_email</code></a> filter<br>
		</td>
	</tr>
</table>

## New User

### An existing user is invited to a site

Multisite only. Sent when an existing user is added to a site from Users -> Add New -> Add Existing User.

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>User being invited</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>[%s] Joining Confirmation</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td>wp-admin/user-new.php</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/invited_user_email/"><code>invited_user_email</code></a><br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>Click the "Skip Confirmation Email" checkbox when adding the user</td>
	</tr>
</table>

### A new user is invited to join a site

Multisite only. Sent when a new user is invited to join a site from Users -> Add New -> Add New User.

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>User being invited</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>[Network Name] &lt;[network admin]&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>[%1$s] Activate %2$s</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/wpmu_signup_user_notification/"><code>wpmu_signup_user_notification()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/wpmu_signup_user_notification_subject/"><code>wpmu_signup_user_notification_subject</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/wpmu_signup_user_notification_email/"><code>wpmu_signup_user_notification_email</code></a><br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>
			- Click the "Skip Confirmation Email" checkbox when adding the user<br>
			- Return false from <a href="https://developer.wordpress.org/reference/hooks/wpmu_signup_user_notification/"><code>wpmu_signup_user_notification</code></a> filter<br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Note</th>
		<td>There is a default filter attached to <a href="https://developer.wordpress.org/reference/hooks/wpmu_signup_user_notification_email/"><code>wpmu_signup_user_notification_email</code></a>: <a href="https://developer.wordpress.org/reference/functions/admin_created_user_email/"><code>admin_created_user_email()</code></a></td>
	</tr>
</table>

### A new user account is created

Multisite only. Sent when a new user account is created via `wpmu_create_user()`:

* From Network Admin -> Sites -> Add New and the email address doesn't already exist
* From Network Admin -> Sites -> [Edit] -> Users -> Add New User
* From Network Admin -> Users -> Add New
* From Users -> Add New -> Add New User and the "Skip Confirmation Email" checkbox is checked
* When a user activates their new account on `wp-activate.php`
* Via a REST API request to create a new user (`POST` to `/wp/v2/users`)

Details:

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>Network Admin</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>New User Registration: %s</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/newuser_notify_siteadmin/"><code>newuser_notify_siteadmin()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/newuser_notify_siteadmin/"><code>newuser_notify_siteadmin</code></a><br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>
			- Filter the <code>registrationnotification</code> option value<br>
			- Remove action from the <a href="https://developer.wordpress.org/reference/hooks/wpmu_new_user/"><code>wpmu_new_user</code></a> hook<br>
			- Toggle "Registration notification" in Network Admin -> Settings<br>
		</td>
	</tr>
</table>

### A user is added, or their account activation is successful

Multisite only.

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>User being added</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>[Network Name] &lt;[network admin]&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>New %1$s User: %2$s</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/wpmu_welcome_user_notification/"><code>wpmu_welcome_user_notification()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/update_welcome_user_subject/"><code>update_welcome_user_subject</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/update_welcome_user_email/"><code>update_welcome_user_email</code></a><br>
			See also the "Welcome User Email" setting in Network Admin -> Settings<br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>
			- Click the "Skip Confirmation Email" checkbox when adding the user<br>
			- Return false from the <a href="https://developer.wordpress.org/reference/hooks/wpmu_welcome_user_notification/"><code>wpmu_welcome_user_notification</code></a> filter<br>
			- Remove action from the <a href="https://developer.wordpress.org/reference/hooks/wpmu_activate_user/"><code>wpmu_activate_user</code></a> hook<br>
		</td>
	</tr>
</table>

### A new user is created

When a new user is created, two emails are sent from the same function. One to the site admin:

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>Site Admin</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>[%s] New User Registration</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/wp_new_user_notification_email_admin/"><code>wp_new_user_notification_email_admin</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/wp_send_new_user_notification_to_admin/"><code>wp_send_new_user_notification_to_admin</code></a> (WP 6.1+)<br>
		</td>
	</tr>
</table>

and one to the newly created user:

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>New user</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>[%s] Login Details</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/wp_new_user_notification_email/"><code>wp_new_user_notification_email</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/wp_send_new_user_notification_to_user/"><code>wp_send_new_user_notification_to_user</code></a> (WP 6.1+)<br>
		</td>
	</tr>
</table>

Details:

<table>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/wp_new_user_notification/"><code>wp_new_user_notification()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>Yes</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>
			- Return false from the <a href="https://developer.wordpress.org/reference/hooks/wp_send_new_user_notification_to_admin/"><code>wp_send_new_user_notification_to_admin</code></a> or <a href="https://developer.wordpress.org/reference/hooks/wp_send_new_user_notification_to_user/"><code>wp_send_new_user_notification_to_user</code></a> filter (WP 6.1+)<br>
			- Remove <a href="https://developer.wordpress.org/reference/functions/wp_send_new_user_notifications/"><code>wp_send_new_user_notifications()</code></a> action from the <a href="https://developer.wordpress.org/reference/hooks/register_new_user/"><code>register_new_user</code></a> hook<br>
			- Remove <a href="https://developer.wordpress.org/reference/functions/wp_send_new_user_notifications/"><code>wp_send_new_user_notifications()</code></a> action from the <a href="https://developer.wordpress.org/reference/hooks/edit_user_created_user/"><code>edit_user_created_user</code></a> hook<br>
			- Remove <a href="https://developer.wordpress.org/reference/functions/wp_send_new_user_notifications/"><code>wp_send_new_user_notifications()</code></a> action from the <a href="https://developer.wordpress.org/reference/hooks/network_site_new_created_user/"><code>network_site_new_created_user</code></a> hook<br>
			- Remove <a href="https://developer.wordpress.org/reference/functions/wp_send_new_user_notifications/"><code>wp_send_new_user_notifications()</code></a> action from the <a href="https://developer.wordpress.org/reference/hooks/network_site_users_created_user/"><code>network_site_users_created_user</code></a> hook<br>
			- Remove <a href="https://developer.wordpress.org/reference/functions/wp_send_new_user_notifications/"><code>wp_send_new_user_notifications()</code></a> action from the <a href="https://developer.wordpress.org/reference/hooks/network_user_new_created_user/"><code>network_user_new_created_user</code></a> hook<br>
			- Overwrite the pluggable <a href="https://developer.wordpress.org/reference/functions/wp_new_user_notification/"><code>wp_new_user_notification()</code></a> function
		</td>
	</tr>
</table>

## New Site

### A new site is created

Multisite only. Sent when a new site is created from Network Admin -> Sites -> Add New.

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>Network Admin</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>Site Admin &lt;[network admin]&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>[%s] New Site Created</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/wpmu_new_site_admin_notification/"><code>wpmu_new_site_admin_notification()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/new_site_email/"><code>new_site_email</code></a><br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>
			Return false from the <a href="https://developer.wordpress.org/reference/hooks/send_new_site_email/"><code>send_new_site_email</code></a> filter<br>
		</td>
	</tr>
</table>

### User registers for a new site

Multisite only, with site registration allowed. Sent when a visitor registers a new user account and site from wp-signup.php.

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>Site Admin</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>[Network Name] &lt;[network admin]&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>[%1$s] Activate %2$s</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/wpmu_signup_blog_notification/"><code>wpmu_signup_blog_notification()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/wpmu_signup_blog_notification_subject/"><code>wpmu_signup_blog_notification_subject</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/wpmu_signup_blog_notification_email/"><code>wpmu_signup_blog_notification_email</code></a><br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>
			- Return false from the <a href="https://developer.wordpress.org/reference/hooks/wpmu_signup_blog_notification/"><code>wpmu_signup_blog_notification</code></a> filter<br>
			- Remove <a href="https://developer.wordpress.org/reference/functions/wpmu_signup_blog_notification/"><code>wpmu_signup_blog_notification()</code></a> action from the <a href="https://developer.wordpress.org/reference/hooks/after_signup_site/"><code>after_signup_site</code></a> hook<br>
		</td>
	</tr>
</table>

### User activates their new site, or site added from Network Admin -> Sites -> Add New

Multisite only.

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>Network Admin</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>New Site Registration: %s</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/newblog_notify_siteadmin/"><code>newblog_notify_siteadmin()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/newblog_notify_siteadmin/"><code>newblog_notify_siteadmin</code></a><br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>
			- Filter the <code>registrationnotification</code> option value
			- Change the "Registration notification" setting in Network Admin -> Settings
			- Remove <a href="https://developer.wordpress.org/reference/functions/newblog_notify_siteadmin/"><code>newblog_notify_siteadmin()</code></a> action from the <a href="https://developer.wordpress.org/reference/hooks/wpmu_new_blog/"><code>wpmu_new_blog</code></a> hook<br>
			- Remove <a href="https://developer.wordpress.org/reference/functions/newblog_notify_siteadmin/"><code>newblog_notify_siteadmin()</code></a> action from the <a href="https://developer.wordpress.org/reference/hooks/wp_initialize_site/"><code>wp_initialize_site</code></a> hook<br>
		</td>
	</tr>
</table>

### User activates their new site, or site added from Network Admin -> Sites -> Add New

Multisite only.

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>New Site Admin</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>[Network Name] &lt;[network admin]&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>New %1$s Site: %2$s</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/wpmu_welcome_notification/"><code>wpmu_welcome_notification()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/update_welcome_subject/"><code>update_welcome_subject</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/update_welcome_email/"><code>update_welcome_email</code></a><br>
			See also the "Welcome Email" setting in Network Admin -> Settings<br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>
			- Return false from the <a href="https://developer.wordpress.org/reference/hooks/wpmu_welcome_notification/"><code>wpmu_welcome_notification</code></a> filter<br>
			- Remove <a href="https://developer.wordpress.org/reference/functions/wpmu_welcome_notification/"><code>wpmu_welcome_notification()</code></a> action from the <a href="https://developer.wordpress.org/reference/hooks/wpmu_activate_blog/"><code>wpmu_activate_blog</code></a> hook<br>
		</td>
	</tr>
</table>

## Other

### Installation

Sent when WordPress is initially installed.

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>Site Admin</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>New WordPress Site</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/wp_new_blog_notification/"><code>wp_new_blog_notification()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>yes</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/wp_installed_email/"><code>wp_installed_email</code></a><br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>
			Overwrite the pluggable <a href="https://developer.wordpress.org/reference/functions/wp_new_blog_notification/"><code>wp_new_blog_notification()</code></a> function
		</td>
	</tr>
</table>

### A fatal error occurs

Sent when a fatal error occurs in a plugin or theme and Recovery Mode is not active.

**Important:** The `wp_fatal_error_handler_enabled` filter cannot be used by plugins as it runs too early. [Information about using this filter can be found here](https://core.trac.wordpress.org/browser/trunk/src/wp-includes/error-protection.php?rev=49489&marks=114-131#L97).

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>Site Admin / Value of RECOVERY_MODE_EMAIL constant</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>[%s] Your Site is Experiencing a Technical Issue</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Function</th>
		<td><a href="https://developer.wordpress.org/reference/functions/send_recovery_mode_email/"><code>send_recovery_mode_email()</code></a></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/wp_fatal_error_handler_enabled/"><code>wp_fatal_error_handler_enabled</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/recovery_email_support_info/"><code>recovery_email_support_info</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/recovery_email_debug_info/"><code>recovery_email_debug_info</code></a><br>
			<a href="https://developer.wordpress.org/reference/hooks/recovery_mode_email/"><code>recovery_mode_email</code></a><br>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>
			Define <code>WP_DISABLE_FATAL_ERROR_HANDLER</code> as <code>true</code><br>
			Return <code>false</code> from the <a href="https://developer.wordpress.org/reference/hooks/wp_fatal_error_handler_enabled/"><code>wp_fatal_error_handler_enabled</code></a> filter
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Note</th>
		<td>
			Does not currently apply to Multisite
		</td>
	</tr>
</table>

### Site admin requests to delete site

Multisite only. Sent when an Administrator requests to delete their site from the Tools -> Delete Site screen.

<table>
	<tr>
		<th scope="row" valign="top" align="left">To</th>
		<td>Site Admin</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">From</th>
		<td>WordPress &lt;wordpress@host&gt;</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Subject</th>
		<td>[%s] Delete My Site</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">File</th>
		<td><code>wp-admin/ms-delete-site.php</code></td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Pluggable</th>
		<td>No</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Filters</th>
		<td>
			<a href="https://developer.wordpress.org/reference/hooks/delete_site_email_content/"><code>delete_site_email_content</code></a>
		</td>
	</tr>
	<tr>
		<th scope="row" valign="top" align="left">Disable</th>
		<td>
			Not possible
		</td>
	</tr>
</table>

## License: GPLv2

Copyright 2015 - 2023 John Blackbourn

This documentation is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either version 2 of the License, or
(at your option) any later version.

This documentation is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.
