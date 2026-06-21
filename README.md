 <h1> 
  <p align="center">
  chronoEventB
</p>
</h1>

The plugin **chronoEventB** permits to specify timing properties on Event-B models. Using this plugin, users have the possibililty to define _atomic_ tasks and tasks with _duration_ as specific Event-B events. To each task, _periodicity_ and _separation_ constaints may be attached. Moreover, ordering properties can be associated with a couple of tasks to state constaints on the occurence of their instances. These properties may be also timed.

## How to use chronoEventB
### Components 
   The plugin is composed of two main components:
   1. The Rodin extension that adds buttons to the Rodin menu for creating tasks as Event-B events and decorating them with timing constraints. 
   2. The theory Scheduler that defines a new Event-B data type named _TaskScheduler_ along with different operators allowing to manipulate it (last, append,etc.). This theory also defines a set of theorems and inference rules that help users to prove the correctness of timing properties on an Event-B component. 
   
   ### Installation
   #### Installation of the Rodin extension
   To extend Rodin with the chronoEventB plugin, follow these steps:
   1. Download and unzip the zip file plugin.
   2. Go to the menu _Help...Install New Software_, then click the button _Add_:

      <p align="center">
       <img width="400" height="400" alt="firstST" src="https://github.com/user-attachments/assets/11882d9a-3296-43bb-9ff4-0a53a5f4e21b" />
      </p>
      
   3. Click the button _Local_ and select the directory associated to the plugin:
 <p align="center">
      <img width="400" height="400" alt="sec" src="https://github.com/user-attachments/assets/7eaecbf0-ca59-4b59-8021-d7a6e84b2cef" />
   <img width="400" height="400" alt="last" src="https://github.com/user-attachments/assets/7b732e49-9a1b-454c-833d-95ce769486c7" />
 </p>

<p align="center">
 <img width="400" height="400" alt="last2" src="https://github.com/user-attachments/assets/c3b466b3-eb0f-4fc9-8b91-fe33fc2fc644" />
 </p>
   
   4. Select the plugin _ChronoEventB_ and press on _Next_ to complete the installation.

      <p align="center">
       <img width="400" height="400" alt="last3" src="https://github.com/user-attachments/assets/a170364c-ce34-4fcf-b449-1de9bc109470" />
       </p>
   #### Installation of the theory TaskScheduler
   Three steps are required: 
   1. Install the theory plugin: see [Here](https://wiki.event-b.org/index.php/Theory_Plug-in)
      
   2. Import the theory Scheduler under Rodin by selecting the entry _Import_ under the menu _File_: 

   <p align="center">
   <img width="500" height="500" alt="importThe" src="https://github.com/user-attachments/assets/f24eb979-439d-42fb-8588-ba29779c2b5e" />
   </p>
   
   The theory will then apprear under the _Event-B Explorer_ panel
    <p align="center">
   <img width="300" height="300" alt="importdone" src="https://github.com/user-attachments/assets/61221ad2-fb71-4ed2-b956-9c3a6ff2efe7" />
   </p>
  
  3. Create a project that defines a theory path to use the imported theory according to the follwoing steps:
  
  <p align="center">
     <img width="400" height="400" alt="createProj" src="https://github.com/user-attachments/assets/4f8d39b9-b301-464c-ae88-082aebe9a348" />
   <img width="500" height="500" alt="theoPath" src="https://github.com/user-attachments/assets/78d3b84f-6e72-4501-872b-f4d9dd9aed04" />
</p>

  4. Create an Event-B machine to have access to the menu that permits the creation of timed tasks as depicted below:
     
<p align="center">
   <img width="400" height="400" alt="createExample" src="https://github.com/user-attachments/assets/2e0b9a54-952a-4e0f-9cc4-7d9c5d655170" />
</p>

### chronoEventB by example
To create a task using our plugin, click on the  button **Te**. Then select its type. 
<p align="center">
<img width="1284" height="576" alt="atomicTask" src="https://github.com/user-attachments/assets/bd4c037e-7392-47fc-992c-3e5ec538d27b" />
</p>

**Case of an instantaneous task**: an instantaneous task does not take time to complete. The creation of an instantaneous task produces the following Event-B specificaition. To deal with timing properties, a master variable _CK_ along with a _Progress_ event are defined. This event makes time progress by a given non null amont of time _Step_. Of course, the master varaible and the associated event are created once while creating the first task.

   <p align="center">
   <img width="1486" height="766" alt="AttaskCreated" src="https://github.com/user-attachments/assets/d72f74f6-d62c-4fbc-a70d-2989713ebbc7" />
  </p>
  
   
For each task _A_,  we have  a variable _Scheduler_A_ which denotes an increasing sequence of value pairs (_start_, _end_) for each  execution of the task _A_. An instantaneous task  _A_ is represented by a unique event _Instantaneous_A_. Event _Instantaneous_A_ has the follwowing implicit guard/action:

guard: CK > last(_Scheduler_A_) //to ensure that two different instances of the task _A_ do not occur at the same time  
action: _Scheduler_A_:=_Append_(_Append_(_Scheduler_A_, _CK_), _CK_)//Append denotes the append operation of sequences

On an instantaneous task, a min/max timing periodicity constraint my be defined. The periodicity constraint denotes the min/max amount of time that separates the begining of two conseuctive instances. This constraint induces the following implicit guard: 

guard_per: _Scheduler_A_ $\neq$ $\emptyset$ $\Rightarrow$ CK - **BefLast**(_Scheduler_A_) $\geq$ / $\leq$ PSMin/PSMax //**BefLast**(_Scheduler_A_) denotes the before last element of the sequence _Scheduler_A_.
  
**Case of a task with duration**: the creation of a task with duration produces the following Event-B specificaition. Two events _Start_A__ and __End_A_ are defined. The _duration_ property (Minimun/maximum values DMin/DMax) is attached to the end event. These events include the following implicit guards/actions:

<p align="center">
<img width="600" height="120" alt="n" src="https://github.com/user-attachments/assets/7121ca69-12c3-4552-ba1f-c0d7794def10" />
</p>

1. **Event Start_A**:  
      guard: $\exists$ $k$. k $\in$ NAT $\wedge$ **size**(_Scheduler_A_) = $2k$

      action:  _Scheduler_A_:=_Append_(_Scheduler_A_, _CK_)

2. **Event End_A**:  
      guard: $\exists$ $k$. k $\in$ NAT $\wedge$ **size**(_Scheduler_A_) = $2k$ + 1 $\wedge$ _CK_ - **last**(_Scheduler_A_)  { $\geq$, $\leq$} {_DMin_ , _DMax_}
   
      action:  _Scheduler_A_:=_Append_(_Scheduler_A_, _CK_)

 To both instantaneous and non instantaneous tasks, two additional properties can be attached:
 (a) Seperation: puts a constraint on the elapsed time between the end of an ocurrence and the start of the  next one. 
 (b) Periodicity: puts a constraint on the elapsed time between the starts of two consecutive occurences. This constraint is associated with the start event. 

Both properties are attached to the _Start_A_ (resp. _Instantaneous_A_) event for non-instantaneous (resp. instantaneous) task. They induce implicit invariants/guards than can be displayed using [ProB](https://stups.hhu-hosting.de/rodin/prob1/nightly).

**Bi-task properties**
On two distinct tasks, two ordering constraints can be defined: _FollowedBy_ and _PrecededBy_. A task _A_ must be followed by a task _B_ means that it should exist an occurence of the task _B_ between evry two distinct occurences of _A_. The figure bellow depicts how such dependecy is created. Depending on the task type, users have to select the event _Instantaneous_A_ (resp. _End_A_) for an instantaneous task (resp. task with duration).
   
<p align="center">
  <img width="788" height="506" alt="follow" src="https://github.com/user-attachments/assets/67fb0598-afe6-4c7e-a3d3-a32144df940e" />
</p>



Creating such constraint makes the related event decorated with a _FOLLOWED_BY_ tag that permits a user select the desired task and its status complete/incomplete (_only start _/_start and end_). Moroerver, there are two optional information to state the min/max time that should elapse between occurences of _A_ and _B_. 

<p align="center">
<img width="600" height="120" alt="followB" src="https://github.com/user-attachments/assets/a8a35bfe-5d64-4c5f-8ee3-08eb4cb64ec5" />
</p>

Constraint _PrecededBy_ is similar to the constraint _FollowedBy_. A task _A_ is preceded by a task _B_ means that it should exist an occurence of the task _B_  that started (or started and accomplished its execution) before begining each occurence of _A_.
 
  ## Contributors
  Amel Mammar, amel.mammar@telecom-sudparis.eu, IP Paris/Télécom SudParis  
  Feukeng Mono Cabrel, feukengcabrel13@gmail.com , IP Paris/Télécom SudParis
  
   
