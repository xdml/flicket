# Changelog

If you are migrating from earlier version (since 0.2.1) you should ensure:

*   you upgrade the database:

    ```
    python manage.py db upgrade
    ```
    
*   Please read the changelog descriptions. Particularly those for 0.2.1.


## 0.2.1
*   now requires python =>3.6
*   **When upgrading from 0.2.0, follow:**

      * Do database backup according to your database engine.
      
      * Backup `migrations/` directory.
      
      * Clean `migrations/` directory:
       
        This can be done using git using the following command.

        ```
        git clean -fdx migrations
        ```
        
        Otherwise use another method to delete all migration files prior to merging this version. 
        The `migrations/versions` folder should only contain the following files:
        ```
        ../../flicket/migrations/versions/253ae54f5788_change_category_config_options.py
        ../../flicket/migrations/versions/36c91aa9b3b5_new_action_model.py
        ../../flicket/migrations/versions/70820003badd_add_logging_of_hours.py
        ../../flicket/migrations/versions/fe0f77ef3f46_migrations_before_source_code_control.py
        ../../flicket/migrations/versions/bcac6741b320_hours_scale_two_dp.py
        ```
           
        * Drop 'alembic_version' table:

        ```
        python manage.py shell
        >>> from application import db
        >>> db.engine.execute('DROP TABLE IF EXISTS alembic_version')
        <sqlalchemy.engine.result.ResultProxy object at 0x7fe5484034e0>
        >>> exit()
        ```

      * Stamp migration:

        ```
        python manage.py db stamp fe0f77ef3f46
        ```

      * Upgrade database:

        ```
        python manage.py db upgrade
        ```
        
      * update virtual env with latest module requirements
      
        ```
        pip install -r requirements.txt
        ```
      
      

*   Migration scripts now under git revision control.
*   New `FlicketAction` structure and updated `add_action()` function.
*   Moved to bootstrap 4.
*   Lots of UI changes as a result of change. Removed usage of html tables. 
*   Changed icon-set from glyphicons to font-awesome.
*   Fixed a number of Flash message rendering issues introduced during locale implementation.
*   Added carousel to front page showing all open high priority tickets.
*   Added pie charts to front page showing overall ticket status for each department. Removed tables showing same data.
*   Change category feature (user can put ticket into another "department / category").
*   Added time tracking. Users can now see total hours spent per ticket.
*   Can now login with username or email.
*   Person replying to ticket no longer receives an email. Other subscribers still do.


## 0.2.0
*   users can add subscriber other users to ticket so they receive notifications of tickets.
*   user can reset password.
*   when replying to ticket the priority level is set correctly. would previously always be set to low.
*   can sort tickets using column headers (xdml).
*   token expiration checked prior to login. fixes problems with auto-filling forms due to api authentication fails.
*   admin can send test email.
*   other minor tweaks and cosmetic changes. see commit history for more details.


## 0.1.9
*   Expanded API functionality ... still work to be done during this release
*   Fixed issue where field contents were not remembered after a search.
*   Added command to send emails to users who have tickets not closed. This needs to be invoked from the command line
    using `python manage.py email_outstanding_tickets` using a (weekly? don't spam your users too much!") cron job or 
    similar.
*   Fixed user not being able to edit own ticket (replies were OK).
*   The assignee can now close the ticket.
*   Ticket priority can now be changed during reply. Submitting reply and close will overwrite selection to closed.
*   Status changes now logged.
*   Started documentation.
*   Removed FAQ link from Flicket. Now controlled by documentation.
*   Added markdown help and removed link to external reference.
*   Documentation. 


## 0.1.8
*   Upgraded SQLAlchemy and Jinja2 due to security warnings.
*   Updated wording of prompts in 'populate_database_with_junk.py'.
*   Added admin setting so the page banner title can be changed from 'Flicket'.
*   Merged pull request: https://github.com/evereux/flicket/pull/20
*   Added ability to export tickets view as an excel file. Very project manager friendly, I believe.
*   Added authentication method for nt machines. Requires pywin32 to be installed.
    If Flicket is running on an NT (windows) machine and pywin32 is installed it will try to authenticate the user
    on that machine if they aren't already registered.
    I will add LDAP authentication at some point soon when I can find a means to test (OpenLDAP hasn't worked for me.)
*   Added a default group "super_user". super_users's can create departments and categories but can't access the 
    administration settings or delete topics or posts.

    If you are migrating from any earlier version you should ensure:
       * you upgrade the database. 
    ~~~
    python manage.py db migrate
    python manage.py db upgrade
    ~~~
    
        * add the user super_user to the groups in flicket_admin/groups/ groups page. You can use the admin 
        configuration area to do this.
    

## 0.1.7
*   Added view 'my_tickets'.
*   Added links to ticket views filtered by department on main page.
*   Refactored ticket views methods and placed in FlicketTicket model.
*   Moved ticket creation and editing from views to own class.
*   Total assigned now stored in users details and not calculated on the fly.

    If you are migrating from any earlier version you should ensure you upgrade
    the database. 
    ~~~
    python manage.py db migrate
    python manage.py db upgrade
    ~~~    
    
    Also, manually update your users total_posts count whilst site
    is offline. This can be done by running `python manage.py update_user_assigned`
*   Added missing showmarkdown toggle to reply form.
*   populate_database_with_junk.py now uses mimesis for random data generation.
*   Change README to rst format and various wording changes.
*   Added flask-babel support.
*   Added French locale option. Thanks to SolvingCurves.


## 0.1.6a
*   various bug fixes.
*   user can now change status when replying to ticket.

## 0.1.5a
*   Changed dependency from Flask-Misaka to the python library Markdown.
    This is because installing Flask-Misaka on Windows is too many hoops
    to jump through.

*   A number of package updates made to requirements. See requirements.txt.

*   The total number of posts is now stored in the user table instead of manually
    calculated. This is due to slow page loading times (users page) for large
    databases.
    
    If you are migrating from any earlier version you should ensure you upgrade
    the database. Also, manually update your users total_posts count whilst site
    is offline. This can be done by running `python manage.py update_user_posts`
    
 *  Some minor text updates to flash notifications.
