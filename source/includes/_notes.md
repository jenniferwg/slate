# Notes

> Example Response

```json
{
  "id":22984,
  "creator_id":860197,
  "person_ids":[38706,89734],
  "organization_ids":[64779194],
  "opportunity_ids":[117],
  "content":"Had a lunch meeting with Jane and John today. They are looking to invest.",
  "created_at":"2017-03-28 00:38:41 -0700"
}
```

Just like field values, notes are used to keep track of state on an entity. They could be
notes manually taken from due diligence, a meeting, or a call. Or, notes could be used to
store logged activity from a prospect's visit to your website.

## The note resource
A note object contains `content`, which is a string containing the note body. In
addition, a note can be associated with multiple people, organizations, or opportunities.
Each person, organization, or opportunity will display linked notes on their profiles.

Attribute | Type | Description
--------- | ------- | -----------
id | integer | The unique identifier of the note object.
creator_id | integer | The unique identifier of the person object who created the note.
person_ids | integer[] | An array of unique identifiers of person objects that are associated with the note.
organization_ids | integer[] | An array of unique identifiers of organization objects that are associated with the note.
opportunity_ids | integer[] | An array of unique identifiers of opportunity objects that are associated with the note.
content | string | The string containing the content of the note.
created_at | datetime | The string representing the time when the note was created.

## Create a new note

> Example Request

```shell
curl "https://api.affinity.co/notes" \
  -u :<API-KEY> \
  -d person_ids[]=38706 \
  -d person_ids[]=624289 \
  -d organization_ids[]=120611418 \
  -d opportunity_ids[]=167 \
  -d content="Had a lunch meeting with Jane and \
              John today. They want to invest in \
              Acme Corp."
```
> Example Response

```json
{
  "id":22984,
  "creator_id":860197,
  "person_ids":[38706,89734],
  "organization_ids":[64779194],
  "opportunity_ids":[117],
  "content":"Had a lunch meeting with Jane ... ",
  "created_at":"2017-03-28 00:38:41 -0700"
}
```
`POST /notes`

Creates a new note with the supplied parameters.

Affinity's Gmail Chrome Extension allows users to save an email as a note in Affinity.
This same functionality is exposed in our API via the `gmail_id` parameter. Passing in the
`id` of a Gmail
[message](https://developers.google.com/gmail/api/v1/reference/users/messages) will
automatically sync the email to Affinity (if it hasn't synced with Affinity already)
and associate it with the newly created note. (Make sure that Affinity has access to the
account that owns the Gmail mesage.) A note associated with an email cannot have any
`content`.

### Path Parameters

Parameter | Type | Required | Description
--------- | ------- | ---------- | -----------
person_ids | integer[] | false | An array of unique identifiers of person objects that are associated with the new note.
organization_ids | integer[] | false | An array of unique identifiers of organization objects that are associated with the new note.
opportunity_ids | integer[] | false | An array of unique identifiers of opportunity objects that are associated with the new note.
content | string | false | The string containing the content of the new note.
gmail_id | string | false | The id of a Gmail message to save as the content of the note.
creator_id | integer | false | The id of a Person resource who should be recorded as the author of the note. Must be a person who can access Affinity. If not provided the creator defaults to the owner of the API key.

Note that either `content` or `gmail_id` must be specified.

### Returns
The note resource created through this request.
