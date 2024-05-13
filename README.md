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

## $onInteraction
this is the interaction for the 3 ticket buttons and the close ticket button.
```
$nomention
$defer
$var[c;$getServerVar[ticketCategory;$guildID]]
$if[$customID==report]
$var[text;report-$random[100;9999999]]
$async[c]
$createChannel[$var[text];text;$var[c]]
$endasync
$await[c]
$var[c;$findChannel[$var[text]]]
$async[e]
$var[e;$sendEmbedMessage[$var[c];;> User Report;;> Send details related to your report below like,
> - User your reporting
> - Report reason
> - Proof of accusation;FFFFFF;;;;;;;true;true]]
$endasync
$await[e]
$ephemeral
$removeButtons
your ticket has been created <#$var[c]>
$useChannel[$var[c]]
$editChannelPerms[$channelID;$authorID;+readmessages]
$addButton[no;close;Close Ticket;danger;;;$var[e]]
$stop

$elseif[$customID==feedback]
$var[text;feedback-$random[100;9999999]]
$async[c]
$createChannel[$var[text];text;$var[c]]
$endasync
$await[c]
$var[c;$findChannel[$var[text]]]
$async[e]
$var[e;$sendEmbedMessage[$var[c];;> Server Feedback;;> - Positive, Negative or Neutral?
> - Your feedback.;444444;;;;;;;true;true]]
$endasync
$await[e]
$ephemeral
$removeButtons
your ticket has been created <#$var[c]>
$useChannel[$var[c]]
$editChannelPerms[$channelID;$authorID;+readmessages]
$addButton[no;close;Close Ticket;danger;;;$var[e]]
$stop

$elseif[$customID==other]
$var[text;ticket-$random[100;9999999]]
$async[c]
$createChannel[$var[text];text;$var[c]]
$endasync
$await[c]
$var[c;$findChannel[$var[text]]]
$async[e]
$var[e;$sendEmbedMessage[$var[c];;> Ticket Details;;> - Send ticket details below;444444;;;;;;;true;true]]
$endasync
$await[e]
$ephemeral
$removeButtons
your ticket has been created <#$var[c]>
$useChannel[$var[c]]
$editChannelPerms[$channelID;$authorID;+readmessages]
$addButton[no;close;Close Ticket;danger;;;$var[e]]
$stop
$endif
$if[$customID==close]
$deleteChannels[$channelID]
$endif
```
