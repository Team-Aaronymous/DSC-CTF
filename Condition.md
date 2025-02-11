# 📝 Conditions Writeup  

## 📌 Challenge Overview  
We were given a Flask-based login system with impossible conditions:  

if len(username) >= 40:
    return "Username is too long!"
elif len(username.upper()) <= 50:
    return "Username is too short!"
else:
    return flag

The conditions seem impossible, but there's a trick!

##🚀Exploit:
The key issue is:

len(username.upper()) <= 50

Normal letters don’t change length, but some Unicode characters do!
"ß" (sharp S in German) expands into "SS", increasing length.


##Payload:
Use 30 "ß" characters:

ßßßßßßßßßßßßßßßßßßßßßßßßßßßßßßßßßßßß

Before .upper() → 30 characters ✅
After .upper() → "SS" expands → 60 characters ✅
This bypasses both conditions and gives us the flag! 🎉


##🏆 Final Flag:

dscctf{1mp0551bl3_c0nd1710n5_0r_p0551bl3_c0nd1710n5}

Lesson: Never trust user input! Unicode can be tricky! 🔥
