
A change management process ensures that *personnel can perform a security impact analysis*. Experts evaluate changes to identify any security impacts before personnel deploy the changes in a production environment.

Change management controls provide a process to **control, document, track, and audit all system changes**. This includes changes to any aspect of a system, including hardware and software configuration. Organisations implement change management processes through the life cycle of any system.

### Standard Operating Procedures for Changes

Common tasks within a change management process are as follows:

1. **Request the change**. Once personnel identify desired changes, they request the change. Some organisations use internal websites, allowing personnel to submit change requests via a web page. The website *automatically logs the request in a database, which allows personnel to track the changes*. It also allows anyone to see the status of a change request.
   
2. **Review the change**. Experts within the organization review the change. Personnel reviewing a change are typically from several different areas within the organization. These should be *identified through a complete impact analysis performed* in consultation with the owners of the change and the various stakeholders in the change. In some cases, stakeholders may quickly complete the review and approve or reject the change. In other cases, the change *may require approval at a formal change review board or change advisory board* (CAB) after extensive testing. Board members are the personnel that review the change request.
   
3. **Approve/reject the change**. Based on the review, these experts then approve or reject the change. They also *record the response in the change management documentation*. For example, if the organization uses an internal website, someone will document the results in the website's database. In some cases, the change review board *might require the creation of a rollback or backout plan*. This ensures that personnel can return the system to its original condition if the change results in a failure.
   
4. **Test the change**. Once the change is approved, *it should be tested, preferably on a non-production server*. Testing helps verify that the change doesn't cause an unanticipated problem. Test results should be included in the change documentation.
   
5. **Schedule and implement the change**. The change is scheduled so that it can be implemented with the least impact on the system and the system's customers. This may require *scheduling the change during off-duty or non-peak hours*. Testing should discover any problems, but it's still possible that the change causes unforeseen problems. Because of this, it's *important to have a backout plan*. This allows personnel to undo the change and return the system to its previous state if necessary.
   
6. **Document the change**. The last step is the documentation of the change to ensure that all interested parties are aware of it. This step often requires a *change in the configuration management documentation*. If an unrelated disaster requires administrators to rebuild the system, the change management documentation provides them with information on the change. This ensures that they can return the system to the state it was in before the change.


>[!tip] Technical Impact of Changes
>The technical impacts of a change may be far-reaching. As organizations consider the potential for a change to disrupt other processes, they should consider all of those potential impacts. It's very important to involve a diverse set of technical stakeholders in this analysis because most organizations have a complex technical environment that no single person understands completely.
Some of the issues you should consider are: 
>
>- Whether the change will require any modifications to security controls, such as firewall rules, allow lists, or deny lists
>- Whether any other business or technical activities need to be restricted during or after the change
>- Whether the change will cause downtime for critical systems
>- Whether the change will require restarting any services or applications
>- Whether the change involves any legacy applications that lack vendor support
>- Whether all possible dependencies have been identified and documented

