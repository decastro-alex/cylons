
# Build

### Build OpenCV

**KMAG YOYO**

### Build Cylon

	export CYLON_HOME=/path/to/checkout
	cd $CYLON_HOME
	mvn clean package

### Start Apache Kafka 

`$KAFKA_HOME/bin/zookeeper-server-start.sh $KAFKA_HOME/config/zookeeper.properties`

*Hint: Change set* `log.retention.minutes=1` *in* `$KAFKA_HOME/config/server.properties` 

`$KAFKA_HOME/bin/kafka-server-start.sh $KAFKA_HOME/config/server.properties`


### Start Apache Flink

`$FLINK_HOME/bin/start-cluster.sh`

(You can now go to [http://localhost:8080](http://localhost:8080) to see the Flink GUI, if you didn't already know that.)

# Run Sample Scripts

### Testing Kafka Video Producer

`$CYLON_HOME/bin/test-video-producer.sh`

The test will connect to an RSTP video stream, which is the same type of video that the webcam generates.

Drones get about 8 minutes each run, and there is some other quirks to connecting to them, we test our video/markup/face
 detection ability with this video feed.
 
### Viewing the Output

Start the http-server with 

`$CYLON_HOME/bin/test-http-server.sh`

The video server URLs are 

`http://localhost:8090/cylon/cam/<topic>/<key>`

So to check the test feed that was just set up

[http://localhost:8090/cylon/cam/test/test](http://localhost:8090/cylon/cam/test/test)

### Flink

Copy OpenCV jar and binary to Flink Libraries Folder as well as the static loader

	cp $OPENCV_HOME/build/bin/opencv-330.jar $FLINK_HOME/lib
	cp $OPENCV_HOME/build/lib/libopencv_java330.so $FLINK_HOME/lib
	cp $CYLON_HOME/opencv/target/opencv-1.0-SNAPSHOT.jar $FLINK_HOME/lib

	
This is required to avoid a variety of experiences of the dreaded
`java.lang.UnsatisfiedLinkError`
	
Now you can run the Flink engin (which marks up with detected faces)

`$FLINK_HOME/bin/flink run /home/rawkintrevo/gits/cylon-blog/flinkengine/target/flink-engine-1.0-SNAPSHOT.jar`

# Observer

You should be able to see some interesting things at:

[http://localhost:8090/cylon/cam/test/test](http://localhost:8090/cylon/cam/test/test)
[http://localhost:8090/cylon/cam/test-flink/test](http://localhost:8090/cylon/cam/test-flink/test)
