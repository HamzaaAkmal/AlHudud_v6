# AttendPro Ultimate - Attendance Management System

Assalamu Alaikum! Yeh README file aapko "AttendPro Ultimate" project ko samajhne mein madad karegi. Yeh ek student attendance management system hai.

## Project Ka Maqsad (Project Purpose)

Is project ka bunyadi maqsad students ki hazri (attendance) ko asaan tareeqe se manage karna hai. Teachers ya admins is system ko istemal karke:
*   Naye students ko register kar sakte hain.
*   Mojooda students ki maloomat (information) ko update ya delete kar sakte hain.
*   Rozana (daily) basis par students ki hazri mark kar sakte hain (Present/Absent).
*   Attendance ki reports dekh sakte hain, jese ke kisi khaas din ki report ya kisi ek student ki poori attendance history.
*   Reports ko CSV file mein download bhi kar sakte hain.

Yeh system manual attendance lene ke kaam ko kam karta hai aur data ko organize rakhta hai.

## Istemaal Shuda Tools aur Libraries (Tools and Libraries Used)

Is project ko banane ke liye darj zeel (following) tools aur Python libraries istemal ki gayi hain:

1.  **Python:** Yeh programming language hai jis mein saara code likha gaya hai.
2.  **Streamlit:** Yeh ek bohot zabardast Python library hai. Iski madad se hum web applications (jese yeh attendance system) bohot aasani se aur jaldi bana sakte hain, bina zyada web designing ki details mein jaye. Yeh front-end (jo user ko nazar ata hai) banane ke kaam ati hai.
3.  **Pandas:** Yeh library data ko manage aur analyze karne ke liye istemal hoti hai. Humne isko students aur attendance ke data ko tables ki shakal (DataFrames) mein dikhane aur CSV files banane ke liye use kiya hai.
4.  **hashlib:** Yeh Python ki apni library hai. Isko passwords ko secure tareeqe se store karne ke liye istemal kiya gaya hai. Yeh passwords ko hash (ek qisam ki encryption) kar deti hai takay woh seedhe-seedhe na parhe ja saken.
5.  **os:** Yeh library operating system ke sath interact karne mein madad karti hai, jese yeh check karna ke koi file mojood hai ya nahi.
6.  **datetime:** Yeh Python ki apni library hai jo tareekhon (dates) aur waqt (time) ke sath kaam karne mein madad karti hai.

## Project Ka Structure aur Files Ki Tafseel (Project Structure and File Details)

Project mein yeh अहम (important) files hain:

### 1. `app.py`

*   **Role (Kirdar):** Yeh project ki main file hai. Jab aap application run karte hain, to sab se pehle yeh file chalti hai. Yeh "Dil" (heart) hai is application ka.
*   **Code Ka Basic Logic (Kaam Karne Ka Tareeqa):**
    1.  **Imports:** Sab se pehle, yeh zaroori libraries (Streamlit, AuthManager, DataManager, waghera) ko load karta hai.
    2.  **Page Configuration:** `st.set_page_config()` se application ka title ("AttendPro Ultimate") aur icon (👑) set hota hai.
    3.  **Managers ko Banana:** `AuthManager` (authentication ke liye) aur `DataManager` (data handling ke liye) ke objects banata hai.
    4.  **Session State:** `st.session_state` ka istemal karta hai. Yeh ek khaas jaga hai jahan Streamlit app ke run hone ke doran information store kar sakta hai, jese ke:
        *   `logged_in`: User login hai ya nahi (True/False).
        *   `username`: Login hue user ka naam.
        Inki initial values set ki jati hain agar yeh pehle se mojood na hon.
    5.  **`main_dashboard()` Function:**
        *   Yeh function tab chalta hai jab user `logged_in` hota hai.
        *   Screen par "AttendancePro " title dikhata hai.
        *   Left side par (sidebar mein) logged-in user ka naam aur "Logout" ka button dikhata hai. Logout button dabane par `logged_in` ko `False` kar deta hai aur app `rerun` (dobara shuru) hojati hai.
        *   Chaar (4) tabs banata hai: "👑 Admin Dashboard", "👥 Manage Students", "✍️ Mark Attendance", "🗓️ Daily Reports".
        *   Har tab ke andar content dikhane ke liye `ui_pages.py` file mein mojood functions (`show_admin_dashboard_tab`, waghera) ko call karta hai, aur unko `data_manager` ka object pass karta hai takay woh data access kar saken.
    6.  **`login_signup_page()` Function:**
        *   Yeh function tab chalta hai jab user `logged_in` nahi hota.
        *   User ko "Login" ya "Sign Up" karne ka option deta hai (ek dropdown menu ke zariye).
        *   **Agar "Login" chuna:**
            *   Ek form dikhata hai jismein username aur password poocha jata hai.
            *   "Login" button dabane par, `auth.login_user()` function (jo `auth_manager.py` mein hai) ko call karke check karta hai ke username aur password sahi hain ya nahi.
            *   Agar sahi hain, to `st.session_state['logged_in']` ko `True` aur `st.session_state['username']` ko user ke naam par set karta hai, aur app `rerun` hojati hai (jis se `main_dashboard` khul jayega).
            *   Ghalat hone par error message dikhata hai.
        *   **Agar "Sign Up" chuna:**
            *   Ek form dikhata hai jismein naya username, password aur confirm password poocha jata hai.
            *   "Sign Up" button dabane par, check karta hai ke username/password khali na hon aur dono passwords match karte hon.
            *   Phir `auth.register_user()` function (jo `auth_manager.py` mein hai) ko call karke naya user register karta hai.
            *   Kamyab registration par success message, warna error (jese username pehle se mojood hai) dikhata hai.
    7.  **Router (Main Logic Flow):**
        *   File ke akhir mein, yeh check karta hai ke `st.session_state['logged_in']` `True` hai ya `False`.
        *   Agar `True` hai, to `main_dashboard()` call hota hai.
        *   Agar `False` hai, to `login_signup_page()` call hota hai.

### 2. `auth_manager.py`

*   **Role (Kirdar):** Is file ka kaam users ke accounts ko manage karna hai – naye users banana (registration) aur purane users ko login karwana. Passwords ko mehfooz (secure) tareeqe se store karna iski zimmedari hai.
*   **Code Ka Basic Logic (Kaam Karne Ka Tareeqa):**
    1.  **`AuthManager` Class:** Ek class banai gayi hai jiske andar authentication se related saare functions hain.
    2.  **`__init__()` (Constructor):**
        *   Jab `AuthManager` ka object banta hai (`app.py` mein `auth = AuthManager()`), yeh function chalta hai.
        *   Yeh `auth.txt` file ka naam set karta hai. Isi file mein users ka data (username, hashed password) store hota hai.
        *   `_load_users()`: `auth.txt` se users ka data load karta hai.
        *   `_ensure_default_admin()`: Check karta hai ke "admin" naam ka user mojood hai ya nahi. Agar nahi, to ek default admin user "admin" password "admin123" ke sath bana deta hai. Yeh khas taur par pehli dafa app chalane ke liye hota hai.
    3.  **`_hash_password(password)`:**
        *   Yeh function password ko SHA-256 algorithm istemal karke hash (ek qisam ka code) mein badal deta hai. Hash kiye hue password se asal password maloom karna bohot mushkil hota hai. Is se passwords secure rehte hain.
    4.  **`_load_users()`:**
        *   `auth.txt` file ko kholta hai.
        *   Har line se username aur uske sath store shuda hashed password ko parhta hai.
        *   Inko ek dictionary (jese ek list jismein har cheez ka ek naam ho) mein store karta hai, jahan username key hota hai aur hashed password value.
    5.  **`_save_users()`:**
        *   Users ki dictionary (jo abhi memory mein hai) ko wapis `auth.txt` file mein save kar deta hai. Har username aur uska hashed password ek nayi line par `username,hashed_password` format mein likha jata hai.
    6.  **`register_user(username, password)`:**
        *   Naya user register karne ke liye.
        *   Pehle check karta hai ke diya gaya username pehle se `users` dictionary mein mojood to nahi.
        *   Agar nahi hai, to password ko `_hash_password()` se hash karta hai aur naye user ko `users` dictionary mein add karke `_save_users()` call karta hai. `True` return karta hai.
        *   Agar username pehle se hai, to `False` return karta hai.
    7.  **`login_user(username, password)`:**
        *   User ko login karwane ke liye.
        *   Diye gaye username ka hashed password `users` dictionary se nikalta hai.
        *   Agar username mojood hai, to diye gaye password ko bhi `_hash_password()` se hash karta hai.
        *   Phir dono hashed passwords (ek jo file se aya aur ek jo abhi banaya) ko compare karta hai.
        *   Agar dono same hain, to `True` (login kamyab) return karta hai, warna `False`.

### 3. `data_manager.py`

*   **Role (Kirdar):** Yeh file students ke data aur unki attendance ke data ko manage karti hai. Data ko files (`students.txt`, `attendance.txt`) mein save karna aur wahan se parhna iski zimmedari hai. Reports ke liye data tayyar karna bhi iska kaam hai.
*   **Code Ka Basic Logic (Kaam Karne Ka Tareeqa):**
    1.  **`DataManager` Class:** Ek class banai gayi hai jo data management ke saare kaam karti hai.
    2.  **`__init__()` (Constructor):**
        *   Jab `DataManager` ka object banta hai (`app.py` mein `data_manager = DataManager()`), yeh function chalta hai.
        *   `students.txt` (students ka data) aur `attendance.txt` (attendance ka data) files ke naam set karta hai.
        *   `_load_students()`: `students.txt` se students ka data load karke `self.students` dictionary mein rakhta hai.
    3.  **Student Methods (Students Se Mutalliq Functions):**
        *   `_load_students()`: `students.txt` file se data parhta hai (format: `student_id,name`) aur ek dictionary banata hai jahan student ID key aur naam value hota hai.
        *   `_save_students()`: `self.students` dictionary ke data ko `students.txt` file mein wapis save karta hai.
        *   `_get_next_student_id()`: Naya student add karte waqt usko ek unique ID dene ke liye, yeh function sab se bade mojooda ID mein 1 jama karke naya ID return karta hai.
        *   `add_student(name)`: Naya student add karta hai. Uska naam `self.students` dictionary mein naye ID ke sath save karta hai aur phir `_save_students()` call karta hai.
        *   `update_student(student_id, new_name)`: Kisi student ka naam uske ID se update karta hai.
        *   `delete_student(student_id)`: Student ko `self.students` dictionary se delete karta hai aur uski attendance records ko bhi `_cleanup_attendance_for_student()` se delete karwata hai.
        *   `get_students_as_df()`: `self.students` dictionary ko Pandas DataFrame (table jaisa) mein convert karke return karta hai. UI mein dikhane ke liye.
    4.  **Attendance Methods (Attendance Se Mutalliq Functions):**
        *   `mark_attendance(student_id, date_obj, status)`: Kisi student ki (student_id), kisi tareekh par (date_obj), kya hazri thi (status: "Present" ya "Absent"), yeh record karta hai.
            *   Yeh data `attendance.txt` file mein `student_id,YYYY-MM-DD,status` format mein save hota hai.
            *   Agar us student ki us date ki entry pehle se hai, to usko update karta hai, warna nayi entry daal deta hai.
        *   `get_all_attendance_records()`: `attendance.txt` se saare attendance records parh kar ek list ki shakal mein return karta hai.
        *   `get_attendance_for_date(date_obj)`: Kisi khaas tareekh ki attendance nikal kar deta hai.
        *   `get_student_attendance_report(student_id)`: Kisi ek student ki poori attendance history nikal kar deta hai.
        *   `_cleanup_attendance_for_student(student_id)`: Jab koi student delete hota hai, to `attendance.txt` mein se us student ke saare records hata deta hai.
    5.  **CSV Export Methods (CSV File Banane Ke Functions):**
        *   `generate_daily_report_csv(date_obj)`: Ek din ki attendance report ko CSV format mein text (bytes) bana kar deta hai, jo download ho sakti hai.
        *   `generate_student_report_csv(student_id)`: Ek student ki poori attendance history ko CSV format mein text (bytes) bana kar deta hai.
    6.  **Summary Stats (Majmoi Adad o Shumar):**
        *   `get_overall_attendance_stats()`: Kuch ahem statistics calculate karta hai, jese kul (total) students, kul attendance records, aur majmoi (overall) attendance rate (kitne feesad hazri rahi).

### 4. `ui_pages.py`

*   **Role (Kirdar):** Yeh file `app.py` mein banaye gaye tabs ke andar jo kuch dikhana hai, usko define karti hai. Har function ek tab ka content (UI elements) banata hai.
*   **Code Ka Basic Logic (Kaam Karne Ka Tareeqa):**
    *   Har function ko `manager` (jo `DataManager` ka object hai) pass kiya jata hai takay woh data hasil kar sake.
    *   **`show_admin_dashboard_tab(manager)`:**
        *   "System Overview" dikhata hai (total students, total records, attendance rate). Yeh data `manager.get_overall_attendance_stats()` se ata hai.
        *   "Individual Student Report" section banata hai:
            *   Ek dropdown deta hai jahan se student select kiya ja sakta hai.
            *   Select kiye gaye student ki report (kitne din hazir, kitne ghair hazir, personal attendance rate) dikhata hai.
            *   Report ko CSV mein download karne ka button deta hai.
            *   Student ki poori attendance history ek table mein dikhata hai.
    *   **`show_attendance_report_tab(manager)`:**
        *   "Daily Attendance Sheet" ka tab banata hai.
        *   User se tareekh (date) select karwata hai.
        *   Us tareekh ki attendance report ko CSV mein download karne ka button deta hai.
        *   Us tareekh ki attendance ko table mein dikhata hai (Student ID, Name, Status).
    *   **`show_student_management_tab(manager)`:**
        *   "Add a New Student" ke liye form dikhata hai. Naam enter karke "Add Student" button dabane par `manager.add_student()` call hota hai.
        *   "Edit and Delete Students" ke liye `st.data_editor` istemal karta hai. Yeh ek editable table hota hai jismein students ki list dikhai jati hai. User wahan se naam badal sakta hai ya student ko delete (row remove karke) kar sakta hai. "Save Changes" par yeh tabdeeliyan `manager` ke zariye save hoti hain.
    *   **`show_mark_attendance_tab(manager)`:**
        *   "Mark Daily Attendance" ka tab banata hai.
        *   User se tareekh select karwata hai.
        *   Sab students ki list dikhata hai. Har student ke samne uska mojooda status (agar us din ki attendance mark ho chuki hai to "Present"/"Absent", warna "Not Marked") dikhata hai.
        *   Har student ke liye "Present" aur "Absent" ke buttons deta hai. Button click karne par `manager.mark_attendance()` call hota hai aur page `st.rerun()` se refresh hojata hai takay updated status nazar aye.

## Khaas Features (Special Features)

1.  **Default Admin Account:** Jab aap application pehli baar chalate hain, to "admin" naam ka user khud ba khud ban jata hai jiska password "admin123" hota hai. Aap isse login karke system istemal karna shuru kar sakte hain.
2.  **Password Hashing:** Users ke passwords `auth.txt` file mein direct nahi, balke SHA-256 algorithm se hash karke store kiye jate hain. Yeh security ke liye behtar hai.
3.  **CSV Export:** Aap daily attendance reports aur har student ki individual attendance report ko CSV file format mein download kar sakte hain. Yeh files Excel ya doosre software mein kholi ja sakti hain.
4.  **Interactive Student Management:** Students ki list ko `st.data_editor` ke zariye aaram se edit (naam badalna, delete karna) kiya ja sakta hai.
5.  **Session Management:** `st.session_state` ka istemal karke user ka login status yaad rakha jata hai, takay woh app mein navigate karte waqt logged in rahe.
6.  **File-Based Storage:** Saara data (users, students, attendance) text files (`auth.txt`, `students.txt`, `attendance.txt`) mein save hota hai. Iske liye koi alag se database software install karne ki zaroorat nahi.

## Viva Ke Liye Sawal Jawab (Viva Q&A)

**Q1: Is project ka asal maqsad kya hai?**
A: Sir, is project ka maqsad students ki hazri (attendance) ko digital tareeqe se manage karna hai. Is se teachers ya admins students ko add/edit kar sakte hain, roz ki hazri laga sakte hain, aur reports dekh/download kar sakte hain.

**Q2: Aapne Streamlit library kyun istemal ki?**
A: Sir, Streamlit Python mein web applications banane ka ek bohot hi asaan aur tez tareeqa hai. Isse hum jaldi se user interface (UI) bana sakte hain bina complex web technologies (HTML, CSS, JavaScript) ki gehrai mein jaye.

**Q3: Passwords ko kaise secure kiya gaya hai?**
A: Sir, passwords ko `hashlib` library ki madad se SHA-256 algorithm istemal karke hash kiya gaya hai. Iska matlab hai ke `auth.txt` file mein اصل password nahi, balke uska ek encrypted form (hash) store hota hai, jisse asal password maloom karna mushkil hota hai.

**Q4: Data (students, attendance) kahan store ho raha hai?**
A: Sir, students ka data `students.txt` file mein aur attendance ka data `attendance.txt` file mein store ho raha hai. Users ka data `auth.txt` mein store hota hai. Yeh sab text files hain.

**Q5: Agar main ek student ko system se delete kar deta hoon, toh kya uski purani attendance bhi delete ho jayegi?**
A: Ji sir, jab ek student ko delete kiya jata hai, to `data_manager.py` mein likha code (`_cleanup_attendance_for_student` function) us student se mutalliq tamam purani attendance records ko bhi `attendance.txt` file se delete kar deta hai.

**Q6: CSV export feature ka kya faida hai?**
A: Sir, CSV export se admin daily attendance sheets ya kisi student ki poori attendance history ko file mein download kar sakta hai. Yeh file Excel mein kholi ja sakti hai, print ki ja sakti hai, ya kisi aur system mein istemal ki ja sakti hai.

**Q7: `st.rerun()` kya karta hai aur yeh kyun zaroori hai?**
A: Sir, `st.rerun()` Streamlit ka ek command hai jo app ke script ko shuru se dubara chalata hai. Yeh page ko refresh karne jaisa hai. Yeh zaroori hai takay jab bhi data mein koi tabdeeli ho (jese naya student add karna, ya attendance mark karna), woh foran user ko screen par nazar aa jaye.

**Q8: `st.session_state` ka kya kirdar hai is project mein?**
A: Sir, `st.session_state` user ke session ke dauran information store karne ke liye istemal hota hai. Is project mein, yeh `logged_in` (user login hai ya nahi) aur `username` (login user ka naam) ko yaad rakhta hai. Iski wajah se, jab user login karta hai ya app ke andar mukhtalif actions karta hai, to uska login status barqarar rehta hai.

Umeed hai yeh README aapke liye madadgar sabit hogi! Best of luck for your viva!
