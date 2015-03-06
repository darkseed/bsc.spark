# bsc.spark

##1. Introduction
[TODO]
			
##2. Compilation and packaging

###2.1 Prerequisites

####2.1.1 Spark version

    IMPORTANT: bsc.spark requires Spark 1.2.0 or greater

####2.1.2 OpenCV

    NOTE: pom.xml has a property to select between opencv-249.jar and opencv-300.jar.

    Two prerequisites

    1) opencv-249.jar has to be packaged with the app or available to Spark somehow. 
        - A "Class no found..." will be raised otherwise.
        - opencv-249.jar can be found in opencv build directory, within the "bin" folder.
        - It must be added manually to the local Maven repo:
		
          mvn install:install-file -DgroupId=opencv -DartifactId=opencv -Dpackaging=jar -Dversion=2.4.9 -Dfile=/home/rtous/aplic/opencv/release/bin/opencv-300.jar
		  
		  or
		  
		  mvn install:install-file -DgroupId=opencv -DartifactId=opencv -Dpackaging=jar -Dversion=2.4.9 -Dfile=/home/rtous/aplic/opencv/release/bin/opencv-249.jar
		  
        - Add or remove the "provided" directive from pom.xml to exclude or include the .jar in the package
        - It is not recommended to assemble the .jar when submitting to MareNostrum

    2) libopencv_java249.so has to be accesible for Spark
        - A UnsatisfiedLinkError will be raised otherwise.
        - The file can be found in opencv build directory, within the "lib" folder.
        - One alternative is to copy the file within /usr/lib/ (Spark will look there)
        - Another alternative is to use the "driver-library-path" spark-submit directive this way:
        
		--driver-library-path /home/rtous/aplic/opencv/release/lib

###2.3 Compilation and packaging with Maven

An uber jar, including the dependencies, is necessary to submit to Spark. This uber jar is obtained this way:

    $mvn clean compile assembly:single
