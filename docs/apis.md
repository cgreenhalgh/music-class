# music class APIs

paths & ops
- `/api/userlogin` POST w. `userid`, `groupid`, `password`
- `/api/adminlogin` POST w. `email`, `password`
- `/api/guestlogin` POST w. `groupid`
- `/api/logout` POST

groups:
- `/api/groups/` GET (admin), POST (admin)
- `/api/groups/:groupid` GET (member/admin), PUT (admin) (DELETE?)

assignments:
- `/api/groups/:groupid/assignments/` ?shared GET (admin), POST (admin)
- `/api/groups/:groupid/assignments/:assignmentid` ?shared GET (user?/admin), PUT (admin), (DELETE?)

- `/api/groups/:groupid/users/:userid/assignments/` ?shared GET (user/admin)

versions:
- `/api/groups/:groupid/versions/` POST (admin/user*)
- `/api/groups/:groupid/versions/:versionid` ?shared GET (admin/owner/?), PUT (admin/owner)

- `/api/groups/:groupid/assignments/:assignmentid/versions/` ?shared GET (admin)
- `/api/groups/:groupid/users/:userid/assignments/:assignmentid/versions/` ?shared GET (user/admin?)
- ? current

users:
- `/api/groups/:groupid/users/` GET (admin), POST (admin)
- `/api/groups/:groupid/users/:userid` GET (admin/user?), PUT (admin)

responses:
- `/api/groups/:groupid/responses/` POST (admin/user*)
- `/api/groups/:groupid/responses/:responseid` ?shared GET (owner?/admin), PUT (owner/admin)

- `/api/groups/:groupid/versions/:versionid/responses/` ?shared GET (user/admin)
- `/api/groups/:groupid/assignments/:assignmentid/responses/` ?shared GET (user/admin)

admins:
- `/api/admins/` GET (admin), POST (admin)
- `/api/admins/:adminid` GET (admin), PUT (admin)

site roles:
- `/api/siteroles/:adminid` GET (admin), PUT (admin)
- ? `/api/siteroles/` GET (admin)

group roles:
- `/api/groups/:groupid/grouproles/:adminid` GET (admin), PUT (admin)
- ? `/api/groups/:groupid/grouproles/` GET (admin)

## security

use bearer token or session cookie to hold validated identity
(user:groupid:userid or admin:email or guest:groupid).

for user (and guest) requests 
- check group matches
- beware moderation.
- beware ownership/authorship (inc. version associated w response)

note shared vs specific. Any user or admin can make a 'shared' request but 
stuff will always be filtered out.

for admin check site and group roles on entry.

for collection gets, filter by visibility rather than fail.

