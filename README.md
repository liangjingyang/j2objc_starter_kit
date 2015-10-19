# J2objc Project Template

## Usage

    $./create_project domain app_name

## Example

    $./create_project com.example demo

It will generate the `demo` directory in parallel with AvantX project template:

    your_path
    ├── j2objc_project_template
    │   ├── ...
    ├── demo
    │   ├── Gemfile
    │   ├── README
    │   ├── j2objc_project.iml
    │   ├── build.gradle
    │   ├── create_project
    │   ├── demo                        // for xcode
    │   ├── demo.ios
    │   ├── gradle
    │   ├── gradle.properties
    │   ├── gradlew
    │   ├── gradlew.bat
    │   ├── lib
    │   ├── local.properties
    │   └── settings.gradle

### IOS

To build the project, you must have j2objc, a java to objectiveC transpiler. Get it from:

    $git clone git@github.com:Sellegit/j2objc.git

and follow the [instructions on building](https://github.com/google/j2objc/wiki/Building-J2ObjC)

Now you should have excutables j2objc and j2objcc under /path/to/j2objc/dist/. Add them to your search path. I personally prefer to use symlink. Assuming on Mac,

    $ln -s /path/to/j2objc/dist/j2objc /usr/bin/j2bjc
    $ln -s /path/to/j2objc/dist/j2objcc /usr/bin/j2objcc  

Test you do have those two under the search path by typing

    $j2objc

If above went through, type 

    $bundle; bundle install

to install dependencies. Then we can go into the `demo/demo` directory to make the project.

    $cd your_path/demo/demo
    $make

don't worry if you see message from make that complains missing directories the first time you make the project, as they will be created by make later. Also, if the above complains 
> /bin/sh: integratej2objc: command not found

try

    $bundle exec make

If make successes, optionally you can enable incremental build by 

    $gem install rerun
    $rerun -p "**/*.java" "make incremental -j$N" #!replace $N with the number of processors you want for a concurrent build(e.g. -j8)


Now you have translated all the java sources into objectiveC sources, it's time to compile the project. This project uses Xcode.
Assuming you are under the project directory

    #! replace the following to /a/path/to/your/j2objc/dist
    $echo "J2OBJC_HOME = /path/to/j2objc/dist;" > Env.xcconfig

    #! skip the next line if you have pod already
    $sudo gem install cocoapods

    $pod install
    $open demo.xcworkspace

Now, try build and run the project from Xcode.
