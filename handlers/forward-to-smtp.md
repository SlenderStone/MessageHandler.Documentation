## Forward to smtp

Sends an email to an Smtp server in response to a message that satisfies a certain condition.

### Handler properties

**Trigger:** A condition that will trigger the handler to send an email via the smtp server, the trigger has to be created using Javascript syntax and must evaluate to a bool. You can access the message under consideration using the `msg` parameter. For example:

	msg.Temperature > 50

**Server:** The DNS name of the SMTP server.

**Port:** The port on which to send the email to the SMTP server.

**Username:** The username to use when authenticating to the SMTP server.

**Password:** The password to use when authenticating to the SMTP server.

**To:** (Optional) The email address to send the email to.

**From:** (Optional) The email address to send the email from.

**Subject:** (Optional) The subject to use in the email.

**Body:** (Optional) The body to use in the email.

### Message requirements

**Serializer:** JSON

**Format:** All fields are optional, if not provided the value from the config will be used.

	{
		To : string,
		From: string,
		Subject: string,
		Body: string
	}
