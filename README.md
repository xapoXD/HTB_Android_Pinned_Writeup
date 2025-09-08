# Hack The Box Android challenge - Pinned!!!

Hello everyone! 👋 In this writeup, we’ll dive into the **Hack The Box Android Easy challenge “Pinned.”** This challenge is all about digging into an Android app, and while there are several possible ways to crack it, I’ll walk you through my two favorite approaches: **static analysis** and **dynamic analysis.** Along the way, we’ll uncover some interesting tricks, highlight key takeaways, and break down the thought process so you can apply these techniques in your own Android security testing.

## **STATIC approach**

Upon downloading and unpacking the app we can decompile the app with `jadx-gui` tool. In general the first thing I do when I reverse Android apps is to locate the MainActivity within AndroidManifest.xml. This is a critical configuration file at the root of every Android app that defines app structure and components.

![image.png](https://private-user-images.githubusercontent.com/99973492/486709551-9ee95c8b-a98f-4e52-b8df-b67de396f7db.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTczMjY5MzIsIm5iZiI6MTc1NzMyNjYzMiwicGF0aCI6Ii85OTk3MzQ5Mi80ODY3MDk1NTEtOWVlOTVjOGItYTk4Zi00ZTUyLWI4ZGYtYjY3ZGUzOTZmN2RiLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA5MDglMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwOTA4VDEwMTcxMlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWMyYTI1MDVhOTFjNTQyYTdjMTAxNTBlNWM5YTZlODU2YTRmMzNjOTBlZTVhMTMxNGU1MjFlMWI1Yjk2ZDVmZDUmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.GnrZrw028lWu4cSVyu0n-6qNZBeUI53Dk2j7jWy0REE)

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    android:versionCode="1"
    android:versionName="1.0"
    android:compileSdkVersion="30"
    android:compileSdkVersionCodename="11"
    package="com.example.pinned"
    platformBuildVersionCode="30"
    platformBuildVersionName="11">
    <uses-sdk
        android:minSdkVersion="16"
        android:targetSdkVersion="30"/>
    <uses-permission android:name="android.permission.INTERNET"/>
    <application
        android:theme="@style/Theme.Pinned"
        android:label="@string/app_name"
        android:icon="@mipmap/ic_launcher"
        android:allowBackup="true"
        android:supportsRtl="true"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:appComponentFactory="androidx.core.app.CoreComponentFactory">
        <activity android:name="com.example.pinned.MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
```

Now we have a location of com.example.pinned.MainActivity. 

After examining the code the main concept of the app is that it will perform a login request with predefined credentials `bnavarro:1234567890987654`. When this login creds are filled in it will print *You are logged in.* This logic is facilitated by a function called x(). 

```java
    public void x() throws NoSuchPaddingException, NoSuchAlgorithmException, InvalidKeyException {
        HttpsURLConnection httpsURLConnection;
        TextView textView;
        String str;
        MainActivity mainActivity = this;
        HttpsURLConnection httpsURLConnection2 = (HttpsURLConnection) new URL("https://pinned.com:443/pinned.php").openConnection();
        httpsURLConnection2.setRequestMethod("POST");
        httpsURLConnection2.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
        httpsURLConnection2.setRequestProperty("Accept", "application/x-www-form-urlencoded");
        httpsURLConnection2.setRequestProperty("charset", "utf-8");
        httpsURLConnection2.setDoOutput(true);
        httpsURLConnection2.setSSLSocketFactory(mainActivity.q.getSocketFactory());
        if (mainActivity.s.getText().toString().equals("bnavarro") && mainActivity.t.getText().toString().equals("1234567890987654")) {
            StringBuilder sbG = c.a.a.a.a.g("uname=bnavarro&pass=");
            StringBuilder sb = new StringBuilder();
            sb.append(d.a());
            sb.append(c.b.a.b.a());
            sb.append(h.a());
            sb.append(c.a());
            sb.append(i.a());
            ArrayList arrayList = new ArrayList();
            arrayList.add("9GDFt6");
            arrayList.add("83h736");
            arrayList.add("kdiJ78");
            arrayList.add("vcbGT6");
            arrayList.add("LPGt63");
            arrayList.add("kFgde4");
            arrayList.add("5drDr4");
            arrayList.add("Y6ttr5");
            arrayList.add("444w45");
            arrayList.add("hjKd56");
            sb.append((String) arrayList.get(4));
            sb.append(e.a());
            sb.append(f.a());
            ArrayList arrayList2 = new ArrayList();
            arrayList2.add("TG7ygj");
            arrayList2.add("U8uu8i");
            arrayList2.add("gGtT56");
            arrayList2.add("84hYDG");
            arrayList2.add("yRCYDm");
            arrayList2.add("7ytr4E");
            arrayList2.add("j5jU87");
            arrayList2.add("yRCYDm");
            arrayList2.add("jd9Idu");
            arrayList2.add("kd546G");
            sb.append((String) arrayList2.get(7));
            sb.append(c.b.a.a.a());
            String string = sb.toString();
            httpsURLConnection = httpsURLConnection2;
            SecretKeySpec secretKeySpec = new SecretKeySpec((String.valueOf(b.q.h.b().charAt(3)) + String.valueOf(b.q.h.b().charAt(0)) + String.valueOf(c.a().charAt(0)) + String.valueOf(c.b.a.a.a().charAt(8)).toUpperCase() + String.valueOf(h.a().charAt(1)) + String.valueOf(b.q.h.b().charAt(0)) + String.valueOf(i.a().charAt(5)).toUpperCase() + String.valueOf(c.b.a.a.a().charAt(7)) + String.valueOf(c.b.a.b.a().charAt(4)) + String.valueOf(e.a().charAt(4)) + String.valueOf(e.a().charAt(4)) + String.valueOf(i.a().charAt(5)).toUpperCase() + String.valueOf(d.a().charAt(3)) + String.valueOf(d.a().charAt(5)) + String.valueOf(h.a().charAt(1)) + String.valueOf(h.a().charAt(1))).getBytes(), g.a());
            Cipher cipher = Cipher.getInstance(g.a());
            cipher.init(2, secretKeySpec);
            sbG.append(new String(cipher.doFinal(Base64.decode(string, 0)), "utf-8"));
            mainActivity = this;
            mainActivity.o = sbG.toString();
            textView = mainActivity.r;
            str = "You are logged in.";
        } else {
            httpsURLConnection = httpsURLConnection2;
            StringBuilder sbG2 = c.a.a.a.a.g("uname=");
            sbG2.append(mainActivity.s.getText().toString());
            sbG2.append("&pass=");
            sbG2.append(mainActivity.t.getText().toString());
            mainActivity.o = sbG2.toString();
            textView = mainActivity.r;
            str = "Wrong credentials!";
        }
        textView.setText(str);
        DataOutputStream dataOutputStream = new DataOutputStream(httpsURLConnection.getOutputStream());
        try {
            dataOutputStream.writeBytes(mainActivity.o);
            dataOutputStream.flush();
            dataOutputStream.close();
            new Thread(mainActivity.new b(httpsURLConnection)).start();
        } catch (Throwable th) {
            try {
                throw th;
            } finally {
            }
        }
    }
```

That checks the correct creds but also establishes an HTTP connection and sends credentials inside a body

`StringBuilder sbG = c.a.a.a.a.g("uname=bnavarro&pass=");`

The string `sbG` is very important because it is used for appending the password for the login request. That happens within this part of code.

```java
SecretKeySpec secretKeySpec = new SecretKeySpec((String.valueOf(b.q.h.b().charAt(3)) + String.valueOf(b.q.h.b().charAt(0)) + String.valueOf(c.a().charAt(0)) + String.valueOf(c.b.a.a.a().charAt(8)).toUpperCase() + String.valueOf(h.a().charAt(1)) + String.valueOf(b.q.h.b().charAt(0)) + String.valueOf(i.a().charAt(5)).toUpperCase() + String.valueOf(c.b.a.a.a().charAt(7)) + String.valueOf(c.b.a.b.a().charAt(4)) + String.valueOf(e.a().charAt(4)) + String.valueOf(e.a().charAt(4)) + String.valueOf(i.a().charAt(5)).toUpperCase() + String.valueOf(d.a().charAt(3)) + String.valueOf(d.a().charAt(5)) + String.valueOf(h.a().charAt(1)) + String.valueOf(h.a().charAt(1))).getBytes(), g.a());
Cipher = Cipher.getInstance(g.a());
cipher.init(2, secretKeySpec);
sbG.append(new String(cipher.doFinal(Base64.decode(string, 0)), "utf-8"));
```

Looking up the API documentation for [*SecretKeySpec*](https://developer.android.com/reference/javax/crypto/spec/SecretKeySpec) and [*Cipher](https://developer.android.com/reference/javax/crypto/Cipher)* we can identify that it is used for encryption and decryption. 

`cipher.init(2, secretKeySpec);` the 2 means its in decrypting mode and `cipher.doFinal(Base64.decode(string, 0)), "utf-8")` call is used for the final decryption of the string with predefined key and algorithm.

Now we get info the fun part! We can reconstruct all strings based on its corresponding imports, reverse used decrypt algorithm to get the flag that will get send inside the request as a pass parameter. 

The principle is that we need to locate the imported class (e.g. `b.q.h.b().charAt(3))`) and follow its indexes to get the character used. With this we are able to reconstruct the following code:

```java
SecretKeySpec secretKeySpec = new SecretKeySpec((String.valueOf("u29Ve2SEXVVS4jee").getBytes(), "AES")
Cipher = Cipher.getInstance("AES");
cipher.init(2, secretKeySpec);
sbG.append(new String(cipher.doFinal(Base64.decode("zlg4rjdEd0Xvwel80q98Cc1Z1TPpCsLPGt63lw+sVsk3ED9ayRCYDmfQn/gdiEvh", 0)), "utf-8"));
```

Now we know what algorithm is used, what string to decipher and what is the key. This is the code to reconstruct the string to its original form.

```python
import base64
import hashlib
from Crypto import Random
from Crypto.Cipher import AES

def unpad(s):
    return s[:-ord(s[len(s)-1:])]

def decrypt():
    key = b"u29Ve2SEXVVS4jee"
    ciphertext_b64 = "zlg4rjdEd0Xvwel80q98Cc1Z1TPpCsLPGt63lw+sVsk3ED9ayRCYDmfQn/gdiEvh"
    print("BLOB: ", ciphertext_b64)
    # Decode Base64
    ciphertext = base64.b64decode(ciphertext_b64)

    print("B64 decoded blob: ", ciphertext)
    print("KEY bytes object for Cipher.doFinal: ", key)

    # AES ECB decryption
    cipher = AES.new(key, AES.MODE_ECB)
    decrypted = unpad(cipher.decrypt(ciphertext)).decode('utf-8')

    #print(decrypted.decode("utf-8"))
    print(decrypted)

if __name__ == "__main__":
    decrypt()

```

This is the output:

```bash
└─$ python3 decode.py        
BLOB:  zlg4rjdEd0Xvwel80q98Cc1Z1TPpCsLPGt63lw+sVsk3ED9ayRCYDmfQn/gdiEvh
B64 decoded blob:  b'\xceX8\xae7DwE\xef\xc1\xe9|\xd2\xaf|\t\xcdY\xd53\xe9\n\xc2\xcf\x1a\xde\xb7\x97\x0f\xacV\xc97\x10?Z\xc9\x10\x98\x0eg\xd0\x9f\xf8\x1d\x88K\xe1'
KEY bytes object for Cipher.doFinal:  b'u29Ve2SEXVVS4jee'
HTB{trust_n0_1_n0t_3v3n_@_c3rt!}

```

---

## DYNAMIC approach

For this section I will be solving the challenge with dynamic approach. For this we will need an android phone emulator. You can setup one with the help of Android Studio. (URL) https://developer.android.com/studio. After that add a new virtual device. As this is quite old challenge I used Pixel4 API 29 with those specifications. 

![image.png](https://private-user-images.githubusercontent.com/99973492/486709623-4e94754d-5751-45e5-b919-f59099edc785.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTczMjY2MTUsIm5iZiI6MTc1NzMyNjMxNSwicGF0aCI6Ii85OTk3MzQ5Mi80ODY3MDk2MjMtNGU5NDc1NGQtNTc1MS00NWU1LWI5MTktZjU5MDk5ZWRjNzg1LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA5MDglMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwOTA4VDEwMTE1NVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWZiYmU2N2FkMmYxODJmMWExMDUwYmViNDViMWIyNWUxNTVhNDJlOGRjNzc5ODJmZGNiNmQ2YTNmZjA4ZGUxODkmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.kQKLf80UG9GoaJRwXHoEFcMV7ixrT6UodQBSjqdZRTk)

The next things needed are Android SDK Tools since we will be using **Android Debug Bridge (adb)** which is a part of that package. 

https://developer.android.com/tools/releases/platform-tools

This command line tool will let us communicate with the emulated device. 

After that we can boot up our new emulated android phone and check with **adb** tool if we can see the device. With this command: `.\adb.exe devices`

![image.png](https://private-user-images.githubusercontent.com/99973492/486709615-81560408-7db0-48b6-a176-23bb25ffe7c9.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTczMjY2MTUsIm5iZiI6MTc1NzMyNjMxNSwicGF0aCI6Ii85OTk3MzQ5Mi80ODY3MDk2MTUtODE1NjA0MDgtN2RiMC00OGI2LWExNzYtMjNiYjI1ZmZlN2M5LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA5MDglMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwOTA4VDEwMTE1NVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTdlNTFmMGQ2NWRhNDQ2ODYyZjEyYmFjNjhjZTM1ZjM3NTFjZmIwMTJkMDQzMjJhZTcyNjAxMjFhMjY4NDM5ZWImWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.iZmoscsyWhorK1VKZy4UtyzJbO96Y-4LkgGJiR-8mZw)

This tool will give us the ability to interact with the emulated android phone. 

Next tool we will be using is **Frida**. Frida is a powerful dynamic toolkit widely used in Android app testing, security research, and reverse engineering. It will allow us to inject scripts into a running apps - runtime hooking and many other cool stuff. Here is a good structured guide to install it on Windows 10. 

https://medium.com/@waqas.ahmed.faroouqi/frida-installation-guide-on-windows-10-898ae8c69e54

We will also need a Frida server available from releases on GitHub. I used: frida-server-17.2.17-android-x86_64. 

https://github.com/frida/frida/releases

---

After installing all necessary prerequisites our next steps will be:

1. Install android pinned.apk into the emulated device
    
    `./adb install D:\PATH_TO_FILE\pinned.apk`
    

![image.png](https://private-user-images.githubusercontent.com/99973492/486709622-ace20845-8cce-4bc8-b764-7ebf71b357f3.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTczMjY2MTUsIm5iZiI6MTc1NzMyNjMxNSwicGF0aCI6Ii85OTk3MzQ5Mi80ODY3MDk2MjItYWNlMjA4NDUtOGNjZS00YmM4LWI3NjQtN2ViZjcxYjM1N2YzLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA5MDglMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwOTA4VDEwMTE1NVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTA2MWFhMWU1M2YyNGRiYjE1NDg1NjVlMDU5YmQ1MWUwNTdmMWUzNmFiZDUwMWZlNWRiMWJhOGRmZTk3MzQwNTUmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0._Ld-8rlb-nmXqjJIhyHaWNPWi9Acrx7N3FM_jK0VVEI)

When we open the app we can see its content and behavior when LOGIN button is clicked.

![image.png](https://private-user-images.githubusercontent.com/99973492/486709624-6ff56654-9e27-49ad-8280-5c813a87992a.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTczMjY2MTUsIm5iZiI6MTc1NzMyNjMxNSwicGF0aCI6Ii85OTk3MzQ5Mi80ODY3MDk2MjQtNmZmNTY2NTQtOWUyNy00OWFkLTgyODAtNWM4MTNhODc5OTJhLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA5MDglMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwOTA4VDEwMTE1NVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWQ1YjM2OTQ2Y2YyZmE5NjIwZTBkZTFiNjJkOTA1YzVkMzRhYTM4YzkzMGMwNjRmOTkzMDZjZGVjOWZjNmFhMGMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.FL11ik9psXK_Hu5AApJcjhiQWDJyVRgbB-lgNVeFDmg)

![image.png](https://private-user-images.githubusercontent.com/99973492/486709619-b12702d0-24be-4db8-8181-2d1877792b13.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTczMjY2MTUsIm5iZiI6MTc1NzMyNjMxNSwicGF0aCI6Ii85OTk3MzQ5Mi80ODY3MDk2MTktYjEyNzAyZDAtMjRiZS00ZGI4LTgxODEtMmQxODc3NzkyYjEzLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA5MDglMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwOTA4VDEwMTE1NVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTU3ZTcwYzMyYmJiNmQzOWE3YTE1MjYxY2VmOWRkMzY5Yjg2NDUwNTVjNmU2NWZlYzc1NTY0MWI2ZjlmY2JiMWYmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.RMh-4XfkCHG1BoppIsnVidelFfQcc_2h2uCQuBBie0g)

2. Push Frida server onto the device. 

`./adb push D:\PATH_TO_FILE\frida-server-17.2.17-android-x86_64 /data/local/tmp`

3. Get inside the Android device and run Frida server.

`./adb shell`

`su`

`cd /data/local/tmp`

`chmod +x ./frida-server-17.2.17-android-x86_64`

`./frida-server-17.2.17-android-x86_64`

![image.png](https://private-user-images.githubusercontent.com/99973492/486709618-aaa3a804-7471-46ac-90b8-7d8fdc569037.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTczMjY2MTUsIm5iZiI6MTc1NzMyNjMxNSwicGF0aCI6Ii85OTk3MzQ5Mi80ODY3MDk2MTgtYWFhM2E4MDQtNzQ3MS00NmFjLTkwYjgtN2Q4ZmRjNTY5MDM3LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA5MDglMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwOTA4VDEwMTE1NVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWJlMWNlNmRhZWJmZDYxZmI1MWZlNjVjNWVhOWMyYWJmMTdjODU1ZTZhYjU1MDUwNmE1OTE2MTVkNjI5MTE3OWUmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0._NjXRoOBLG_TanTrYJ3U1R6RyQQjobW7pIyti_YXlGY)

Now We have a running instance for a Frida server. We will need this for later as we will want to run script in context of that app.

4. Check identifier of running Pinned app

![image.png](https://private-user-images.githubusercontent.com/99973492/486709616-d1d1e118-2814-4ab2-90c9-3ca9e1f86999.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTczMjY2MTUsIm5iZiI6MTc1NzMyNjMxNSwicGF0aCI6Ii85OTk3MzQ5Mi80ODY3MDk2MTYtZDFkMWUxMTgtMjgxNC00YWIyLTkwYzktM2NhOWUxZjg2OTk5LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA5MDglMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwOTA4VDEwMTE1NVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTRiMzI0YzQzOWM5ZWE3NTU4MDhiNjcxZDlhNzE3ZWQ3ODQ4MDk3OGY4YzA5NDJhMWVjYTU4YjFlNGRmYWZmYWMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.RfzXd_9prxcsgZsmgUY2R2zFbPC_6fKWYqnaQ71xUnU)

5. Now we have everything we need to start running our scripts on the android app!

---

Our goal with this approach is to locate a function to hook at runtime and observe the values that are coming in and out. We can hook basically any type of function as long as we know where is the library/class the function uses.

My approach here was to hook the same Cipher function from the code as in the static section. 

![image.png](https://private-user-images.githubusercontent.com/99973492/486709621-273c2e03-c4e2-4ca5-b9b9-46a91d20e9ce.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTczMjY2MTUsIm5iZiI6MTc1NzMyNjMxNSwicGF0aCI6Ii85OTk3MzQ5Mi80ODY3MDk2MjEtMjczYzJlMDMtYzRlMi00Y2E1LWI5YjktNDZhOTFkMjBlOWNlLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA5MDglMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwOTA4VDEwMTE1NVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTRmNjY2MGY3MGI2ZTUyMjMyMjI1OGEzZDMwZTIyMDVkODhlNGEzOTYyZjEwMGYwM2JhODEyZDNlZDY1ZTVhMWMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.jIQ7BaTW_czK20hKo3nJblnfAFxMP9mXkDPabqxI8AI)

Cipher uses and API method called `.doFinal`, where the string is being decoded and appended to sbG string. This method gets imported within the MainActivity.

![image.png](https://private-user-images.githubusercontent.com/99973492/486709620-904582eb-ad4c-4172-8951-91fe1dc76191.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTczMjY2MTUsIm5iZiI6MTc1NzMyNjMxNSwicGF0aCI6Ii85OTk3MzQ5Mi80ODY3MDk2MjAtOTA0NTgyZWItYWQ0Yy00MTcyLTg5NTEtOTFmZTFkYzc2MTkxLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA5MDglMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwOTA4VDEwMTE1NVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTNhN2Y4ZTI5NmM1MTg2MzE1MWY1MTdhNGU4YWRhNjFjOGU0ZDBkZjMwZjQzYzk2MzUxOWU1MDBjZTEyN2Y1NDImWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.dclarww_Coy-kPS-2le0f_0SGfcFmJxnGZwHQf9dSXo)

`import javax.crypto.Cipher;`

Now when we have located a function to hook we can make a Frida script and hook the .doFinal call. One problem with this call is that this function has a lot of overloaded functions, so we need to tell Frida which one we want to overload. 

In the Android documentation we can see that the `doFinal` function expects the message to be in bytes. We can then intercept the calculation of the flag, capture the output of the function and inside the script convert the value from byte array to UTF-8 string to get a flag.

The script:

```jsx
Java.perform(()=>{
    const Cipher = Java.use('javax.crypto.Cipher');

    Cipher.doFinal.overload('[B').implementation = function (bytes){
        console.log("Bytes: ", bytes);
        
        var result = this.doFinal(bytes);
        console.log("Send back: ", result);

        var str = Java.use("java.lang.String").$new(result, "UTF-8");
        console.log("Decoded string: ", str);

        return result;
      }
})
```

Frida supports writing scripts with JavaScript. All scripts are executed when the appropriate method gets called, so when we run our script, nothing happens until we hit the LOGIN button as the method gets called afterwards not just after starting the app.

We can run our script with this command within the context we got earlier and after we hit the LOGIN button: 

`frida.exe -U -l .\frida_script.js -f com.example.pinned`

![image.png](https://private-user-images.githubusercontent.com/99973492/486709617-1b203e94-248e-4841-a561-a10ddf4114d6.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTczMjY2MTUsIm5iZiI6MTc1NzMyNjMxNSwicGF0aCI6Ii85OTk3MzQ5Mi80ODY3MDk2MTctMWIyMDNlOTQtMjQ4ZS00ODQxLWE1NjEtYTEwZGRmNDExNGQ2LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA5MDglMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwOTA4VDEwMTE1NVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTQxMDk3YTQ5OGRkNDY3NThjZmQxOTI5NTVmYmVlYWIzYTQ5OWJhNjM3YTg2N2ZiMDI5ZWFlMWQxMmQwNWViZDMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.oQlm9DDcN6TSzKJ537bMKEQ1D0MVjRth560K3LzOIww)

We can see we successfully hooked the doFinal function and got the decoded flag as output!

FLAG: `HTB{trust_n0_1_n0t_3v3n_@_c3rt!}`
