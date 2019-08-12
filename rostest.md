# How to use rostest for testing?

## Create workspace

```
mkdir -p ~/workspace/src
cd ~/workspace/src
catkin_init_workspace
```

## Create package

```
cd ~/workspace/src
catkin_create_package mypackage rostest
```

## Add tests

```
cd ~/workspace/src/mypackage
mkdir test
cd test
touch mytests.py
chmod +x mytests.py
```

Using your favorite editor, edit `mytests.py` (e.g., vim or gedit)
``` 
#!/usr/bin/env python
import unittest
import sys

class MyTestCase(unittest.TestCase):
    def runTest(self):
        self.assertTrue(True)

if __name__ == '__main__':
    import rostest
    rostest.rosrun('mypackage', 'mytest1', MyTestCase, sys.argv)
```

# Add rostest

```
cd ~/workspace/src/mypackage/test
touch run_tests.test
```

Using your favorite editor, edit `run_tests.test` (e.g., vim or gedit)
```
<launch>
  <test test-name="tests" pkg="mypackage" type="mytests.py"/>
</launch>
```

# Configure CMakeLists.txt

```
cd ~/workspace/src/mypackage
echo "add_rostest(test/run_tests.test)" >> CMakeLists.txt
```

# Build package & run tests

```
cd ~/workspace
catkin_make run_tests
```
