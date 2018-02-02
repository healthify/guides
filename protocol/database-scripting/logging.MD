# Logging


It is important that virtually every significant change to our database is tracked and auditable. 

## PaperTrail
In service of this, we use the [PaperTrail gem](https://github.com/airblade/paper_trail) to record the past versions/states of the records contained in most of our important database tables.

In every rails console action, rake task, or other script, be sure to set a proper PaperTrail whodunnit field.

To do this, execute the following command before any database transactions:
```
PaperTrail.whodunnit = "<a_well_formatted_whodunnit>"
```

The whodunnit should be formatted in the following manner:

```
"<a_colon_and:underscore_separated_descriptive_name> email:<your_email@healthify.us>"
```

If you are running a rake task, the descriptive name may be the name of the rake task:

```
geolocation:fix_outliers email:krishna@healthify.us
```

If you performing another console action or script, you may choose your own appropriate name
or a relevant Pivotal Tracker or Trello story id.
