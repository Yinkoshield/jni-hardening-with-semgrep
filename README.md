# JNI Hardening with Semgrep 🚀  

## 🔐 Strengthening JNI Security & Stability  

YinkoShield is committed to building **secure, crash-proof mobile apps** that run smoothly on even the most constrained devices. This repository provides a **Semgrep rule set** designed to enforce **defensive JNI (Java Native Interface) programming** best practices, helping developers:  

✅ Prevent **memory leaks** & **null dereferencing**  
✅ Ensure **JNI exception handling** is correctly implemented  
✅ Optimize **JNI performance** on low-end Android devices  
✅ **Enhance security** by reducing attack surfaces  

---

## 📌 Why JNI Hardening?  

**JNI is powerful, but also dangerous.** While it enables direct access to native C/C++ code, improper JNI handling can lead to:  

🚨 **Memory leaks** (failure to release allocated objects)  
🚨 **App crashes** (null dereferencing, unhandled exceptions)  
🚨 **Performance bottlenecks** (inefficient JNI calls in loops)  
🚨 **Security risks** (attackers exploiting uncleaned JNI errors)  

To tackle these risks, **YinkoShield** has developed a **Semgrep ruleset** that catches common JNI mistakes **before they reach production.**  

---

## 📥 Installation & Setup  

### 1️⃣ Install Semgrep  

Semgrep is a lightweight, open-source **static analysis tool** that scans code for vulnerabilities and best practices.  

**Install via pip:**  
```sh
pip install semgrep
```

**Or use Homebrew (MacOS/Linux):**  
```sh
brew install semgrep
```

**Verify installation:**  
```sh
semgrep --version
```

---

## 🚀 Using the JNI Ruleset  

### 2️⃣ Clone this repository  
```sh
git clone https://github.com/yinkoshield/jni-hardening-with-semgrep.git
cd jni-hardening-with-semgrep
```

### 3️⃣ Run Semgrep on your JNI code  
```sh
semgrep --config ./jni-harderning-rules.yaml  path/to/your/jni/code
```

✅ This will **scan your JNI code** using YinkoShield’s defensive programming rules and **flag unsafe patterns.**  

### Example output:  
```
path/to/file.c:42: WARNING: Missing exception check after calling 'FindClass'. Ensure to handle exceptions properly after JNI calls.
path/to/file.c:56: WARNING: Missing DeleteLocalRef for 'cls'. Make sure to free local references to avoid memory leaks.
```

---

## 🔧 Integrating with CI/CD  

To **enforce JNI security checks** in your CI/CD pipeline, add Semgrep to your build process.

### 📌 GitHub Actions Integration  
Create a `.github/workflows/semgrep.yml` file in your repository:  

```yaml
name: Semgrep JNI Security Scan
on: [push, pull_request]

jobs:
  semgrep:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Install Semgrep
        run: pip install semgrep

      - name: Run Semgrep
        run: semgrep --config ./rules --lang c --error
```

🚀 **This will fail the build if security issues are found!**  

### 📌 GitLab CI Integration  
Add this to `.gitlab-ci.yml`:  

```yaml
stages:
  - security

semgrep:
  image: returntocorp/semgrep
  script:
    - semgrep --config ./rules --lang c --error
  allow_failure: false
```

---

## 🔍 What Does This Ruleset Catch?  

Our **JNI Hardening Semgrep Rules** help detect and prevent:  

### 🛡 1️⃣ **Missing Exception Checks**  
❌ **Bad:**  
```c
jclass cls = (*env)->FindClass(env, "com/example/MyClass");  // Can fail without exception handling!
```
✅ **Good:**  
```c
jclass cls = (*env)->FindClass(env, "com/example/MyClass");
if ((*env)->ExceptionCheck(env)) {
    (*env)->ExceptionDescribe(env);
    (*env)->ExceptionClear(env);
}
```

### 🛡 2️⃣ **Null Return Value Checks**  
❌ **Bad:**  
```c
jmethodID mid = (*env)->GetMethodID(env, cls, "methodName", "()V");  // No NULL check!
```
✅ **Good:**  
```c
jmethodID mid = (*env)->GetMethodID(env, cls, "methodName", "()V");
if (mid == NULL) {
    return;  // Handle error safely
}
```

### 🛡 3️⃣ **Memory Leaks (Unreleased References)**  
❌ **Bad:**  
```c
jclass cls = (*env)->FindClass(env, "com/example/MyClass");
// No DeleteLocalRef, causing a memory leak!
```
✅ **Good:**  
```c
jclass cls = (*env)->FindClass(env, "com/example/MyClass");
(*env)->DeleteLocalRef(env, cls);  // ✅ Prevent memory leak
```

### 🛡 4️⃣ **Expensive JNI Calls in Loops**  
❌ **Bad:**  
```c
for (int i=0; i<100; i++) {
    jclass cls = (*env)->FindClass(env, "com/example/MyClass");  // Inefficient!
}
```
✅ **Good:**  
```c
jclass cls = (*env)->FindClass(env, "com/example/MyClass");
for (int i=0; i<100; i++) {
    // Reuse 'cls' instead of calling FindClass every time
}
(*env)->DeleteLocalRef(env, cls);
```

---

## 🛠 Contribute & Improve the Rules  

📢 We **welcome contributions** from security researchers and developers! If you find new JNI pitfalls or want to enhance the rule set:  

1️⃣ **Fork this repository**  
2️⃣ **Improve the Semgrep rules** in `rules/`  
3️⃣ **Submit a pull request**  

Your contributions will help **strengthen JNI security** across the Android developer community! 💪  

---

## 📄 License  

This repository is **open-source** under the **MIT License**. See [LICENSE](LICENSE) for details.  

---

📢 **Join the Discussion & Stay Updated!**  
🔗 Follow us on LinkedIn for more **JNI security research**:  
👉 [YinkoShield on LinkedIn](https://www.linkedin.com/showcase/yinkoshield/)  

🚀 **Let’s make mobile apps more secure, one JNI call at a time!**  
