Week 6 Project: Authentication and Login Throttling
Due: unspecified at 11:59pm

Overview: Add a login page for user authentication to the staff area of a content management system, using best practices and security precautions. Then add a login throttling feature which will prevent login attempts for a period of time after too many login failures have occurred.

Walkthrough:  Video

Submitting Assignments: Submitted through Github, check out the Submitting Assignments page for more details.

Be sure to include a README containing a GIF walkthrough of your app and list of completed stories.
Use this README template in order to have a complete README.
Assets: The following assets may be helpful in completing this assignment.

 Assignment 6 Starting Database
 Assignment 6 Starting Code
User Stories:

User stories are a way to describe the features of a web application from the user's point of view.

The following user stories must be completed:

1. On the existing pages "public/staff/users/new.php" and "public/staff/users/edit.php", a user should see:

A form which includes a text input for "Password" and "Confirm Password" which do not display the password in plain text as it is being typed. (Tips)

Text which reads "Passwords should be at least 12 characters and include at least one uppercase letter, lowercase letter, number, and symbol."

2. For both users/new.php and users/edit.php, submitting the form performs data validations: (Tips)

Returns an error if password or confirm_password are blank.

Returns an error if password and confirm_password do not match.

Returns an error if password is not at least 12 characters long.

Returns an error if password does not contain at least one of each: uppercase letter, lowercase letter, letter, symbol.

Returns any errors related to other validations already on the user.

3. If all validations on the user data pass:

It encrypts the password with PHP's password_hash() function. (Tips)

It stores the password in the database as users.hashed_password in the same SQL statement which creates/updates the user record. (Tips)

4. Notice that login and logout pages already exist ("public/staff/login.php" and "public/staff/logout.php") and have the CSRF tokens and other protections added last week.

Currently, the login page is using a $master_password variable so that all user passwords are the same password (which is terrible security). Remove it from the code. Then use PHP's password_verify() function to test the entered password against the password stored in users.hashed_password for the username provided. (Tips)

Ensure the login page does not display content which would create a User Enumeration weakness. (Tips)

5. If a user fails to log in:

Record the failed login in "failed_logins" for the first 5 attempts. (Tips)

After 5 failed attempts, their next attempt should return a message which says "Too many failed logins for this username. You will need to wait X minutes before attempting another login." X should be the number of minutes remaining in the lockout period and start at 5 minutes. (Tips)

If users try again during the lockout period—for example, one minute later—the message will decrease the number of minutes remaining. It will always round fractional minutes upward. (Because rounding either way would make it say "0 minutes" when there were 29 seconds left.) (Tips)

After a lockout period has past, the next attempt (successful or not) will reset the failed_logins.count to 0.

6. After any successful login:

Set the failed_logins.count for the username to 0.
7. Watch out for SQLI Injection and Cross-Site Scripting vulnerabilities. There should not be any existing vulnerabilities in the code. Make sure you did not introduce any vulnerabilities while accomplishing the previous objectives. (Tips)

The following advanced user stories are optional:

Bonus 1: Even if all of the page content and error messages are exactly the same, the login page does have a subtle Username Enumeration weakness. Identify the weakness and write a short description of how the code could be modified to be more secure. Include your answer in the README file that accompanies your assignment submission. (Tips)

Bonus 2: On "public/staff/users/edit.php", it is no longer possible to edit a user without also editing their password because the password input cannot be blank. Instead, modify the code in validate_user() to only run the password validations if the password is not blank. Next, modify update_user so that it only encrypts the password and updates users.hashed_password with the result if the password is not blank. In other words, a blank password will still allow updating other user values and not touch the existing password, but providing a password will validate and update the password too.

Bonus 3: password_hash() allows configuration options to be a passed in as arguments. Use the options to set the bcrypt "cost" parameter to 11 (the default is 10). Be sure to change it for both the insert and update actions. Before you make the change, create a new user using the default cost (10). After you make the change, try to login as that user again. It will still succeed even though a hash with cost 10 and cost 11 return different values. How is this possible? Include your answer in the README file that accompanies your assignment submission. (Tips)

Bonus 4: On "public/staff/users/edit.php", add a new text field for "Previous password". It should not display the password in plain text as it is being typed. When the form is submitted, validate that the correct password has been provided before allowing the password to be updated. If not, it should return the validation message: "Previous password is incorrect". If you also completed Bonus Objective 2, then it should only require the correct previous password if the new password is being updated.

Advanced 1: Pretend that password_hash() and password_verify() do not exist. (They were added to PHP in 2013.) Implement these same functions yourself using the PHP function crypt() and the bcrypt hash algorithm. Name your versions my_password_hash() and my_password_verify() and include them in "private/auth_functions.php". Be sure to include a custom salt value of 22 characters like the PHP functions do.

Advanced 2:: Write a PHP function in "private/auth_functions.php" called generate_strong_password() which will generate a random strong password containing the number of characters specified by a function argument. On the new and edit user pages add the text: "Strong password suggestion: ". Then use your function to create a strong password with 12 characters. Every time the page is reloaded the suggestion should change.
