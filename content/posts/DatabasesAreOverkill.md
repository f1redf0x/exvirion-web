+++
title = 'Databases Are Overkill For Personal Projects'
date = 2025-02-07T12:52:22-06:00
draft = false
tags = ["data", "recfiles"]
categories = ["Text is King"]
+++

# What's the Deal with Data?

If you read my [post on the best tech video ever](https://firedfox.netlify.app/posts/unreasonable_plain_text/), you will know that I will always prefer plaintext files over pretty much every other format. In that article I explain how the simplicity of plaintext and the flexibility of unix tools make text not only easier to work with, but remarkably powerful, and un-killable if you know how to use git.

The problem with text is that there is a kind of limit to what it can be used for without adding more abstraction to it. Let's say you want structured data, similar to the kind you might find in a spreadsheet. You'll run into an issue with plaintext because it doesn't force a structure itself. It's freedom becomes a vice in that context. You'll have the freedom to mess up the data.

The next step up is the CSV format. Simple, comma-deliminated plaintext is very easy to work with and it can be imported into tools like Excel and Pandas. That's pretty sweet. It has a couple of issues. If you aren't using a spreadsheet application to edit data, it's remarkably easy to add a rogue comment and mess up the structure of the file. And on the subject of rogue commas, if you include commas in the data field (fairly easy to do if you insert json objects into the document), it can turn a really simple task into a difficult one. You could switch to a TSV file where the delimeter is tabs, but you'll run into the same problem eventually.

You could go with an XLSX file. XSLX is essentially a compressed file containing a bunch of CSV files and XML data to format, structure, and prettify the data. If you go with that though, you are stuck with using Excel, pandas, or one of Microsoft's other toolkits. You'll lose any access to things like grep, ripgrep, sed, awk, etc. Not only that, if Microsoft changes its licensing or increases the fees to renew your 365 apps next year, your company might decide to use other tools. (I get that's unlikely, but I prefer FOSS to proprietary and subscription based tools because there is an element of unpredictability to the market. If you don't believe me, I've lost access to Docker, ELK, and VMWare due to licensing within the span of 3 years).

So, I need something more powerful than CSV, but less proprietary than XLSX. This is where sqlite tends to enter the chat. Sqlite is one of the most used applications on the planet. It's got all the goodies you'd expect from a database, but all the data is stored in a single sqlite flatfile. There are gui editing tools, programming libraries, and the sqlite3 cli tool. It forces data structure and won't let you insert things that'll jack up your dataset. This sounds like a perfect fit for my preferences, right?

Weeeeeeellllllll...

The jump from CSV to XLSX lost me access to using unix utils to carve through data. This for me is an unacceptable loss. In the case of sqlite, I have new open-souce tools that'll give me structured data, but I'm locked into the sqlite ecosystem now. There's also the matter of complexity. The average salary of a database administrator is [$101,510](https://www.bls.gov/ooh/computer-and-information-technology/database-administrators.htm#tab-5) because databases are complicated and dealing with one requires specialized training. 

This complication is no small factor. On the dev team I was a part of, I saw first hand how the structuring of queries could take hours because fine-tuning things for speed, creating high availability, and missing minor details could create all sorts of ruckus. 

Also, as a matter of personal preference, I find sql queries to be cumbersome. The syntax is simple, but it's powerful and easy to mess up.

# Requirements

So I need to find a file format that:

1. Is human-readable
2. Can be manipulated by unix utilities
3. Can enforce a structure on data
4. Can be queried for information
5. Stays as close as possible to plaintext
6. Is Free and Open Source

# Recfiles

The answer to my cries is a filetype that's so rarely used, ChatGPT doesn't even know how to structure queries for it (I checked). It's called a recfile. A recfile is a plaintext file that has a schema at the top, and all the data adhering to that schema underneath. It's got an insertion tool called recins, and it can translate data into CSV format and can import CSV into itself.

## An Example Recfile

A recfile is completely human readable. To teach myself the concept, I made a little contacts application. First thing you need is a recfile, which acts sort of as your database. A recfile looks like this:
```
# -*- mode: rec -*-

# This is a template for a recfile. 
# Each record represents a contact with various details.

%rec: Contact
%mandatory: Full_Name
%optional: Email
%optional: Phone_Number
%optional: Organization
%optional: Role
%optional: Website
%optional: Nickname
%optional: Birthday
%optional: Home_Address
%optional: Work_Address
%optional: Notes

# records

Full_Name: John Doe
Email: john.doe@example.com
Phone_Number: +1234567890
Organization: Example Corp
Role: Developer
Website: https://example.com
Nickname: Johnny
Birthday: 1990-01-01
Home_Address: 123 Main St, Anytown, USA
Work_Address: 456 Corporate Blvd, Business City, USA
Notes: This is a sample contact record.

# End of contacts.rec
```

The `# -*- mode: rec -*-` line indicates that the file is a recfile.

The `%rec: Contact` line is setting up the recfile equivalent of a table in a database. It's called a template.

The `%mandatory: Full_Name` is the start of the schema. I use mandatory for primary identifiers and optional for ... well... optional data values

The rest of the file was created by inserting data. Let me show you how to insert data into a recfile.

# Inserting data:
The following is a script that I use to insert data into the recfile:

```
#!/bin/bash

# Define the recfile path
RECFILE="/home/ross/Data/address_book.rec"

# Prompt for user input
read -p "Full Name: " full_name
read -p "Email: " email
read -p "Phone Number: " phone_number
read -p "Organization: " organization
read -p "Role: " role
read -p "Website: " website
read -p "Nickname: " nickname
read -p "Birthday (YYYY-MM-DD): " birthday
read -p "Home Address: " home_address
read -p "Work Address: " work_address
read -p "Notes: " notes

# Use recins to insert the values into the recfile
recins -t Contact \
    -f "Full_Name" -v "$full_name" \
    -f "Email" -v "$email" \
    -f "Phone_Number" -v "$phone_number" \
    -f "Organization" -v "$organization" \
    -f "Role" -v "$role" \
    -f "Website" -v "$website" \
    -f "Nickname" -v "$nickname" \
    -f "Birthday" -v "$birthday" \
    -f "Home_Address" -v "$home_address" \
    -f "Work_Address" -v "$work_address" \
    -f "Notes" -v "$notes" \
    "$RECFILE"

echo "Record inserted successfully into $RECFILE."
```

It's a simple script. It just prompts me for all the data by field. I enter the data and it uses the recins command.

`-t` is for the template (i.e. the table) that you are inserting into. A recfile can have multiple.

`-f` is for the field and `-v is for the value`

`$RECFILE` is for the file you want recins to send the data.

You can enforce datatypes, but I never do because I love plaintext. More data for how to structure a recfile can be found in [the docs](https://www.gnu.org/software/recutils/manual/recutils.html#Invoking-recsel).

# Querying the data

Querying the data is easy too. I don't need anything fancy here, so I'll show you the easiest query you can run. 

```
#!/bin/bash

# Define the recfile path
RECFILE="/home/ross/Data/address_book.rec"

# Prompt for user input
read -p "Enter the Full Name to search for: " search_name

# Use recsel to search for a keyword in the recfile
recsel -q "$search_name" "$RECFILE"

# Check if the search returned any results
if [ $? -eq 0 ]; then
    echo "Search completed successfully."
else
    echo "No records found for Full Name: $search_name."
fi

```

Most of the above script is just because I want to build out a contact recfile, but you only really need the `recsel -q "$search_name" "$RECFILE"` line. It queries the recfile for the text contained after the `-q` flag. If you were to run:

```
recsel -q "John" /home/ross/Data/address_book.rec
```

It will pull all of John Doe's information:

```
Full_Name: John Doe
Email: john.doe@example.com
Phone_Number: +1234567890
Organization: Example Corp
Role: Developer
Website: https://example.com
Nickname: Johnny
Birthday: 1990-01-01
Home_Address: 123 Main St, Anytown, USA
Work_Address: 456 Corporate Blvd, Business City, USA
Notes: This is a sample contact record.
```
If you had multiple Johns, you could use further queries to narrow down your results.

# The Unix utils

But you don't have to stop there. The recfile is plaintext. You can run `grep Full_Name: ` on the recfile and pull all of the names in your address book. You can pipe that into `wc -l` to get a count on how many you have, etc. You can use regular expressions to pull all the data you want. The world is your oyster.

# Why do I care?

So the reason I want to have an option like this is for building pentesting loot files. With just a little effort, I can convert scan results from nmap, insert user creds, and write notes about testing into a recfile format that I can easily parse with my tools. I was doing something similar to that back in the day with the ELK stack, but it was in a json format and I feel like the recfile might be a better fit for an assignment like that given how it can be parsed with standard tools.
