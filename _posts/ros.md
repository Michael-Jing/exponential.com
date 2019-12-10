## msg types of topics
rostopic type /gps/fix
sensor_msgs/NavSatFix

rostopic type /gps/rtkfix
nav_msgs/Odometry

rostopic type /gps/time
sensor_msgs/TimeReference

rostopic type /radar/points
sensor_msgs/PointCloud2


rostopic type /radar/range
sensor_msgs/Range

rostopic type /radar/tracks
radar_driver/RadarTracks

## detail of message
PointCloud2

This message holds a collection of N-dimensional points, which may contain additional information such as normals, intensity, etc. The point data is stored as a binary blob, its layout described by the contents of the "fields" array.

The point cloud data may be organized 2d (image-like) or 1d (unordered). Point clouds organized as 2d images may be produced by camera depth sensors such as stereo or time-of-flight.
