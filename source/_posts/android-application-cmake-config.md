---
title: android_application_cmake_config
date: 2018-03-13 10:32:37
tags: android_application_cmake
---
```
 app.build

 android{
   ...
   defalutConfig{
   	...
   	externalNativeBuild{
		cmake{
		}
        }
        ndk{
	  abiFilters "x_86","x_86_64","armeabi","armeabi_v7a","arm64-v8a"
        }
   }

   externalNativeBuild{
      cmake{
	path " CMakeLists.txt"
      }
   }

   productFlavors{
	release{
        	applicationId com.example.xu.test
		externalNativeBuild{
			cmake{
				targets "Test"
                        }
                  }
         }
   }

}
```
