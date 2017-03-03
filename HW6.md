#Find all time entries.
Select time_entries.created_at, time_entries.worked_on, time_entries.duration, projects.name
From time_entries, projects
where time_entries.project_id = projects.id
Order by projects.name desc

Results: 500

#Find the developer who joined most recently.
Select developers.name, developers.joined_on
from developers
Order by developers.joined_on DESC
LIMIT 3

Results: "Dr. Danielle McLaughlin"	"2015-07-10"

#Find the number of projects for each client.
Select clients.name, count(projects.id)
from clients, projects
Where clients.id = projects.client_id
Group by clients.name

Results: 9

#Find all time entries, and show each one's client name next to it.
select time_entries.worked_on, developers.name, time_entries.project_id, projects.name, clients.name
from time_entries
left Join developers, projects, clients
on time_entries.developer_id = developers.id and time_entries.project_id = projects.id and projects.client_id = clients.id
order by time_entries.worked_on

Results: 500

#Find all developers in the "Ohio sheep" group.
select developers.name, group_assignments.group_id, groups.name
from developers, group_assignments, groups
where developers.id = group_assignments.developer_id and groups.id = group_assignments.group_id
and groups.name like 'Ohio sheep'

Results: 3

#Find the total number of hours worked for each client.
select clients.name, sum(time_entries.duration)
from clients
left join time_entries, projects
on time_entries.project_id = projects.id
and projects.client_id = clients.id
group by clients.name

Results: 9

#Find the client for whom Mrs. Lupe Schowalter (the developer) has worked the greatest number of hours.
select clients.name, sum(time_entries.duration)
from developers, time_entries, clients, projects
where developers.name like 'Mrs. Lupe Schowalter'
	and time_entries.developer_id = developers.id
	and time_entries.project_id = projects.id
	and projects.client_id = clients.id
group by clients.name
order by sum(time_entries.duration) DESC

Results: "Kuhic-Bartoletti"	"11"

#List all client names with their project names (multiple rows for one client is fine). Make sure that clients still show up even if they have no projects.
select clients.name, projects.name
from clients
left join projects
on projects.client_id = clients.id
order by clients.name

Results: 33

#Find all developers who have written no comments.
select developers.name, comments.id
from developers
left join comments
on comments.developer_id = developers.id
and developers.id is null
order by developers.name

Results: 50
