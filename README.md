# DevOps_Sem.5


Task for Assignment 1

echo "First line" > firstfile.txt

echo "Second line" >> firstfile.txt
echo "Third line" >> firstfile.txt

echo "Fourth line" >> firstfile.txt
echo "Fifth line" >> firstfile.txt

mv firstfile.txt secondfile.txt

head -n 5 secondfile.txt

tail -n 5 secondfile.txt

awk 'NR>=2 && NR<=6' secondfile.txt

nano firstfile.txt
nano secondfile.txt

ps aux
ps aux | grep nano
kill <PID>
