## ROS developing with c++

### create workspace

``` latex
$ mkdir -p ~/catkin_ws/src
$ cd ~/catkin_ws/src
$ catkin_init_workspace
$ cd ~/catkin_ws
$ catkin_make
```

### create package

``` latex
$ cd ~/catkin_ws/src
$ catkin_create_pkg <package_name> [depend1] [depend2] [depend3]
example: 
$ catkin_create_pkg beginner_tutorials std_msgs rospy roscpp
```

### create Eclipse project files

``` latex
$ cd ~/catkin_ws
$ catkin_make --force-cmake -G"Eclipse CDT4 - Unix Makefiles"
```

``` mermaid
graph TD
E(Inside Eclipse:)
A(import)-->B(gneral)
B-->C(existing project into workspace)
C-->D(directory: /catkin_ws/build)
```

### edit Cmakelist:

``` cmake
add_executable(${PROJECT_NAME} src/file_name.cpp)
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES})
```

### Turtlesim Example

``` c++
#include <ros/ros.h>
#include <turtlesim/Pose.h>
#include <geometry_msgs>

class lawnmover
{
	ros::Subscriber sub2Pose;
	ros::Publisher pub2Vel;
public:
	lawnmover();
	~lawnmover();
	
	void pose_callback(const turtlesim::Pose::ConstPtr &msg);
};


lawnmover::lawnmover()
{
	ros::NodeHandle nh_;
	sub2Pose=nh_.subscribe("/turtle1/pose",1000,pose_callback,this);
	pub2Vel=nh_.advertise<geometry_msgs::Twist>("turtle1/cmd_vel",1);

	geometry_msgs::Twist velmsg;//define message
	velmsg.linear.x = 1;
    	velmsg.angular.z = 1;
	
	pub2Vel.publish(velmsg);
}

void lawnmover::pose_callback(const turtlesim::Pose::ConstPtr &msg)
{
	ROS_INFO("x=%f, y=%f, theta=%f",msg->x,msg->y,msg->theta);
}
	
	
int main(int argc, char** argv)
{
	ros::init(argc,argv,"lawnmover_node")
	lawnmover obj1;
	
	ros::spin();
	
	return 0;
}
```

