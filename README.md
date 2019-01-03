## Motivaiton
Crmsh is a full function command-line interface for HA cluster management.<br>
From my opinion, some sublevels/subcommands need to change or improve,<br>
to make the command-line UI more simple and more easier to use.<br>
Certainly I want to do these while keeping original sublevels/subcommands **compatiable**.<br>
But we will not provide auto completion for the original syntax.<br><br>
In this RFC, I have deleted many subcommands, in some level, more than 50% subcommands have been deleted.<br>
We should review and disscus, whether it worth or make sense to pull some of them back.<br><br>
All of navigate/help subcommands like "cd"/"help"/"ls"/"quit"/"up" will not list here.
## root level
<pre>
crm
    cluster		<b>Cluster setup and management</b>
    configure		<b>Cib configuration</b>
    node		<b>Node management</b>
    options		<b>Configure crmsh itself</b>
    resource		<b>Resource management</b>
    status		<b>CLuster status</b>
</pre>
Personally, I think the sublevels in root level are probably too many.<br>
It looks clear and simple that just leave the most frequently used.
#### Diff table(1) in root level
|name|description|original| plan| explain|
|--|--|--|--|--|
|<del>cib</del>|CIB shadow management|crm>cib|crm>configure>shadow|<ul><li>seems less used, not suitable in root level</li><li>"shadow" is more specific</li><li>"cibshadow" is also fine</li></ul>|
|<del>cibstatus</del>|CIB status management and editing|crm>cibstatus|crm>configure>cibstatus|<ul><li>seems less used, not suitable in root level</li></ul>|
|<del>corosync</del>|Corosync management|crm>corosync|crm>configure>corosync|<ul><li>seems less used, not suitable in root level</li></ul>|
|<del>history</del>|Cluster history|crm>history|crm>cluster>tools>history|<ul><li>should better put into "tools" level</li></ul>|
|<del>maintenance</del>|Maintenance mode commands|crm>maintenance|crm>configure>property|<ul><li>maintenance here is actually a cluster property "maintenance-mode"</li></ul>|
||<pre>                  </pre>|||<pre>                                          </pre>|
#### Diff table(2) in root level
|name|description|original| plan| explain|
|--|--|--|--|--|
|<del>ra</del>|Resource Agents lists and doc|crm>ra|crm>configure>resource>agent|<ul><li>should better put it into specific level, not root</li></ul>|
|<del>report</del>|Create cluster status report|crm>report|crm>cluster>tools>report|<ul><li>should better put into "tools" level</li></ul>|
|<del>script</del>|Cluster script management|crm>script|None|<ul><li>have no idea yet; maybe crm>configure>wizard ?</li><li>just feel that it's not suitable in root level</li><li>maybe rewrite it using Salt, to make a better "wizard"</li></ul>|
|<del>site</del>|GEO clustering site support|crm>site|crm>cluster>geo>ticket|<ul><li>should better put into "cluster>geo" level</li></ul>|
|<del>verify</del>|Verify cluster status|crm>verify|crm>cluster>tools>verify|<ul><li>should better put into "tools" level</li></ul>|
||<pre>              </pre>|||<pre>                                                </pre>|
---
## cluster level
<pre>
cluster
    add			<b>Add a new node to the cluster</b>
    geo			<b>Geo cluster setup and management</b>
        init		<b>Configure cluster as geo cluster</b>
        init_arbitrator <b>Initialize node as geo arbitrator</b>
        join		<b>Join cluster to existing geo cluster</b>
        ticket		<b>GEO cluster tickets management</b>
    init		<b>Initialize a new HA cluster</b>
    join		<b>Join existing cluster</b>
    remove		<b>Remove node(s) from the cluster</b>
    rename		<b>Rename the cluster</b>
    service		<b>Cluster service management(start;stop;enable;disable;status;wait_for_startup)</b>
    tools
        copy		<b>Copy file to other cluster nodes</b>
        diff		<b>Diff file across cluster</b>
        health          <b>Cluster health check</b>
        history         <b>Cluster history</b>
        report          <b>Create cluster status report</b>
        run             <b>Execute an arbitrary command on all nodes/specific node</b>
        test            <b>cluster test tool set</b>
        verify          <b>Verify cluster status</b>
</pre>
#### Diff table(1) in cluster level
|name|description|original|plan|explain |
|--|--|--|--|--|
|<del>copy</del>|Copy file to other cluster nodes|cluster>copy|cluster>tools>copy|<ul><li>seems less used</li></ul>|
|<del>diff</del>|Diff file across cluster|cluster>diff|cluster>tools>diff|<ul><li>seems less used</li></ul>|
|**service**|Cluster service management|None|cluster>service|<ul><li>This is a sub-command, with options "start"/"stop"/"disable"/"enable"/"status"/"wait_for_startup"</li><li>It's more specific for service operations</li></ul>|
|<del>disable</del>|Disasble cluster services|cluster>disable|cluster>service|<ul><li>"disable" is an option for cluster service command</li></ul>|
|<del>enable</del>|Enable cluster services|cluster>enable|cluster>service|<ul><li>"enable" is an option for cluster service command</li></ul>|
|<del>start</del>|Start cluster services|cluster>start|cluster>service|<ul><li>"start" is an option for cluster service command</li></ul>|
|<del>stop</del>|Stop cluster services|cluster>stop|cluster>service|<ul><li>"stop" is an option for cluster service command</li></ul>|
|<del>status</del>|Cluster status check|cluster>status|cluster>service|<ul><li>"status" is an option for cluster service command</li></ul>|
|<del>wait_for_startup</del>|Wait for cluster to start|cluster>wait_for_startup|cluster>service|<ul><li>"wait_for_startup" is an option for cluster service command</li></ul>|
||<pre>               </pre>|||<pre>                                     </pre>|
#### Diff table(2) in cluster level
|name|description|original|plan|explain |
|--|--|--|--|--|
||||||
|**geo**|Geo cluster setup and management|None|cluster>geo|<ul><li>Create a specific sublevel for GEO cluster</li></ul>|
|<del>geo_init</del>|Configure cluster as geo cluster|cluster>geo_init|cluster>geo>init|<ul><li>move to the new geo sublevel</li></ul>|
|<del>geo_init_arbitrator</del>|Initialize node as geo cluster arbitrator|cluster>geo_init_arbitrator|cluster>geo>init_arbitrator|<ul><li>move to the new geo sublevel</li></ul>|
|<del>geo_join</del>|Join cluster to existing geo cluster|cluster>geo_join|cluster>geo>join|<ul><li>move to the new geo sublevel</li></ul>|
|**geo>ticket**|GEO cluster tickets management|crm>site|cluster>geo>ticket|<ul><li>move to the new geo sublevel</li></ul>|
|<del>health</del>|Cluster health check|cluster>health|cluster>tools>health|<ul><li>move to the cluster tools sublevel</li></ul>|
|<del>run</del>|Execute a command on node/nodes|cluster>run|cluster>tools>run|<ul><li>move to the cluster tools sublevel</li></ul>|
|**tools**|Cluster tools|None|cluster>tools|<ul><li>include useful but less used tools, like "report","history","verify","health" and "test"</li><li>while "test" is related to the FATE#321073</li></ul>|
||<pre>               </pre>|||<pre>                                        </pre>|
---
## configure level
<pre>
configure
    acl		   	<b>Define target access rights</b>
        role            <b>Define role access rights</b>
        user            <b>Define user access rights</b>
    alert		<b>Event-driven alerts</b>
    cibstatus		<b>Cib status management and editing</b>
    commit              <b>Commit the changes to the CIB</b>
    constraints		<b>Define resource constraints</b>
        colocation      <b>Colocate resources</b>
        location        <b>A location preference</b>
        order           <b>Order resources</b>
        ticket          <b>Resources ticket dependency</b>
    corosync		<b>Corosync management</b>
        diff            <b>Diffs the corosync configuration</b>
	edit            <b>Edit the corosync configuration</b>
	get             <b>Get a corosync configuration value</b>
	log             <b>Show the corosync log file</b>
	pull            <b>Pulls the corosync configuration</b>
	push            <b>Push the corosync configuration</b>
	reload          <b>Reload the corosync configuration</b>
	set             <b>Set a corosync configuration value</b>
	show            <b>Display the corosync configuration</b>
	status          <b>Display the corosync status</b>
    delete              <b>Delete CIB objects</b>
    edit                <b>Edit CIB objects</b>
    fencing_topology    <b>Node fencing order</b>
    load                <b>Import the CIB from a file</b>
    node		<b>Define a cluster node</b>
    property            <b>Set/Show a cluster property</b>
    refresh             <b>Refresh from CIB</b>
    resource		<b>Define a cluster resource</b>
        agent		<b>Resource Agents lists and documentation</b>
        bundle		<b>bundle resource</b>
        clone		<b>clone resource</b>
        group		<b>group resource</b>
        master		<b>master resource</b>
        op_defaults	<b>Set resource operations defaults</b>
        primitive	<b>primitive resource</b>
        rsc_defaults	<b>Set resource meta defaults</b>
        rsc_template	<b>resource template</b>
    save                <b>Save the CIB to a file</b>
    schema              <b>Set or display current CIB RNG schema</b>
    shadow		<b>Cib shadow management</b>
    show                <b>Display CIB objects</b>
    tag                 <b>Define resource tags</b>
</pre>
#### Diff table(1) in configure level
|name|description|original|plan|explain|
|--|--|--|--|--|
|**acl**|Define target access rights|configure>acl_target|configure>acl|<ul><li>make it more simple</li></ul>|
|<del>assist</del>|Configuration assistant|configure>assist|None|<ul><li>no need a specific command for such rarely used configuration</li></ul>|
|<del>bundle</del>|bundle resource|configure>bundle|configure>resource>bundle|<ul><li>should better put into 'resource' sublevel</li></ul>|
|<del>cib</del>|CIB shadow management|configure>cib|configure>shadow|<ul><li>"shadow" is more specific</li><li>"cibshadow" is also fine</li></ul>|
|<del>clone</del>|clone resource|configure>clone|configure>resource>clone|<ul><li>should better put into 'resource' sublevel</li></ul>|
|<del>colocation</del>|Colocate resources|configure>colocation|configure>constraints>colocation|<ul><li>should better put into 'colocation' sublevel</li></ul>|
|**constraints**|Define resource constraints|None|configure>constraints|<ul><li>Create a specific sublevel for constraints</li></ul>|
||<pre>           </pre>|||<pre>                             </pre>|
#### Diff table(2) in configure level
|name|description|original|plan|explain|
|--|--|--|--|--|
|<del>default-timeouts</del>|Set timeouts for operations to minimums from the meta-data|configure>default-timeouts|None|<ul><li>The use of this command is discouraged</li></ul>|
|<del>erase</del>|Erase the CIB|configure>erase|None|<ul><li>Personally I think this is dangerous</li><li>have cmd delete is enough</li></ul>|
|<del>filter</del>|Filter CIB objects|configure>filter|None|<ul><li>require user known some shell/sed? It's hard</li><li>have cmd edit is enough</li></ul>|
|<del>get_property</del>|Get property value|configure>get_property|None|<ul><li>should be an option of configure>property</li></ul>|
|<del>graph</del>|Generate a directed graph|configure>graph|None|<ul><li>I don't think the graph is useful for user</li></ul>|
|<del>group</del>|Define a group|configure>group|configure>resource>group|<ul><li>should better put into 'resource' sublevel</li></ul>|
|<del>history</del>|Cluster history|configure>history|crm>cluster>tools>history|<ul><li>should better put into "tools" level</li></ul>|
|<pre>           </pre>|<pre>                  </pre>|<pre>                    </pre>||<pre>                               </pre>|
#### Diff table(3) in configure level
|name|description|original|plan|explain|
|--|--|--|--|--|
|<del>location</del>|A location preference|configure>location|configure>constraints>location|<ul><li>should better put into 'colocation' sublevel</li></ul>|
|<del>modgroup</del>|Modify group|configure>modgroup|None|<ul><li>no need a specific command</li><li>use edit if needed</li></ul>|
|<del>monitor</del>|Add monitor operation to a primitive|configure>monitor|None|<ul><li>no need a specific command</li><li>use edit is needed</li></ul>|
|<del>ms</del>|master resource|configure>ms|configure>resource>master|<ul><li>should better put into 'resource' sublevel</li></ul>|
|<del>op_defaults</del>|Set resource operations defaults|configure>op_defaults|configure>resource>op_defaults|<ul><li>should better put into 'resource' sublevel</li></ul>|
|<del>order</del>|Order resources|configure>order|configure>constraints>order|<ul><li>should better put into 'colocation' sublevel</li></ul>|
|<del>primitive</del>|primitive resource|configure>primitive|configure>resource>primitive|<ul><li>should better put into 'resource' sublevel</li></ul>|
||<pre>                </pre>|<pre>                </pre>||<pre>                           </pre>|
#### Diff table(4) in configure level
|name|description|original|plan|explain|
|--|--|--|--|--|
|<del>ptest</del>|Show cluster actions if changes were committed|configure>ptest|None|<ul><li>rarely used?</li></ul>|
|<del>ra</del>|Resource Agents lists and doc|configure>ra|configure>resource>agent|<ul><li>should better put it into specific level</li><li>rename as 'agent', more specific</li></ul>|
|<del>rename</del>|Rename a CIB object|configure>rename|None|<ul><li>no need a specific command</li><li>use edit is needed</li></ul>|
|**resource**|Define a cluster resource|None|configure>resource|<ul><li>lots of resource types, should be classified</li></ul>|
|<del>role</del>|Define role access rights|configure>role|configure>acl>role|<ul><li>move to the configure acl sublevel</li></ul>|
|<del>rsc_defaults</del>|Set resource meta defaults|configure>rsc_defaults|configure>resource>rsc_defaults|<ul><li>move to the configure resource sublevel</li></ul>|
|<del>rsc_template</del>|Define a resource template|configure>rsc_template|configure>resource>rsc_template|<ul><li>move to the configure resource sublevel</li></ul>|
||<pre>                   </pre>|<pre>                </pre>||<pre>                    </pre>|
#### Diff table(5) in configure level
|name|description|original|plan|explain|
|--|--|--|--|--|
|<del>rsc_ticket</del>|Resources ticket dependency|configure>rsc_ticket|configure>constraints>ticket|<ul><li>move to the configure constraints sublevel</li><li>rename as 'ticket'</li></ul>|
|<del>rsctest</del>|Test resources as currently configured|configure>rsctest|None|<ul><li>rarely used?</li></ul>|
|<del>set</del>|Set an attribute value|configure>set|None|<ul><li>rarely used?</li><li>have cmd edit is enough</li></ul>|
|<del>template</del>|Edit and import a configuration from a template|configure>template|None|<ul><li>rarely used?</li></ul>|
|<del>upgrade</del>|Upgrade the CIB|configure>upgrade|None|<ul><li>rarely used?</li></ul>|
|<del>user</del>|Define user access rights|configure>user|configure>acl>user|<ul><li>move to the configure acl sublevel</li></ul>|
|<del>validate-all</del>||configure>validate-all|None|<ul><li>rarely used?</li></ul>|
|<del>verify</del>|Verify the CIB with crm_verify|configure>verify|None|<ul><li>rarely used?</li><li>we will verify when run 'commit'?</li></ul>|
|<del>xml</del>|Raw xml|configure>xml|None|<ul><li>rarely used?</li></ul>|
|<pre>      </pre>|<pre>                       </pre>|<pre>               </pre>||<pre>                              </pre>|
#### Diff table(6) in configure level(corosync sublevel)
|name|description|original|plan|explain|
|--|--|--|--|--|
||||||
|**corosync**|corosync management|crm>corosync|configure>corosync|<ul><li>seems less used, not suitable in root level</li></ul>|
|<del>add-node</del>|Add a corosync node|crm>corosync>add-node|None|<ul><li>Confuse with cluster add? Keep one?</li></ul>|
|<del>del-node</del>|Remove a corosync node|crm>corosync>del-node|None|<ul><li>Confuse with cluster remove? Keep one?</li></ul>|
|**diff**|Diffs the corosync configuration|crm>corosync>diff|configure>corosync>diff|<ul><li>move to new corosync sublevel</li></ul>|
|**edit**|Edit the corosync configuration|crm>corosync>edit|configure>corosync>edit|<ul><li>move to new corosync sublevel</li></ul>|
|**get**|Get a corosync configuration value|crm>corosync>get|configure>corosync>get|<ul><li>move to new corosync sublevel</li></ul>|
||<pre>               </pre>|<pre>                  </pre>||<pre>                            </pre>|
#### Diff table(7) in configure level(corosync sublevel)
|name|description|original|plan|explain|
|--|--|--|--|--|
|**log**|Show the corosync log file|crm>corosync>log|configure>corosync>log|<ul><li>move to new corosync sublevel</li></ul>|
|**pull**|Pulls the corosync configuration|crm>corosync>pull|configure>corosync>pull|<ul><li>move to new corosync sublevel</li></ul>|
|**push**|Push the corosync configuration|crm>corosync>push|configure>corosync>push|<ul><li>move to new corosync sublevel</li></ul>|
|**reload**|Reload the corosync configuration|crm>corosync>reload|configure>corosync>reload|<ul><li>move to new corosync sublevel</li></ul>|
|**set**|Set a corosync configuration value|crm>corosync>set|configure>corosync>set|<ul><li>move to new corosync sublevel</li></ul>|
|**show**|Display the corosync configuration|crm>corosync>diff|configure>corosync>show|<ul><li>move to new corosync sublevel</li></ul>|
|**status**|Display the corosync status|crm>corosync>diff|configure>corosync>status|<ul><li>move to new corosync sublevel</li></ul>|
||<pre>               </pre>|<pre>                  </pre>||<pre>                            </pre>|
---
## node level
<pre>
node
    attribute		<b>Manage attributes</b>
    clearstate		<b>Clear node state</b>
    delete		<b>Delete node</b>
    fence		<b>Fence node</b>
    maintenance		<b>Put node into maintenance mode</b>
        on
        off 
    show		<b>Show node</b>
    standby             <b>Put node into standby</b>
        on
        off 
    status-attr         <b>Manage status attributes</b>
    utilization         <b>Manage utilization attributes</b>
</pre>
#### Diff table in node level
|name|description|original|plan|explain|
|--|--|--|--|--|
|<del>online</del>|Set node online|node>online|node>standby|<ul><li>as an option of node>standby: "standby off"</li></ul>|
|<del>ready</del>|Put node into ready mode|node>ready|node>maintenance|<ul><li>as an option of maintenance: "maintenance off"</li></ul>|
|<del>server</del>|Show node hostname or server address|node>server|None|<ul><li>rarely used?</li></ul>|
|<del>status</del>|Show nodes' status as XML|node>status|None|<ul><li>overlaped with node>show command</li></ul>|
---
## resource level
<pre>
resource
    ban			<b>Ban a resource from a node</b>
        on
        off
    cleanup		<b>Cleanup resource status</b>
    constraints		<b>Show constraints affecting a resource</b>
    demote		<b>Demote a master resource</b>
    failcount		<b>Manage failcounts</b>
    locate		<b>Show the location of resource</b>
    maintenance		<b>Enable/disable pre-resource maintenance mode</b>
    manage		<b>Put a resource into managed mode</b>
        on
        off
    meta		<b>Manage a meta attribute</b>
    move		<b>Move a resource to another node</b>
        on
        off
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
        on
        off
    utilization		<b>Manage a utilization attribute</b>
</pre>
#### Diff table in resource level
|name|description|original|plan|explain|
|--|--|--|--|--|
|<del>clear</del>|Clear any relocation constraint|resource>clear|None|<ul><li>integrated in move and ban as "off" option</li></ul>|
|<del>unmanage</del>|Put a resource into unmanaged mode|resource>unmanage|None|<ul><li>integrated in manage as "off" option</li></ul>|
|<del>untrace</del>|Stop RA tracing|resource>untrace|None|<ul><li>integrated in trace as "off" option</li></ul>|
