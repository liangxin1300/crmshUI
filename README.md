## Motivaiton
Crmsh is a full function command-line interface for HA cluster management.<br>
From my opinion, some sublevels/subcommands need to change or improve,<br>
to make the command-line UI more simple and more easier to use.<br>
Certainly I want to do these while keeping original sublevels/subcommands **compatiable**.<br>
But we will not provide auto completion for the original syntax.<br><br>
All of navigate/help subcommands like "cd"/"help"/"ls"/"quit"/"up" will not list here.<br>
Although I suggest dropping the "cd" and "ls" subcommands when at interactive mode,<br>
cause we will use prompt_toolkit to provide completion, which can do navigate job very well.
## root level
<pre>
crm
    cluster		<b>Cluster setup and management</b>
    configure		<b>Cib configuration</b>
    node		<b>Node management</b>
    options		<b>Configure crmsh itself</b>
    resource		<b>Resource management</b>
    status		<b>CLuster status</b>
    tools		<b>Cluster tools</b>
</pre>
Personally, I think the sublevels in root level are probably too many.<br>
It looks clear and simple that just leave the most frequently used.
#### Diff table(1) in root level
|name|description|original| plan| explain|
|--|--|--|--|--|
|<del>cib</del>|CIB shadow management|crm>cib|crm>configure>shadow|<ul><li>seems less used, not suitable in root level</li><li>"shadow" is more specific</li><li>"cibshadow" is also fine</li></ul>|
|<del>cibstatus</del>|CIB status management and editing|crm>cibstatus|crm>configure>cibstatus|<ul><li>seems less used, not suitable in root level</li></ul>|
|<del>corosync</del>|Corosync management|crm>corosync|crm>configure>corosync|<ul><li>seems less used, not suitable in root level</li></ul>|
|<del>history</del>|Cluster history|crm>history|crm>tools>history|<ul><li>should better put into "tools" level</li></ul>|
|<del>maintenance</del>|Maintenance mode commands|crm>maintenance|crm>configure>property|<ul><li>maintenance here is actually a cluster property "maintenance-mode"</li><li>Thinking whether we should create some shortcuts subcommands for those important and frequently used property?</li></ul>|
||<pre>                  </pre>|||<pre>                                       </pre>|
#### Diff table(2) in root level
|name|description|original| plan| explain|
|--|--|--|--|--|
||||||
|<del>ra</del>|Resource Agents lists and doc|crm>ra|crm>configure>resource>agent|<ul><li>should better put it into specific level, not root</li></ul>|
|<del>report</del>|Create cluster status report|crm>report|crm>tools>report|<ul><li>should better put into "tools" level</li></ul>|
|<del>script</del>|Cluster script management|crm>script|None|<ul><li>have no idea yet; maybe crm>configure>wizard ?</li><li>just feel that it's not suitable in root level</li><li>maybe rewrite it using Salt, to make a better "wizard"</li></ul>|
|<del>site</del>|GEO clustering site support|crm>site|crm>cluster>geo>ticket|<ul><li>should better put into "cluster>geo" level</li></ul>|
|<del>verify</del>|Verify cluster status|crm>verify|crm>tools>verify|<ul><li>should better put into "tools" level</li></ul>|
|**tools**|Cluster tools|None|crm>tools|<ul><li>include useful but less used tools, like "report","history","verify","health" and "test"</li><li>while "test" is related to the FATE#321073</li></ul>|
||<pre>            </pre>|||<pre>                                               </pre>|
---
## cluster level
<pre>
cluster
    add			<b>Add a new node to the cluster</b>
    file		<b>Cluster file management</b>
        copy		<b>Copy file to other cluster nodes</b>
        diff		<b>Diff file across cluster</b>
    geo			<b>Geo cluster setup and management</b>
        init		<b>Configure cluster as geo cluster</b>
        init_arbitrator <b>Initialize node as geo arbitrator</b>
        join		<b>Join cluster to existing geo cluster</b>
        ticket		<b>GEO cluster tickets management</b>
    init		<b>Initialize a new HA cluster</b>
    join		<b>Join existing cluster</b>
    remove		<b>Remove node(s) from the cluster</b>
    rename		<b>Rename node(s) from the cluster</b>
    service		<b>Cluster service management(start;stop;enable;disable;status;wait_for_startup)</b>
</pre>
#### Diff table(1) in cluster level
|name|description|original|plan|explain |
|--|--|--|--|--|
||||||
|**file**|Cluster file management|None|cluster>file|<ul><li>the object of copy and diff is "file"</li></ul>|
|<del>copy</del>|Copy file to other cluster nodes|cluster>copy|cluster>file>copy|<ul><li>the object of copy is "file"</li><li>seems less used</li></ul>|
|<del>diff</del>|Diff file across cluster|cluster>diff|cluster>file>diff|<ul><li>the object of diff is "file"</li><li>seems less used</li></ul>|
|**service**|Cluster service management|None|cluster>service|<ul><li>This is a sub-command, with options "start"/"stop"/"disable"/"enable"/"status"/"wait_for_startup"</li><li>It's more specific for service operations</li></ul>|
|<del>disable</del>|Disasble cluster services|cluster>disable|cluster>service|<ul><li>"disable" is an option for cluster service command</li></ul>|
|<del>enable</del>|Enable cluster services|cluster>enable|cluster>service|<ul><li>"enable" is an option for cluster service command</li></ul>|
|<del>start</del>|Start cluster services|cluster>start|cluster>service|<ul><li>"start" is an option for cluster service command</li></ul>|
|<del>stop</del>|Stop cluster services|cluster>stop|cluster>service|<ul><li>"stop" is an option for cluster service command</li></ul>|
|<del>status</del>|Cluster status check|cluster>status|cluster>service|<ul><li>"status" is an option for cluster service command</li></ul>|
|<del>wait_for_startup</del>|Wait for cluster to start|cluster>wait_for_startup|cluster>service|<ul><li>"wait_for_startup" is an option for cluster service command</li></ul>|
||<pre>               </pre>|||<pre>                                       </pre>|
#### Diff table(2) in cluster level
|name|description|original|plan|explain |
|--|--|--|--|--|
|**geo**|Geo cluster setup and management|None|cluster>geo|<ul><li>Create a specific sublevel for GEO cluster</li></ul>|
|<del>geo_init</del>|Configure cluster as geo cluster|cluster>geo_init|cluster>geo>init|<ul><li>move to the new geo sublevel</li></ul>|
|<del>geo_init_arbitrator</del>|Initialize node as geo cluster arbitrator|cluster>geo_init_arbitrator|cluster>geo>init_arbitrator|<ul><li>move to the new geo sublevel</li></ul>|
|<del>geo_join</del>|Join cluster to existing geo cluster|cluster>geo_join|cluster>geo>join|<ul><li>move to the new geo sublevel</li></ul>|
|**geo>ticket**|GEO cluster tickets management|crm>site|cluster>geo>ticket|<ul><li>move to the new geo sublevel</li></ul>|
|<del>health</del>|Cluster health check|cluster>health|crm>tools>health|<ul><li>move to the cluster tools sublevel</li></ul>|
|<del>run</del>|Execute a command on node/nodes|cluster>run|crm>tools>run|<ul><li>move to the cluster tools sublevel</li></ul>|
||<pre>               </pre>|||<pre>                                     </pre>|
---
## configure level
<pre>
configure
    acl		   	<b>Define target access rights</b>
    alert		<b>Event-driven alerts</b>
    cibstatus		<b>Cib status management and editing</b>
    commit
    corosync		<b>Corosync management</b>
    delete
    edit
    fencing_topology
    node		<b>Define a cluster node</b>
    resource		<b>Define a cluster resource</b>
    constraints		<b>Define resource constraints</b>
    property
    shadow		<b>Cib shadow management</b>
</pre>
#### Diff table(1) in configure level
|name|description|original|plan|explain|
|--|--|--|--|--|
|**acl**|Define target access rights|crm>configure>acl__target|
#### corosync sublevel
<pre>
configure
    corosync		<b>Corosync management</b>
        diff
	edit
	get
	log
	pull
	push
	reload
	set
	show
	status

</pre>
#### resource sublevel
<pre>
configure
    resource
        primitive	<b>primitive resource</b>
        group		<b>group resource</b>
        clone		<b>clone resource</b>
        master		<b>master resource</b>
        bundle		<b>bundle resource</b>
        rsc_template	<b>resource template</b>
        rsc_defaults	<b>Set resource meta defaults</b>
        op_defaults	<b>Set resource operations defaults</b>
        agent		<b>Resource Agents lists and documentation</b>
</pre>
#### constraints sublevel
<pre>
configure
    constraints		<b>Define resource constraints</b>
        location
        colocation
        order
        ticket
</pre>
#### Diff table(1) in configure level
|name|description|original|plan|explain|
|--|--|--|--|--|
|**corosync**|corosync management|crm>corosync|crm>configure>corosync|<ul><li>seems less used, not suitable in root level</li></ul>|
|<del>add-node</del>|Add a corosync node|crm>corosync>add-node|None|<ul><li>Confuse with cluster add? Keep one?</li></ul>|
|<del>del-node</del>|Remove a corosync node|crm>corosync>del-node|None|<ul><li>Confuse with cluster remove? Keep one?</li></ul>|
|**diff**|Diffs the corosync configuration|crm>corosync>diff|crm>configure>corosync>diff|<ul><li>move to new corosync sublevel</li></ul>|
|**edit**|Edit the corosync configuration|crm>corosync>edit|crm>configure>corosync>edit|<ul><li>move to new corosync sublevel</li></ul>|
|**get**|Get a corosync configuration value|crm>corosync>get|crm>configure>corosync>get|<ul><li>move to new corosync sublevel</li></ul>|
|**log**|Show the corosync log file|crm>corosync>log|crm>configure>corosync>log|<ul><li>move to new corosync sublevel</li></ul>|
|**pull**|Pulls the corosync configuration|crm>corosync>pull|crm>configure>corosync>pull|<ul><li>move to new corosync sublevel</li></ul>|
|**push**|Push the corosync configuration|crm>corosync>push|crm>configure>corosync>push|<ul><li>move to new corosync sublevel</li></ul>|
|**reload**|Reload the corosync configuration|crm>corosync>reload|crm>configure>corosync>reload|<ul><li>move to new corosync sublevel</li></ul>|
|**set**|Set a corosync configuration value|crm>corosync>set|crm>configure>corosync>set|<ul><li>move to new corosync sublevel</li></ul>|
|**show**|Display the corosync configuration|crm>corosync>diff|crm>configure>corosync>show|<ul><li>move to new corosync sublevel</li></ul>|
|**status**|Display the corosync status|crm>corosync>diff|crm>configure>corosync>status|<ul><li>move to new corosync sublevel</li></ul>|
||<pre>               </pre>|<pre>                  </pre>||<pre>                            </pre>|
#### Diff table(2) in configure level
|name|description|original|plan|explain |
|--|--|--|--|--|
||||||
|**resource**|Define a cluster resource|None|crm>configure>resource|<ul><li>lots of resource types, should be classified</li></ul>|
|**primitive**|Define a primitive resource|crm>configure>primitive|crm>configure>resource>primitive|<ul><li>move to the configure resource sublevel</li></ul>|
|**group**|Define a group|crm>configure>group|crm>configure>resource>group|<ul><li>move to the configure resource sublevel</li></ul>|
|**clone**|Define a clone|crm>configure>clone|crm>configure>resource>clone|<ul><li>move to the configure resource sublevel</li></ul>|
|**master**|Define a master resource|crm>configure>master|crm>configure>resource>master|<ul><li>move to the configure resource sublevel</li></ul>|
|**bundle**|Define a container resource|crm>configure>bundle|crm>configure>resource>bundle|<ul><li>move to the configure resource sublevel</li></ul>|
|**rsc_template**|Define a resource template|crm>configure>rsc_template|crm>configure>resource>rsc_template|<ul><li>move to the configure resource sublevel</li></ul>|
|**rsc_defaults**|Set resource meta defaults|crm>configure>rsc_defaults|crm>configure>resource>rsc_defaults|<ul><li>move to the configure resource sublevel</li></ul>|
|**op_defaults**|Set resource operations defaults|crm>configure>op_defaults|crm>configure>resource>op_defaults|<ul><li>move to the configure resource sublevel</li></ul>|
|**agent**|Resource Agents documentation|crm>configure>ra|crm>configure>resource>agent|<ul><li>more specific against "ra"</li><li>should in resource sublevel</li></ul>|
||<pre>             </pre>|||<pre>                          </pre>|
#### Diff table(3) in configure level
|name|description|original|plan|explain |
|--|--|--|--|--|
|**constraints**|Define resource constraints|None|crm>configure>constraints|<ul><li>should better be classified</li></ul>|
|**location**|A location preference|crm>configure>location|crm>configure>constraints>location|<ul><li>move to the configure constraints sublevel</li></ul>|
|**colocation**|Colocate resources|crm>configure>colocation|crm>configure>constraints>colocation|<ul><li>move to the configure constraints sublevel</li></ul>|
|**order**|Order resources|crm>configure>order|crm>configure>constraints>order|<ul><li>move to the configure constraints sublevel</li></ul>|
|**rsc_ticket**|Resources ticket dependency|crm>configure>rsc_ticket|crm>configure>constraints>rsc_ticket|<ul><li>move to the configure constraints sublevel</li></ul>|
||<pre>           </pre>|||<pre>                   </pre>|
#### Diff table(4) in configure level
|name|description|original|plan|explain |
|--|--|--|--|--|
|**shadow**|Cib shadow management|crm>configure>cib|crm>configure>shadow|<ul><li>"shadow" is more specific</li><li>"cibshadow" is also fine</li></ul>|
---
## node level
<pre>
node
    status
</pre>
#### Diff table in node level
| name | original | plan | explain |
|----------|----------|---------|---------|
---
## resource level
<pre>
resource
    ban			<b>Ban a resource from a node</b>
    classify
    cleanup		<b>Cleanup resource status</b>
    clear		<b>Clear any relocation constraint</b>
    constraints		<b>Show constraints affecting a resource</b>
    demote		<b>Demote a master resource</b>
    failcount		<b>Manage failcounts</b>
    locate		<b>Show the location of resource</b>
    maintenance		<b>Enable/disable pre-resource maintenance mode</b>
    manage		<b>Put a resource into managed mode</b>
    meta		<b>Manage a meta attribute</b>
    move		<b>Move a resource to another node</b>
    operations		<b>Show active resource operations</b>
    param		<b>Manage a parameter of a resource</b>
    promote		<b>Promote a master resource</b>
    refresh		<b>Recheck current resource status and drop failure history</b>
    restart		<b>Restart resources</b>
    scores		<b>Display resource scores</b>
    secret		<b>Mange sensitive parameters</b>
    start		<b>Start resources</b>
    status		<b>Show status of resources</b>
    stop		<b>Stop resources</b>
    trace		<b>Start RA tracing</b>
    unmanage            <b>Put a resource into unmanage mode</b>
    untrace		<b>Stop RA tracing</b>
    utilization		<b>Manage a utilization attribute</b>
</pre>
#### Diff table in resource level
| name | original | plan | explain |
|----------|----------|---------|---------|
---
## tools level
```
tools
    report
    history
    test
```
#### Diff table in tools level
| name | original | plan | explain |
|----------|----------|---------|---------|
