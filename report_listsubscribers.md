# Report: ListSubscribers

Description: TBD

## Setup

Data Extension Name:
view_listsubscribers_data_list

Columns:
ListID
ListName
ListType
Records
Subscribers
Active
Bounced
Held
Unsubscribed

Query 1.1: query_listsubscribers.listid
Type: Overwrite
```
SELECT l.ListID as ListID
, l.ListName as ListName
, l.ListType as ListType
FROM ent._ListSubscribers l with (nolock)
GROUP BY l.ListID, l.ListName, l.ListType
```

Query 2.1: query_lisubscribers.listid_count_subscribers
Type: Update/Add
```
select count(l.SubscriberID) as Records
, v.ListID as ListID
from ent._ListSubscribers l with (nolock)
left join view_listsubscribers_data_list v
on v.ListID = l.ListID
group by v.ListID
```

Query 3.1: query_lisubscribers.listid_count_subscribers__distinct
Type: Update/Add
```
select count(distinct l.EmailAddress) as Subscribers
, v.ListID as ListID
from ent._ListSubscribers l with (nolock)
left join view_listsubscribers_data_list v
on v.ListID = l.ListID
where l.Status in ('active', 'held', 'bouced', 'unsubscribed')
group by v.ListID
```

Query 4.1: query_lisubscribers.listid_count_subscribers_active__distinct
Type: Update/Add
```
select count(distinct l.EmailAddress) as Active
, v.ListID as ListID
from ent._ListSubscribers l with (nolock)
left join view_listsubscribers_data_list v
on v.ListID = l.ListID
where l.Status = 'active'
group by v.ListID
```

Query 5.1: query_lisubscribers.listid_count_subscribers_bounced__distinct
Type: Update/Add
```
select count(distinct l.EmailAddress) as Bounced
, v.ListID as ListID
from ent._ListSubscribers l with (nolock)
left join view_listsubscribers_data_list v
on v.ListID = l.ListID
where l.Status = 'bounced'
group by v.ListID
```

Query 6.1: query_lisubscribers.listid_count_subscribers_held__distinct
Type: Update/Add
```
select count(distinct l.EmailAddress) as Held
, v.ListID as ListID
from ent._ListSubscribers l with (nolock)
left join view_listsubscribers_data_list v
on v.ListID = l.ListID
where l.Status = 'held'
group by v.ListID
```

Query 7.1: query_lisubscribers.listid_count_subscribers_unsubs__distinct
Type: Update/Add
```
select count(distinct l.EmailAddress) as Unsubscribed
, v.ListID as ListID
from ent._ListSubscribers l with (nolock)
left join view_listsubscribers_data_list v
on v.ListID = l.ListID
where l.Status = 'unsubscribed'
group by v.ListID
```

Data Extension Name:
view_listsubscribers_data_report

Columns:
Date (Primary Key - default date)
ListID (Primary Key)
ListName
ListType
Records
Subscribers
Active
Bounced
Held
Unsubscribed

Query 8.1: query_lisubscribers.report
Type: Update/Add
```
select * from view_listsubscribers_data_list
```
