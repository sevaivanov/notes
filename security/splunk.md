# splunk

## searches

    # find who searched what
    index=_audit action=search search=* NOT "typeahead" NOT metadata NOT " | history" NOT "AUTOSUMMARY" | table _time, user, search

    # find the user that searched something
    index=_audit action=search search=* user=${user_of_interest} NOT "typeahead" NOT metadata NOT " | history" NOT "AUTOSUMMARY" | table _time, search

    # find who created a user
    index=_audit action=edit_user operation=create
        | rename object as user
        | eval timestamp=strptime(timestamp, "%m-%d-%Y %H:%M:%S.%3N")
        | convert timeformat="%d/%b/%Y" ctime(timestamp)
        | table user timestamp

## ui features

    # there is a build-in view for these stats but less friendly for a report
    /app/splunk_instance_monitoring/search_usage_statistics

    # find users and permissions
    /manager/launcher/authentication/users
