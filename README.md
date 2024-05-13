# BDFD-Basic-Ticket-System
Basic button based ticket system. you will need one variable for this to work. <br>
| name | value |
| ---- | ----  |
| ticketCategory | |

## dev/setTicketPanel
unlike the other command in this guide this one will be a text trigger set in this example as `dev/setTicketPanel` but you can name this whatever you want. <br>
<br>
This command will display the panel users will use to open tickets you can place this anywhere in your server like your rules channel. <br>
The reasom we use a text trigger here is so the ticket panel doesnt display who used the command and makes for a more seamless look.
```
$nomention
$if[$isAdmin[$authorID]]
$deletecommand
$if[$getServerVar[ticketCategory;$guildID]==]
$title[No Ticket Category]
$description[> Use `/set-category` in a channel within the category you want your tickets to appear in.]
$else

$title[Create a Ticket]
$description[> Click the buttons below to open a ticket.]
$addButton[no;report;Report;danger]
$addButton[no;feedback;Give Feedback;success]
$addButton[no;other;Other;primary]
$endif
$endif
```

## /set-category
this slash command will be used in a channel within the category you want your tickets to appear in.
```
$nomention
$if[$isAdmin[$authorID]]
$setServerVar[ticketCategory;$parentID;$guildID]
$title[Success]
$endif
```

