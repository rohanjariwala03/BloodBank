# Blood Bank Management System

A small blood bank website I built in May 2020 as a learning project. It is an ASP.NET Web Forms app in C# with a SQL Server LocalDB database. There is a public site for donors and visitors, and an admin panel for staff to record donations and manage donors.

## Public site (`user/`)

- Home page with totals pulled from the database: registered donors, blood stored (ml) and feedback received.
- Donate form to register as a donor (name, contact details, city, age, weight, blood group).
- Camp page that lists donation camps and lets you filter registered donors by blood group. It isn't linked from the menu, open `user/Camp.aspx` directly.
- Contact, feedback and about pages. Contact messages and feedback are saved to the database.

`Default.aspx` redirects to `user/UserHome.aspx`.

## Admin panel (`admin/`)

Log in at `admin/Admin_Login.aspx` with an account from the `Admin_Master` table.

- Dashboard with donor, blood, contact and feedback counts, plus a list of blood units that expire today.
- Add a donation. If the email already belongs to a donor it reuses that record, otherwise it registers the donor first. Each donation is stored as 500 ml with an expiry date 3 months out.
- Blood list filtered by blood group, and deleting a blood record.
- Donor list with an edit page for donor details.
- Add donation camps.
- Read contact messages and feedback.

The admin pages use an HTML admin template, which lives under `admin/main/`.

## Database

`App_Data/BloodDatabase.mdf` holds these tables: `Admin_Master`, `User_Master` (donors), `BloodGroup`, `BloodLtr` (donations), `AddCamp`, `ContactUs` and `FeedBack`.

## Running it

You need Windows and Visual Studio with the ASP.NET workload, .NET Framework 4.5 and SQL Server Express LocalDB.

1. Open `Blood Bank Management System.csproj` in Visual Studio.
2. Fix the connection string in `Web.config`. It points at an absolute path on the machine it was built on. Change `AttachDbFilename` to `|DataDirectory|\BloodDatabase.mdf` so it uses the file in `App_Data`.
3. Run with IIS Express.

## Known issues

This was an early project and has problems I'd fix before using it for anything real:

- SQL queries are built by concatenating user input, so they are open to SQL injection. They should use parameters.
- Admin passwords are stored and compared in plain text.
- Some menu links point to pages that don't exist: `admin/Expire_Blood.aspx`, `user/user_Login.aspx` and `/Home.aspx`. Deleting a blood record also redirects to `Expire_Blood.aspx`.
- Admin pages read the logged-in name from the session in the master page, so opening one without logging in throws an error instead of redirecting to the login page.
- Build output (`bin/`, `obj/`) and `.vs/` are committed.
