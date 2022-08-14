author: Ganesh Roman
summary: Android Proguard Congiguration Tutorial by Ganesh V Roman
id: android-proguard-config
tags: test, JUnit, jUnit
categories: Android, Java, Kotlin, Codelabs
environments: Web
status: Published
feedback link: https://ganeshroman.github.io/

# Android Proguard by Ganesh V Roman


## Summary
Duration: 0:03:00


<ul>
<li> **Publisher:**  Ganesh V Roman 
<li> **Technology:**  Android, Kotlin 
<li> **Summary:**  Android Proguard Congiguration Tutorial maintained by Ganesh.
<li> **Webpage:**  [http://ganeshroman.github.io](http://ganeshroman.github.io)
<li> **Created Date:** 15 August 2022
<li> **Last modified:** 15 August 2022 
<li> **Copyright:** © Ganesh V Roman 
</ul>



## Technical Terminology
Duration: 0:04:00

### Shrink

Detects and safely ***removes unused classes, fields, methods, and attributes from your app*** and its library dependencies (making it a valuable tool for working around the 64k reference limit). For example, if you use only a few APIs of a library dependency, shrinking can identify library code that your app is not using and remove only that code from your app.

***Resource Shrinking***
Removes unused resources from your packaged app, including unused resources in your app’s library dependencies. It works in conjunction with code shrinking such that once unused code has been removed, any resources no longer referenced can be safely removed as well.


### Obfuscation 
Shortens the name of classes and members, which results in reduced DEX file sizes.


### Optimization
Inspects and rewrites your code to further reduce the size of your app’s DEX files. For example, if R8 detects that the else {} branch for a given if/else statement is never taken, R8 removes the code for the else {} branch.


```

	android {
		buildTypes {
			release {
            
				minifyEnabled true               // Enables code shrinking, obfuscation, and optimization for only your project's release build type
				shrinkResources true               // Enables resource shrinking, which is performed by the Android Gradle plugin.
            
				proguardFiles getDefaultProguardFile(
						'proguard-android-optimize.txt'),
						'proguard-rules.pro'              // Includes the default ProGuard rules files with optimization
				}
			}
			...
		}

```

## Proguard 
Duration: 0:05:00


### 1. Data Classes, Serialized Classes

Probably every app has some kind of data class (also known as DMOs, models, etc. depending on context and where they sit in your app’s architecture). The thing about data objects is that usually at some point they will be loaded or saved (serialized) into some other medium, such as network (an HTTP request), a database (through an ORM), a JSON file on disk or in a Firebase data store.

Many of the tools that simplify serializing and deserializing these fields rely on reflection. GSON, Retrofit, Firebase — they all inspect field names in data classes and ***turn them into another representation*** (`for example- {“name”: “Sue”, “age”: 28}`), either for transport or storage.

***Conclusion- We cannot let ProGuard rename or remove any fields on these data classes***, as they have to match the serialized format. It’s a safe bet to add a `@Keep` annotation on the whole class or a wildcard rule on all your models-

`-keep class com.example.data.api.service.model.** { *; }`


```
		@Keep
		data class User(val id: String = "")
```

for Keeping Class Members

```
		-keepclassmembers class com.mindorks.sample.User{
			public *;
		}
```

## Java code called from native side (JNI)
Duration: 0:03:00

### JNI
Androids default ProGuard files ( you should always include them, they have some really useful rules ) 
already contain a rule for methods that are implemented on the native side 

	`(-keepclasseswithmembernames class * { native <methods>; })`


***Conclusion-*** Because ProGuard can only inspect Java classes, it will not know about any usages that happen in ***native code. We must explicitly retain*** such usages of classes and members via a `@Keep` annotation or `-keep` rule.


## Opening resources from JAR/APK
Duration: 0:03:00

### Resources

The problem is that usually these classes will look for resources under their own package name (which translates to a file path in the JAR or APK). ProGuard can rename package names when obfuscating, so after compilation ***it might happen that the class and its resource file are no longer in the same package in the final APK.***

To identify loading resources in this way, you can look for calls to `Class.getResourceAsStream / getResource and ClassLoader.getResourceAsStream / getResource` in your code and in any third party libraries you depend on.

`-keepnames class okhttp3.internal.publicsuffix.PublicSuffixDatabase`


## Investigation Rules of Third party Libraries
Duration: 0:03:00

### Libraries (included/imported)
Some library authors supply recommended ProGuard rules.

So how to come up with ***rules when the library doesn’t supply*** them?

1 Read the build output and logcat.
2 Build warnings will tell you which `-dontwarn` rules to add
3 `ClassNotFoundException`, `MethodNotFoundException` and `FieldNotFoundException` will tell you which `-keep` rules to add

## Debugging and stack traces
Duration: 0:03:00

### Switch on Messages and Debug
Some of those are actually useful to the developer — for example, you might want to retain source file names and line numbers for stack traces to make debugging easier-

`
-keepattributes SourceFile, LineNumberTable
`

If you are going to attach a debugger to step through method code in a ProGuarded build of your app, you should also keep the following attributes to retain some debug information about local variables (you only need this line in your debug build type)-

`
-keepattributes LocalVariableTable, LocalVariableTypeTable
`


## Runtime annotations, type introspection
Duration: 0:03:00

### Annotations
For some libraries that’s not a problem — those that process annotations and generate code at compile time (such as Dagger 2 or Glide and many more) might not need these annotations later on when the program runs.
 
`-keepattributes *Annotation*, Signature, Exception`
 



## Moving everything to the default package
Duration: 0:03:00

### Reducing path
The -repackageclasses option is not added by default in the ProGuard config. If you are already obfuscating your code and have fixed any problems with proper keep rules, you can add this option to further reduce DEX size. It works by moving all classes to the default (root) package, essentially freeing up the space taken up by strings like ***“com.example.myapp.somepackage”.***


`-repackageclasses`



## Minified debug build type
Duration: 0:03:00

### Minified
In order to fully test and debug any ProGuard problems, it’s good to set up a separate, minified debug build like this-

```
	buildTypes {
		debugMini {
			initWith debug
			minifyEnabled true
			shrinkResources true
			proguardFiles getDefaultProguardFile('proguard-android.txt'), 
					'proguard-rules.pro'
			matchingFallbacks = ['debug']
			}
		}

```

## ProGuard optimizations
Duration: 0:03:00

### Optimization
As I mentioned before, ProGuard can do 3 things for you:

1. It gets rid of unused code,
2. Renames identifiers to make the code smaller,
3. Performs whole program optimizations.


	```
	release {
		minifyEnabled true
		proguardFiles 
		getDefaultProguardFile('proguard-android-optimize.txt'),
					'proguard-rules.pro'
		}
	```
















