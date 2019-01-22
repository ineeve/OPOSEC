Flag 1 - Version Control is easy:

1. Get the repo by traversing to http://0x70.apl3b.com/.git/
2. Get the repo url by opening the file .git/config or .git/logs/HEAD: https://gitlab.com/DDuarte/twipy.git
3. Clone the repo locally
4. git log -p -G"\{flag\}
5. Find flag: {flag}Us3_vault_for_no_p4sswords_1n_s0urce_cod3.

Flag 2 - Debug 101
1. Debug can be done by static code analysis, dynamic code analysis or by looking into the logs.
2. In the code we can see that there is a app.logger being used
3. In the file app/__init__.py we can see that the logger is set to a file called debug.log
2. Go to the website and traverse to /debug.log: {flag}b3_c4r3ful_w1th_Wh4t_y0u_l34v3_pUbl1c

Flag 3 - Nice tweet Eve
1. Discover that website is running on a python server with flask and jinja2
2. Jinja2 is vulnerable to server side template injection.
3. Read some articles. https://pequalsnp-team.github.io/cheatsheet/flask-jinja2-ssti
4. Flask inserts a couple of global functions and helpers into the Jinja2 context, which include: 
- config, request, session, g, url_for(), get_flashed_messages(): http://flask.pocoo.org/docs/1.0/templating/#standard-context
4. Discover that {{1+1}} is actually processed by the server.
5. Make a tweet using the config global object - {{config}}
6. Get the flag: {flag}V4lid4t3_always_us3r_1NPUT

Flag 4 - This link is bamboozling
1. By reading the code we can see that it is using JWT to authenticate users.
2. When we solved last exercise, there was a field called SECRET_KEY , which maybe corresponds to the JWT secret:
yJmsCAeao5zOM3gvoxHrOyM5HGJTTDpQ7UxAIHneCxc=
3. Experimenting with recover password by email, I receive this link: 
- http://0x70.apl3b.com/auth/reset_password/eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InJlbmF0b2luZWV2ZUBnbWFpbC5jb20iLCJpZCI6IjY2ODlkMTg0LWExZDUtNGRmZC05ZGMxLWMzNzYxODVkYTZjZCIsImV4cCI6MTU0ODE4ODkyNi41MTExNDcsIm5hbWUiOiJpbmVldmUifQ.1mmDHQUuH0JnMrnu0IAyA8b2Bx_hM6jgV6e6yfd04NM
4. The path seems to be a JWT token.
5. Checkout the token using https://jwt.io
6. We can see that the payload contains only the user email, user id, expiration date and the name of the account.
7. If we can get these information from user Willis Adams, we can log into his account.
8. username: Willis Adams, user-id (from the url of the profile page): 70a82737-a6d9-4284-93db-0600db6f05ca,
email (it is in the code, file twipy.py): willis.adams@example.com
9. Now we just need to forge his recover password link (I used the jwt.io website):
http://0x70.apl3b.com/auth/reset_password/eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6IndpbGxpcy5hZGFtc0BleGFtcGxlLmNvbSIsImlkIjoiNzBhODI3MzctYTZkOS00Mjg0LTkzZGItMDYwMGRiNmYwNWNhIiwiZXhwIjoxNTQ4MTg4OTI2LjUxMTE0NywibmFtZSI6IldpbGxpcyBBZGFtcyJ9.FuWOQt38jlb2nWoDDncDUgU1vptRdd_jIALwcC11BvM
10. Trigger the recover password mechanism for the user willis adams.
11. Use the link forget to change his password and get the flag.
12. The flag: {flag}4lw4ys_v3r1fy_y0ur_t0k3NS
13. We can see that someone also made a tweet using willis adams account that gives a big into to get the 3rd flag.