# Hide password reset link from login page
## Purpose of this recipe
The password reset link in the login page of your AppGini apps allows users to reset their password if they forget it.
It's a convenient self-service way to create a new password without having the admin do it for users. But sometimes
you might want to disable this feature and have users contact the admin to reset their passwords,
which is what we'll this recipe explains.

## Files to edit
Add this code to `hooks/footer-extras.php`:

```
<?php $Translation['forgot password'] = 'Forgot your password? Please contact the admin!'; ?>
```
The above code would replace the password reset line with the string above. You could change that
to whatever message you wish to display, or just use an empty string to hide it all together:

```
<?php $Translation['forgot password'] = ''; ?>
```

## Files to delete
Now, the above would remove the link to the password reset page, but it won't remove the password reset behavior from your app.
If a user knows the link to the password reset page, they can still access it. So, to remove it altogether, you should
remove the file `membership_passwordReset.php`. Please also remember to remove it if you regenerate your app later.
