
---

- Ctrl + L (Clear terminal)
- Command + L (Lock)

```bash
ls -l
ls -a  # Show all files (including .hidden_files)
ls -la
'''
total 36
drwxr-xr-x 2 kali kali 4096 Apr 10 14:51 Desktop
drwxr-xr-x 2 kali kali 4096 Apr 10 14:51 Documents
drwxr-xr-x 2 kali kali 4096 Apr 10 14:51 Downloads
'''

pwd  # /home/kali

cd
cd Downloads/
cd ..
cd ~  # /home/kali

mkdir folder1 folder2

echo hello world
echo hello world > file.txt  # Redirect output
echo hello world >> file.txt  # Append

cat file.txt  # Show content

rm file.txt
rm -r my_folder/
```