# login-page
@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        username = request.form['username']
        password = request.form['password']
        with sqlite3.connect("database.db") as conn:
            user = conn.execute("SELECT * FROM users WHERE username = ? AND password = ?", (username, password)).fetchone()
        if user:
            session['user_id'] = user[0]
            session['role'] = user[4]
            session['name'] = user[1]
            if user[4] == 'admin':
                return redirect(url_for('add_product'))
            else:
                return redirect(url_for('products'))
        else:
            flash("Invalid username or password", "danger")
    return render_template('login.html')
