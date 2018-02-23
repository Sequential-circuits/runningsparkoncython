Apache Spark has 3 main ways of program it: Java, Scala, and Python

Java and Scala are usually the preferred choice given the fact that they are much faster than Python. However, they are also much harder to program, require a knowledge of external compiling tools such as Maven, SBT, Graddle, etc, suffer from constant compatibility problems, etc.

So, how if we could instead improve Python´s speed? There are many techniques, but by far the most known is using a subset of Python called Cython. This is a subset which allows to run actual C code instead of python code. This is not rocket science: Python WAS developed in C, so we are well within its range.

In this project, I show how to improve the speed of python programs in Spark by recompiling them to C. We will create a program which reads from a Uber database.

Then using SparkSQL we will sum the amount of trips per base number, and we will finally save the results in cvs files (in the code, at c:\results)

We will need 3 tools, cython, easycython, and any C compiler only if you are running this on windows (unix already has one). Easycython is a script that will attempt to automatically convert one or more .pyx files into the corresponding compiled .pyd|.so binary modules files painlessly, avoiding the usual writing a setup.py each time

1 – Download the Uber database from It can can be dowloaded from https://github.com/fivethirtyeight/uber-tlc-foil-response/blob/master/Uber-Jan-Feb-FOIL.csv Save it as uber.csv

2 – Install cython with the command pip install Cython or conda install Cython if you are using Anaconda Python

3 – Install easycython with the command pip install easycython (conda install does not work). This program avoids having to create a setup.py every time that we want to compile something and a lot of headaches

4 – If you are running windows, install any C compiler such as Mingw (https://sourceforge.net/projects/mingw/files/)

5 – Download the test.pyx file. This is just a regular python file, but with an X extension to indicate it is a cython file: nothing more

6 – Convert the test.pyx using easycython test.pyx. This will convert the test.pyx files to C code called test.c and then compile in fact to test.pyd on windows or test.so on linux. You will see it created a lot of files and folders

7 – Download the file test1.py. In it, you will find all it has is a single command:  import test

8 – Run the file test1.py with the command spark-submit python test1.py uber.csv. This will actually import the files in C and execute them

9 – For comparison, download normal.py (which is exactly the same code as test.py) and execute it with spark-submit python normal.py uber.csv

Voilá! You will see that the Cython run took only 10 seconds while the regular python run took 68! That´s almost 7 times decrease in run time!

Please note that this is VERY experimental. In fact, should you want to deploy it to a cluster, you would have to install cython in everyone of them and probably manually deploy the C files. So, you are more than welcome to play with this, make comments, and hopefully be able to use python (a language I love) and be much more productive than messing around with maven and that stuff
