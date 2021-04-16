# Windows

## [How to Disable UAC Windows 10](https://www.minitool.com/news/how-to-disable-uac-windows-10-004.html)

### Option 4: Disable UAC Windows 10 Registry Key

> **Note**: Before changing Windows Registry, we recommend you to [back up registry](https://www.minitool.com/news/how-to-disable-uac-windows-10-004.html) to avoid system accidents.

- Step 1: Press **Win** plus **R** keys to launch the Run dialog.
- Step 2: Input **regedit.exe** and click **OK**.
- Step 3: Go to the path:

```
  HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System
```

- Step 4: Double-click on the key - EnableLUA and change its **Value data** to **0**.

![EnableLUA](https://www.minitool.com/images/uploads/news/2019/08/how-to-disable-uac-windows-10/how-to-disable-uac-windows-10-4.png)

- Step 5: Save the change and restart your computer.

Now, we have shown you how to disable UAC Windows 10 in detail. In addition, you may want to set UAC to automatically deny elevation requests from users with standard-level credentials to avoid being prompted to enter administrator credentials to confirm all the time when running a program requiring elevated permissions.