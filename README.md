



**Introduction: **
To achieve the Continuous Integration Environment for the .NET platform, dot net C# project should be configured and Build/test in Hudson Server using maven and view the results in sonar.  


**Technology/tools:**


<img width="1076" alt="Screenshot 2021-11-14 at 10 35 24 PM" src="https://user-images.githubusercontent.com/66691402/141690881-83c96c22-3258-4ac4-b2cc-0942d70336d1.png">


Steps to achieve the CI Environment:
The environment is achieved by referring to the website (Refer: http://docs.codehaus.org/display/SONAR/.Net+plugin?showChildren=false).
Create a dot net c# project with the below tree structure:
A sample File Tree Solution:
+ my-solution.sln 
 |
 + pom.xml
 |
 + example-project
 |     |
 |     + example-project.csproj
 |     + namespaces...
 |
 + other-project


**1. Install maven:**
Configure the settings.xml in installed path 
Ex:
  <proxies>
    <proxy>
      <id>mysys</id>
      <active>true</active>
      <protocol>http</protocol>
      <username>bka</username>
      <password>Password@2010</password>
      <host>10.249.11.10</host>
      <port>8080</port>
      <nonProxyHosts>local.net|some.host.com</nonProxyHosts>
    </proxy>    
  </proxies>

**2. Install Sonar:**
	a) Edit the sonar.bat file in path ($SONAR_HOME\bin\windows-x86-32) and set jdk path in file "StartSonar.bat".
	b) Start the sonar by firing the "StartSonar.bat" file.


**3. Install and configure Hudson server:**
	Configure the plugins in 'manage Hudson" -> "configure system" ->
	a) Set path for maven:
		Ex: D:\Raghunathan\softwares\apache-maven-2.2.1 
	b) Set path for jDK:
		EX: D:\Raghunathan\softwares\jdk1.6.0_07
	c) Set path for MSBUILD:
		EX: C:\WINDOWS\Microsoft.NET\Framework\v3.5\MSBuild.exe
	d) Set path for Sonar:
		EX: D:\Raghunathan\softwares\sonar-2.0.1
	e) Install the following plugins through 'manage Hudson" - > "Manage Plugins" ->
		i)  Hudson Sonar Plug-in and Hudson MSBuild Plug-in.
		ii) Configure the "http proxy configuration" i.e. (server, port, username and password).
		iii) Start the Hudson server:
			Ex: D:\softwares>set path=D:\Raghunathan\softwares\jdk1.6.0_07\bin;
			      D:\softwares>java -jar hudson.war
Note:	Set the path "C:\hudson" and start the Hudson server (To avoid build errors)
			Ex: java -DHUDSON_HOME=C:\hudson -jar hudson.war
      
<img width="1102" alt="Screenshot 2021-11-14 at 10 35 36 PM" src="https://user-images.githubusercontent.com/66691402/141690889-2777ce6c-2bcf-45ef-a51f-5b60a95c42e6.png">

**4. Deploy Plugins**
  a). Deploy dotnet plugins Deploy the below “dotnet plug-in”   jar files in the installed path “$m2\repository\org\codehaus\maven” folder. Plugins: i) "dot-net            common"  , “maven-dotnet-runtime” , “maven-dotnet-plug-in”.
  b) Deploy Sonar Plugins: Deploy the below “sonar dotnet plugins”  jar files in "$SONAR_HOME\extensions\plugins" folder. 
     Plugins: dotnet common , maven-dotnet-  runtime , sonar-plug-in-dotnet-core jar, sonar-plug-in-dotnet-cpd jar, sonar-plug-in-fxcop jar, sonar-plug-in-gallio        jar, sonar-plug-in-partcover jar, sonar-plug-in-sourcemonitor jar and sonar-plug-in-stylecop jar.
  c) Deploy Maven-Dotnet-Plug-in :
      Deploy  Maven-Dotnet-Plug-in jar files and Pom file in maven installed folder ".m2\repository\org\codehaus\sonar-plugins \dotnet". 
      The folder and file structure inside to dotnet should be 
      ex: 
          maven-dotnet\0.1\maven-dotnet-0.1.pom
          maven-dotnet-plug-in\0.1\maven-dotnet-plugin-0.1.jar	
          The same folder structure should be followed for "dotnet-common" and "maven-dotnet-runtime" jar files.
          
**5. Configuration and Maven Build for .net project with Hudson server:**
     a) Configure settings.xml file in 2 places one in path "$apache-maven\apache-maven-2.2.1\conf" as well another in installed path. Refer : http://maven-dotnet-          plugin.appspot.com/settings.html
     b) Configure Pom.xml in the root folder of the dot net application referring to the above tree structure.
         Refer : http://maven-dotnet-plugin.appspot.com/configuration.html
         Note: The Pom.xml, Solution file,  Project folder should be in the same folder. Configure the pom.xml  by referring to the above website.	
     c) Schedule a new free-style software project job (Refer: Image below).


<img width="1176" alt="Screenshot 2021-11-14 at 10 36 38 PM" src="https://user-images.githubusercontent.com/66691402/141690853-7cf431a1-eac4-49d0-8366-f0c87d90bf64.png">

      d) Copy the entire Dot net c# code into the installed Hudson workspace folder.  
      e) Select Freestyle project - > configure - > Add build step -> Invoke Top-level Maven targets and set goals as "dotnet: compile" (Refer: Image below).


<img width="1136" alt="Screenshot 2021-11-14 at 10 36 42 PM" src="https://user-images.githubusercontent.com/66691402/141690866-ceec75cc-4b06-4497-9f0d-4bef0c1d0348.png">

      f) Build the Project. Two ways to achieve the Build :
            i)Using the command in cmd prompt ”mvn dotnet:compile” (Refer Sample Output below) and  launch the command “mvn sonar:sonar” for code quality reports.               (Refer : http://maven-dotnet-plugin.appspot.com/index.html). Apache maven path : D:\SampleDotnet\TestSolution> set path=%path%;D:\softwares\apache-maven-2.2.1\bin;
            ii) Using Hudson UI (Refer: Image below).


<img width="1084" alt="Screenshot 2021-11-14 at 10 35 45 PM" src="https://user-images.githubusercontent.com/66691402/141690903-3eed6716-238e-48f5-acd2-8f463a054e32.png">


**6) Integrate sonar Plug-in with Hudson Server.**
         a) Freestyle Project -> Configure -> Post-build Actions -> Make checkbox as checked for "Sonar" (Refer: Image 5). 


<img width="935" alt="Screenshot 2021-11-14 at 10 35 55 PM" src="https://user-images.githubusercontent.com/66691402/141690912-06912f90-e625-4963-8607-c36b17267076.png">

          b) Sonar - > Advanced - > Select the Maven in “Maven Version” dropdown.	(If you were not able to find the maven in the dropdown. Build the project and select the maven version). 
         c) Create a c# profile in sonar and make it as default (to compile the .net projects) (Refer: Image 7).


<img width="914" alt="Screenshot 2021-11-14 at 10 36 02 PM" src="https://user-images.githubusercontent.com/66691402/141690917-7f437f15-150e-4190-a3b8-a7fcf8718048.png">


**7) View the Results in sonar:**
        c) Build the project on the Hudson server. 
        d) View the results in sonar (Refer: Image below).


<img width="923" alt="Screenshot 2021-11-14 at 10 36 08 PM" src="https://user-images.githubusercontent.com/66691402/141690929-6e36b358-1a49-42f8-8143-3eef5b90d14a.png">




<img width="938" alt="Screenshot 2021-11-14 at 10 36 14 PM" src="https://user-images.githubusercontent.com/66691402/141690942-84828aab-b680-4ace-8c4e-fe68bbc53099.png">



<img width="947" alt="Screenshot 2021-11-14 at 10 36 20 PM" src="https://user-images.githubusercontent.com/66691402/141690953-c100d51e-2b10-467a-ae31-8dab605a0860.png">



**** 8) Build results in command Prompt:****

D:\ProSolution>mvn dotnet:compile

[INFO] Scanning for projects...

[INFO] ------------------------------------------------------------------------

[INFO] Building test Pro solution

[INFO]    task-segment: [dotnet:compile]

[INFO] ------------------------------------------------------------------------

[INFO] [dotnet:compile {execution: default-cli}]

[INFO] Launching the build of D:\Raghunathan\ProSolution\ProSolution.sln

[INFO] Build of ProSolution.sln in configuration debug terminated with SUCCESS!

[INFO] Launching the build of D:\Raghunathan\ProSolution\ProSolution.sln

[INFO] Build of ProSolution.sln in configuration release terminated with SUCCESS!

[INFO] ------------------------------------------------------------------------

[INFO] BUILD SUCCESSFUL

[INFO] ------------------------------------------------------------------------

[INFO] Total time: 7 seconds

[INFO] Finished at: Tue Jun 01 15:55:28 IST 2010

[INFO] Final Memory: 6M/13M




