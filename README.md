"This is the Practice Project for INFO 530."
from flask import Flask, render_template, request, redirect, url_for, flash, session

app = Flask(__name__)
app.secret_key = 'your_secret_key'

# Hardcoded username and password for demonstration purposes
USER_CREDENTIALS = {
    "username": "admin",
    "password": "password123"
}

# Login route to display login form and handle login logic
@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        username = request.form['username']
        password = request.form['password']
        
        # Check if credentials are valid
        if username == USER_CREDENTIALS['username'] and password == USER_CREDENTIALS['password']:
            session['username'] = username  # Store username in session
            flash('Login successful!', 'success')
            return redirect(url_for('dashboard'))
        else:
            flash('Invalid username or password. Please try again.', 'danger')
            return render_template('login.html')

    return render_template('login.html')

# Dashboard route (protected page)
@app.route('/dashboard')
def dashboard():
    if 'username' in session:
        return f"Welcome, {session['username']}! You are logged in."
    else:
        flash('You must be logged in to view the dashboard.', 'warning')
        return redirect(url_for('login'))

# Logout route to clear session
@app.route('/logout')
def logout():
    session.pop('username', None)
    flash('You have been logged out.', 'info')
    return redirect(url_for('login'))

if __name__ == '__main__':
    app.run(debug=True)

