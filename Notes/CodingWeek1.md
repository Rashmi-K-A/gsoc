
Organizational information to store:
1. Hierarchy of groups/departments/sections/etc within an organization. A group can be composed by 0 to N subgroups, and there's no depth limit.
2. Aliases of organization


Discussion:
An alternate approach to manage organizational information

The proposal [here](https://docs.google.com/document/d/1oUd-G-N4VXh77FRI4PTSWJHoVxGt2WZDNu7JwuFMbW4/edit) modifies the existing Organization model to handle hierarchical data.

An alternate approach is to keep the Organization model as it is and create a new model for groups/divisions/departments. We need to evaluate how to proceed with this alternate approach and leave the existing model untouched. 

Trade-off:
There might be some code duplication between organizations and groups (we might have to write similar methods) but having two models would 
    a) reduce possibility of existing code breaking
    b) draw a clean line of segregation between fields like aliases that might only be present at the top level org and not in groups.