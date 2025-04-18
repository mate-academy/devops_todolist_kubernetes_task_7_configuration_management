# INSTRUCTION.md

## Applying the Changes

Follow these steps to apply the changes made in the project:

1. **Install Dependencies**  
   Ensure all necessary dependencies are installed. Run the following command:
   ```bash
   pip install -r requirements.txt
   ```

2. **Apply Migrations**  
   Generate and apply any new database migrations:
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

3. **Collect Static Files**  
   If any changes involve static files, collect them by running:
   ```bash
   python manage.py collectstatic
   ```

4. **Run Tests**  
   Execute the project's test suite to verify that all tests pass:
   ```bash
   python manage.py test
   ```

5. **Start the Server**  
   Launch the local development server to validate the changes in the browser:
   ```bash
   python manage.py runserver
   ```

---

## Validating the Changes

After applying the above steps, ensure the changes are correctly implemented by following these validation steps:

1. **Write and Run Tests**
   - Confirm that all test cases, including existing and any newly added ones, pass.
   - If necessary, write tests for new features or bug fixes.
   ```bash
   python manage.py test
   ```

2. **Manual Testing**
   - Open the application in a browser at `http://127.0.0.1:8000/`.
   - Navigate through the updated or added features and confirm they work as expected.
   - Verify that unchanged parts of the application still behave correctly.

3. **Lint and Code Quality Check**  
   Run a linter or code quality tool (if configured) to ensure code follows the project's style guidelines:
   ```bash
   flake8 .
   ```

4. **Check Logs for Errors**  
   During testing and while running the server, monitor the logs to ensure there are no new errors or warnings.

Once all the steps above are completed successfully, consider proceeding with deployment as per your project's
deployment workflow.