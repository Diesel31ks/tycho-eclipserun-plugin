# Example of usage #

**Important** If you are using Tycho 0.14, use the plugin included in Tycho extras instead, see [Tycho Additional Tools](http://wiki.eclipse.org/Tycho/Additional_Tools#tycho-eclipserun-plugin)


One use of this plugin is building the documentation index using Eclipse's antRunner and the help.buildHelpIndex Ant task.

Without Tycho, this would typically be done by having an Ant file with the help.buildHelpIndex task. For example, in customBuildCallbacks.xml:

```
<project name="Build specific targets and properties" default="noDefault">
	<target name="post.build.jars">
		<antcall target="build.index"/>
	</target>

	<target name="build.index" description="Builds search index for the plug-in: org.eclipse.someplugin.doc.user." if="eclipse.running">
		<help.buildHelpIndex manifest="plugin.xml" destination="."/>
	</target>
</project>
</pre>
```

tycho-eclipserun-plugin can be used to run Eclipse's antRunner and reuse that Ant file.

org.eclipse.someplugin.doc.user/pom.xml:

```
<build>
	<plugins>
		...
		<plugin>
			<groupId>com.google.code.tycho-eclipserun-plugin</groupId>
			<artifactId>tycho-eclipserun-plugin</artifactId>
			<version>0.13.0</version>
			<configuration>
				<appArgLine>-application org.eclipse.ant.core.antRunner -buildfile customBuildCallbacks.xml build.index</appArgLine>
				<dependencies>
					<dependency>
					   <artifactId>org.apache.ant</artifactId>
					   <type>eclipse-plugin</type>
					</dependency>
					<dependency>
					   <artifactId>org.eclipse.help.base</artifactId>
					   <type>eclipse-plugin</type>
					</dependency>
					<dependency>
					   <artifactId>org.eclipse.ant.core</artifactId>
					   <type>eclipse-plugin</type>
					</dependency>
				</dependencies>
			</configuration>
			<executions>
				<execution>
					<goals>
						<goal>eclipse-run</goal>
					</goals>
					<phase>compile</phase>
				</execution>
			</executions>
		</plugin>
	</plugins>
</build>

```

For other scenarios, it might be necessary to change the elements in `<dependencies>`. Also, note that the JVM arguments can be changed with the `<argLine>` paremeter.