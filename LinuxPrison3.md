# CTF Challenge Walkthrough

## Initial Access

In this challenge, SSH access was provided to a remote machine. The login details were:

- **Command Used:**
  ```bash
  ssh ctf@hearsay3.ctf.dscjssstuniv.in -p 9103
  ```
- **Password:** `ctf`

After successfully logging into the system, the next step was to enumerate files and look for potential flags.

---

## Enumeration

The first step was listing the files available in the current directory:

```bash
ls
```

This command revealed the following files:
- `README.txt`
- `flag.txt`
- `flags`
- `more_flags`

The presence of `flag.txt` suggested that it might contain the flag, so the next logical step was to read its contents.

---

## Flag Extraction Attempts

### **Attempt 1: Using `cat`**

To read `flag.txt`, the `cat` command was initially attempted:

```bash
cat flag.txt
```

#### **Result:** ❌ Failed
- **Error Message:** `-rbash: cat: command not found`
- **Reason:** The shell was restricted (`rbash`), preventing the use of certain commands, including `cat`.

### **Attempt 2: Using `bash`**

Since `cat` was blocked, an alternative method was needed. Running the file using `bash` was attempted:

```bash
bash flag.txt
```

#### **Result:** ✅ Success
- **Flag Revealed:** `dscctf{cat_ahhhh_what}`

---

## Key Findings & Lessons Learned

1. **Restricted Shell Bypass:**
   - The system was using a restricted shell (`rbash`), preventing the execution of certain commands.
   - However, executing the file using `bash` allowed access to its contents, bypassing the restriction.

2. **Alternative Command Execution:**
   - When a command like `cat` is blocked, alternative approaches such as using `bash`, `more`, `less`, or even copying the file to another location can be useful.

3. **Understanding CTF Restrictions:**
   - Many CTF challenges impose artificial restrictions on common commands, requiring creativity to find alternative ways to access information.

---

## **Final Flag**
```bash
dscctf{cat_ahhhh_what}
```

This concludes the challenge walkthrough, demonstrating how to successfully extract the flag despite restrictions. 🚀

