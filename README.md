# JcgEks Artifact description
This is the artifact of **Hybrid Regression Test Selection by Integrating File and Method 
  Dependences** in ASE 2024. 

The artifact is provided as a docker image, which contains the prototype of the proposed method and benchmarks used for evaluation. The aim is to assist the reviewers in reproducing the experimental results in our evaluation.

# Prerequisites
+ Hardware: ~2.50 GHz CPU (all experiments were performed on a server with Intel(R) Xeon(R) Platinum 8269CY CPU @ 2.50GHz, 80 Cores, 192GB), 200+GB free disk space.
+ Unix / Linux OS: We have validated the artifact on Ubuntu system
+ [Docker](https://www.docker.com/pricing/)

# Running the artifact
We assume that the following commands are run in sudo mode. 

Firstly, pull the already prebuilt docker image from [docker hub](https://hub.docker.com/r/ase24rts/jcgeks/tags). Please make sure its name is `ase24rts/jcgeks`.
```sh
$ docker pull ase24rts/jcgeks:v1.2
```

If everything is ok, a `ase24rts/jcgeks:v1.2` image should be found in the images listed by `docker images` command. Then, you can create a container of such image and start a Bash session using the following command. An interactive `bash` shell on the container is also executed at the moment.
```sh
$ docker run -i -t ase24rts/jcgeks:v1.2 bash
```

If all goes well, the container should be running successfully. Otherwise, you can seek help from [Docker Doc](https://docs.docker.com/) if needed. 

First, let's take a look at the benchmark list of our experiment
```sh
$ cd /home/aaa/projects/ && ls
commons-codec/  commons-imaging/  commons-rng/  gerrit-events/
```

Please note that our complete experiment included 20 projects, but in order to limit the size of the Docker (mainly due to the large dependency of Maven), we only provided 4 smaller projects. 

If you need to replicate all the experiments, please use `git` to pull the corresponding project names mentioned in the paper in this `/home/aaa/projects` directory.

Now, navigate into the directory containing our experimental environment and list the contents. 
```sh
$ cd /home/aaa/scripts/ && ls
fin_config/  fin_projects.txt  README.md  RTSTool.py
```
The necessary items to reproduce our evaluation are listed, including benchmark programs and scripts. The rest of this README assumes you are working in `/home/aaa/scripts`.

***
Note that all running tools have been installed in the local Maven repository and can be extracted if needed.
***

The `fin_config` directory contains the running configuration and corresponding commit for all projects; `fin_projects.txt` is used to provide `RTSTool.py` with the name of the project that needs to be exprimented.

# Reproducing experimental results

There are 4 benchmarks in our docker: `commons-codec`, `commons-imaging`, `commons-rng`, `gerrit-events`. Hence, we provide a script to reproduce experimental results. 

In view of the long running time of our evaluation, the scripts try to perform all experiments in parallel. Specifically, the scripts take a user-defined value *n* and exploit at most *n* processes of working machine at run time. The value should be carefully considered to avoid resource exhaustion. In our evaluation, *n* is 8, since all experiments were performed on a 80-core server with 192GB memory.

**Reproduce experimental results:**

```bash
$ nohup python3 RTSTool.py &  
# wait......
```

The 4 projects analysis will take approximately **20 core-hours**. If you run one CPU core for one hour, that is one core-hour. 

The whole projects(20 projects) analysis will take approximately **800 core-hours**.

After the analysis is completed, the script will create corresponding files for each benchmark to output the running results, as shown below.

```bash
$ ls
fin_config/ RTSTool.py  fin_projects.txt   merge_project_total.csv   README.md
```

The `rts_info` directory in each project contains all running logs of all tools, , such as the `commons-codec/rts_info` directory. The result of analyzing these logs is the `merge_project_total.csv` file.

The content of the `merge_project_total.csv` is shown below, which displays the running information of each tool on each commit under each project.

```bash
/home/aaa/projects/commons-imaging,:,commons-imaging
 ,|,-,-,-,exec time,-,-,-,-,|,-,-,-,testclass num,-,-,-,-,|,-,-,-,run num,-,-,-,-,|, ,jcg callgraph time,|,-,-,-,soundness,-,-,|,-,-,-,lost set,-,-,|
CommitID,|,check,all,ori,jcg,jcgne,fine,hyrts,starts,|,check,all,ori,jcg,jcgne,fine,hyrts,starts,|,check,all,ori,jcg,jcgne,fine,hyrts,starts,|,jcg time,jcgnetime,|,ori,jcg,jcgne,fine,hyrts,starts,|,ori,jcg,jcgne,fine,hyrts,starts,|
ca8be30b,|,43.659,44.245,40.413,42.991,50.119,43.789,,41.734,|,153 -- 0,153,153,153,153,153,0,153,|,979,979,979,979,979,979,,979,|,1.29,1.343,|,True,True,True,True,True,True,|,0,0,0,0,0,0,|,N/A,N/A,N/A,N/A,N/A,N/A,|,0
b6618f2c,|,44.125,43.252,7.928,9.397,16.21,9.804,,40.122,|,6 -- 5,153,6,6,6,6,0,80,|,19,979,19,19,19,19,,764,|,2.612,3.526,|,True,True,True,True,False,True,|,0,0,0,0,5,0,|,0.0,0.0,0.0,0.0,100.0,0.0,|,5
......
total,|,-,-,-,total time,-,-,-,-,|,-,-,-,total testclass num,-,-,-,-,|,-,-,-,total run num,-,-,-,-,|,total jcg time,-,|,-,-,total lost num,-,-,-,|
total,|,2234.11,2223.53,796.6,494.38,527.86,462.07,0,1144.67,|,435,7497,435,198,198,194,0,2061,|,5090,47971,5090,2136,2136,2068,0,19565,|,89.27,90.63,|,0,0,0,1,85,0,|
......

......

/home/aaa/projects/commons-codec,:,commons-codec
,|,-,-,-,exec time,-,-,-,-,|,-,-,-,testclass num,-,-,-,-,|,-,-,-,run num,-,-,-,-,|, ,jcg callgraph time,|,-,-,-,soundness,-,-,|,-,-,-,lost set,-,-,|
CommitID,|,check,all,ori,jcg,jcgne,fine,hyrts,starts,|,check,all,ori,jcg,jcgne,fine,hyrts,starts,|,check,all,ori,jcg,jcgne,fine,hyrts,starts,|,jcg time,jcgne time,|,ori,jcg,jcgne,fine,hyrts,starts,|,ori,jcg,jcgne,fine,hyrts,starts,|
7ca5b56e,|,45.191,59.85,43.238,42.305,42.638,46.821,,39.49,|,62 -- 0,62,62,62,62,62,0,62,|,1156,1156,1156,1156,1156,1156,,1156,|,0.844,0.753,|,True,True,True,True,True,True,|,0,0,0,0,0,0,|,N/A,N/A,N/A,N/A,N/A,N/A,|,0
2bacd0ba,|,43.224,44.194,3.493,3.432,3.605,5.685,,3.738,|,0 -- 0,62,0,0,0,0,0,0,|,0,1156,0,0,0,0,,0,|,0.0,0.0,|,True,True,True,True,True,True,|,0,0,0,0,0,0,|,N/A,N/A,N/A,N/A,N/A,N/A,|,0
......
total,|,-,-,-,total time,-,-,-,-,|,-,-,-,total testclass num,-,-,-,-,|,-,-,-,total run num,-,-,-,-,|,total jcg time,-,|,-,-,total lost num,-,-,-,|
total,|,2395.72,2384.52,486.99,423.0,448.71,419.54,0,487.48,|,254,3128,254,179,179,169,0,395,|,6308,61144,6308,4694,4694,4453,0,8909,|,41.54,42.64,|,0,0,0,1,138,0,|

```

The table will list the maven testing time, number of test classes, number of test cases, static analysis time (only for jcgeks's 2 configurations), safety(T/F), and the number of missed test classes.

Finally, after the `total` column, the cumulative result of all information will be listed.

Taking the `commons-imaging` project in the table as an example, its first commit ID is ca8be30b, and then the running time of each task is listed. The checker tool runs for 43.659s, the testall runs for 44.425s, and so on.

# Analyzing single program
If you are interested in several revisions of a benchmark program, e.g., `commons-imaging`(ca8be30b -> b6618f2c -> 23091f99), please navigate to the `scripts` directory, modify the `fin_projects.txt` and `commons-imaging.config` configuration files first, then invoke `RTSTool.py` script.

```bash
$ vi fin_projects.txt
commons-imaging
# Write only one project name, which needs to correspond to the file name in fin_config/

$ vi fin_config/commons-imaging.config
......
=== commit list begin ===
ca8be30b
b6618f2c
23091f99
......
......
=== commit list end ===
# Write the commit IDs you are interested in between these two flags, allowing you to write any ID and execute it in order from top to bottom
```

After modifying these two configuration files(`fin_projects.txt` and `commons-imaging.config`), you can run the RTSTool.py script.

```bash
nohup python3 RTSTool.py &
```

The final analysis results will still appear in the `merge_project_total.csv` file

# Using different RTS tools and Checker in the project

We provide a set of Maven programs in Docker, and the pom.xml of these programs shows how to use different RTS tools

How to configure other RTS tools can refer to the pom.xml file in the corresponding project.

```sh
$ cd /home/aaa/examples/ && ls
CheckerExample  EkstaziExample  FineeksExample  JcgeksExample  JcgeksNEExample
```

Taking `JcgEks` as an example, we demonstrate how to configure Maven plugins in a project to achieve the desired results for regression testing selection.

```sh
$ vi /home/aaa/examples/JcgeksExample/pom.xml
......
<build>
  <plugins>
    <plugin>
      <groupId>org.jcgeks</groupId>
      <artifactId>jcgeks-maven-plugin</artifactId>
      <version>1.0.0</version>
      <executions>
        <execution>
          <id>jcgeks</id>
          <goals>
            <goal>select</goal>
            <goal>restore</goal>
          </goals>
        </execution>
      </executions>
    </plugin>

    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-surefire-plugin</artifactId>
      <version>2.22.2</version>
      <configuration>
        <excludesFile>${java.io.tmpdir}/EkstaziExcludes</excludesFile>
      </configuration>
    </plugin>
  </plugins>
</build>
......
```

After adding the configuration of the above plugins in pom.xml, the regression testing plugin can be used.

The cache clearing and regression testing commands for different RTS tools are shown in the following table:

| Tool     | clean cache           | regression testing |
|----------|-----------------------| ------------------- |
| Ekstazi  | mvn ekstazi:clean     | mvn test   |
| JcgEks   | mvn jcgeks:clean      | mvn test   |
| JcgEksNE | mvn jcgeksnoext:clean | mvn test   |
| FineEks  | mvn ekstazi:clean     | mvn test   |
| Checker  | mvn rtschecker:clean  | mvn test   |


# Using RTSChecker's autoep to check the RTS tools' safety

Please refer to the specific usage of RTSChecker for details : https://github.com/EngineeringSoftware/rtscheck

We provide a replication package in Docker, which only requires running the following command to output the total result of violating 7 rules in the command line (Running this experiment may take more than 4 days).
```sh
$ cd /home/aaa/autoep/
$ ./main
```

The complete experimental results `RTSCheck_Results.tar.xz` can be found in this GitHub repository. 

Please note that we have identified a bug in the implementation of the `autoep` tool and have also provided error report instructions `RTSCheckBugReport.md` and error reproduction code package `RTSCheckBugReport.tar.xz` in this Git repository.