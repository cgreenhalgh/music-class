# music class views

Components:
- document (content) view
- various part views, e.g. badge, scale, video/image/file
- document (content) editor (constrained and not)

Views/routes, User:
- `/user/:groupid/login` - log in
- `/user/:groupid/assignments/:assignmentid` - single assignment (for user)
- `/user/:groupid/assignments/` - all assignments (for user)
- `/user/:groupid/shared/` - all shared assignments (for group)
- `/user/:groupid/shared/:assignmentid` - single shared assignment
and maybe
- `/user/:groupid/shared/version/:versionid` - single shared version
- `/user/:groupid/shared/response/:responseid` - single shared response

Views/routes, Admin:
- `/admin/login` - site log in
- `/admin/groups/:groupid/login` - group login
- `/admin/groups/:groupid/assignments/:assignmentid/users/` - versions for single assignment
- `/admin/groups/:groupid/assignments/:assignmentid/users/:userid` - one user's response to an assignment
- `/admin/groups/:groupid/shared/` - all shared assignments (for group)
- `/admin/groups/:groupid/shared/:assignmentid` - single shared assignment
and maybe
- `/admin/groups/:groupid/shared/version/:versionid` - single shared version
- `/admin/groups/:groupid/shared/response/:responseid` - single shared response
plus
- `/admin/groups/:groupid/assignments/` - all assignments
- `/admin/groups/:groupid/assignments/:assignmentid` - single assignment (add/edit)
- `/admin/groups/:groupid/users/` - all users
- ? `/admin/group/:groupid/users/:userid` - user details (if not in place)
- `/admin/groups/:groupid/users/:userid/assignments/` - all of one users assignments
- `/admin/groups/:groupid/users/:userid/shared/` - one user's shared assignments
- `/admin/groups/` - all groups
- `/admin/groups/:groupid` - single group add/edit
- `/admin/admins/` - add admins for site (and roles)
- `/admin/admins/:adminid` - admin details for site (if needed)
- `/admin/groups/:groupid/admins/` - all admins for group
- `/admin/groups/:groupid/admins/:adminid` - single  admin details/roles for group
- `/admin/groups/:groupid/moderation/` - all ugc/moderation
- `/admin/groups/:groupid/activity/` - all recent activity


