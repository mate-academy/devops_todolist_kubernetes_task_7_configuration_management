# INSTRUCTION.md

## Applying the Changes

Follow these steps to apply the changes made in the project:

1. **Install Dependencies**  
   Ensure you have all necessary dependencies installed. Run the following command to install them:
   ```bash
   pip install -r requirements.txt
   ```

2. **Apply Migrations**  
   Run the following commands to apply database migrations:
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

3. **Collect Static Files**  
   If static files are part of the changes, collect the static files by running:
   ```bash
   python manage.py collectstatic
   ```

4. **Run Tests**  
   Execute the tests to verify that everything is working as expected:
   ```bash
   python manage.py test
   ```

5. **Start the Development Server**  
   Run the development server to validate the changes locally:
   ```bash
   python manage.py runserver
   ```

6. **Check the Application in the Browser**  
   Open your browser and visit the application at:
   ```text
   http://127.0.0.1:8000/
   ```

---

## Validating the Changes

After following the steps above, validate the changes by:

1. **Running Automated Tests**  
   Ensure all existing and newly added tests are passing:
   ```bash
   python manage.py test
   ```

2. **Manual Testing**
    - Navigate through the affected parts of the application in the browser.
    - Verify that new features work as specified.
    - Check no regressions have occurred in unchanged features.

3. **Code Review**  
   Conduct a quick review of code changes to confirm adherence to coding standards and resolve any potential issues.

4. **Error Logs**  
   Monitor the logs during testing and running the application to ensure no new warnings or errors appear.

If all validation steps are successful, you can confidently deploy the changes to production. Follow your project's
deployment instructions for the next steps.