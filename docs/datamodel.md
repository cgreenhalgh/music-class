# music class data model

A single server may server multiple `Site`s.
Each site is independent (give or take overlapping admin
accounts).
Each site may have its own branding, and be linked to a 
different organisation and/or project.
We'll assume for now that most tables are per-site (except Admin).

`Site` properties:
- `id` (string) - site identifier, hopefully URL-able (shortname)
- `name` (string) - site title
- `description` (string) - short description of site
- `terms` - map (key: value (string)) of site-specific terms. 
- `access` - map (key=account(email), value=array of capabilities) of admin/teacher role/capability assignments.
- ?`favicon` - site favicon (encoded? url?)
- ?? site-specific template and CSS stuff (colours, images, etc.)

`term` keys tentatively include:
- `admin`: admin role name, e.g. "teacher"
- `group`: top-level group name, e.g. "class" 
- `user`: participant/user name, e.g. "student"
- `display`: shared display, e.g. "classroom display"
- `assignment`: unit of work-setting

`capabilities` tentatively include:
- `view` - (site or class) view anything
- `view:shared` - (site or class) view shared material
- `edit` - (site or class) change and respond to anything (except create classes)
- `edit:respond` - (site or class) respond to any work
- `edit:moderate` - (site or class) moderate any work
- `admin` - (site) create or (site or class) change classes; change (site or class) admin rights; (site or class) add/update users
- `admin:users` - (site or class) add/update users (ony)

Within a single site there may be one or more `Group`s.
Each group has its own independent set of users, activities and content.
A group represens a single bounded set of related activities with the same set of users/students and admins/teachers. It could involve one school or many.
It is comparable to a Google "Class" or Moodle "Course".

`Group` properties:
- `id` (string) - group/class identifier (shortname), unique within site, URLable
- `name` (string) - display name/title
- `description` (string) - short description
- `access` - map (key=account(email), value=array of capabilities) of admin/teacher role/capability assignments.
- `anon_guest` (boolean) - allow anonymous guest access
- ?? other access options, e.g. allow anon access URLs
- ?? `siteid` (string) - ID of containing site (FK) - not required if tables per-site

An `Admin` is a potentially privileged user, not a student/class-member.
Authentication may be handled by an external authentication services.
It is assumed that every admin has a unique email (which also acts as primary key) and is trusted, at least as far as their access grants allow.

`Admin` properties:
- `id` (string) - assumed PK (server-wide), email by default
- `name` (string) - name to display
- `description` (string) - role to display (for info only), e.g. "Teacher at XXX School"
- ? `password` (string) - suitably hashed password if internal authentication in use.
- `enabled` (boolean) - is access allowed
- `comments` (string) - for admin use
- ?? other security-related attributes, e.g. last successful login, ip whitelist, failed attempts

A `User` is a student/class-member.
Each user belongs to a specific `Group`.
Users may not have an email (e.g. they may be a primary-age child).
User accounts are identified only by a (short) screen name, which would normally just be a first name or initials (for safeguarding, to avoid identification outside the class/group).
For now, authentication is assumed be by password/similar, managed by Admin(s).

`User` properties:
- `id` (number?) - PK, arbitrary, site-wide unique
- `name` (string) - display name
- `description` (string) - optional additional display info, e.g. school
- `groupid` (string) - user's group (FK)
- ?? `siteid` (string) - ID of containing site (FK) - not required if tables per-site
- `password` (string) - suitably hashed password
- `enabled` (boolean) - is allowed access
- `comments` (string) - for admin use
- ?? other access information, e.g. last login
- ?? user tags

An `Assignment` is a piece of work to be done by one or more Users.
Assignments are created by admin/teachers and assigned to users.
They are typically ordered (by time).
A single class is essentially a sequence of Assignments.
The purpose of an assignment is to create a piece of work, perhaps including getting feedback on it and refining it.

`Assignment` properties:
- `id` (number?) - PK, site-wide
- `name` (string) - title of assignment
- `description` (string) - short display description
- `groupid` (string) - assignment's group (FK)
- ?? `siteid` (string) - ID of containing site (FK) - not required if tables per-site
- `published` (boolean) - has it been published/"set" (or is it still draft)
- `shared` (boolean) - visible on shared?
- `assign_to_all` (boolean) - assigned to everyone
- ? `assign_to_userids` (array of string) - if not assigned to all, specific users it is assigned to
- `publisheddate` (string/date) - required, used for sorting
- ? `duedate` (string/date) - optional, for info
- `closed` (boolean) - (manually) closed, i.e. no more work/versions
- `content` (JSON?) - assignment document or equivalent
- ?? TODO: response template info

document type needs more definition.
Probably a JSON array of parts, each with a type and content.
Parts may include text/html and files (attachments) - see `File`.
May also include some structured types/fields, e.g. ratings, badges.

A user's response to an `Assignment` is a `Work`, which in turn may be a sequent of `Version`s.
We'll leave group work for later.
We don't really need `Work` at the moment, just `Version` ?!

`Work` properties:
- `id` - PK
- `assignmentid` - FK to Assignment
- `userid` - FK to User
- ?? `siteid` (string) - ID of containing site (FK) - not required if tables per-site
- ?? more summary info

A `Version` is a single attempt at (iteration of) an `Assignment`.

`Version` properties:
- `id` - PK
- `workid` - FK
- `assignmentid` - FK to Assignment (de-norm from Work)
- `assignmentpublisheddate` (string) for sorting, de-norm from Assignment
- ? `ugc` (boolean) - from User (not admin)
- `userid` - FK to User (do-norm from Work)
- ? `adminid` - FK to Admin
- ?? `siteid` (string) - ID of containing site (FK) - not required if tables per-site
- `submitted` (boolean) - as opposed to draft
- `createddate` (string/date) - started
- `submitteddate` (string/date)
- `current` (boolean) - latest
- `content` - document
- ?? template info
- ?? TODO: moderation info

A `Response` is a user or admin response to a student `Version`, i.e. attempt at an `Assignment`.

`Response` properties:
- `id` - PK
- `versionid` - FK
- `ugc` (boolean) from User (not admin)
- `userid` - FK to User for UGC
- `adminid` - FK to Admin for admin responses
- `workid` - FK to Work, de-norm from Version
- `assignmentid` - FK to Assignment (de-norm from Work)
- `workuserid` - FK to User of Work (de-norm from Work)
- `content` - document ?!
- ?? template info
- ?? TOOD: moderation info

A `File` is an uploaded file. 
Should be part of a document, e.g. content of an Assignment, Version
or Response.

`File` properties:
- `id` - PK?
- `contenttype` (string)
- `length` (number) - bytes
- `fileextension` (string) - as uploaded?
- `filepath` (string) - on filesystem, relative to upload dir?
- `uploadfilename` (string) - as uploaded, if provided
- ?? access control info
- ?? TODO: moderation info
- `ugc` (boolean) if User
- `userid` - FK if User
- `adminid` - FK if Admin
- `groupid` - FK
- ?? video transcoding stuff


TODO: Shared ?

