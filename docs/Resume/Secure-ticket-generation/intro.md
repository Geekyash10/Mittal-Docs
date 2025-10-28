# Secure Ticketing System (JSCoP Event)

I am a technical member of the Optica society in my college. Every year
we organize an event called **JSCoP**. One problem we faced was tickets.
Earlier, tickets were checked manually, and some students used duplicate
or fake tickets, which wasted time at entry.

So, I made a **digital ticketing system**. Whenever a person registers,
the backend creates a ticket with a unique ID in MongoDB. From that ID,
I generate a **QR code** and also send an **OTP** to the user's email.
At the entry gate, when the QR is scanned and OTP entered, the backend
checks in the database if it is valid and unused.

I built the frontend in **React**, backend in **Node.js with Express**,
and used **MongoDB** to store ticket data. We also used **NodeMailer**
to send the ticket with QR and OTP by email.

We used this system in our JSCoP event for more than **200 students**,
and it worked perfectly --- no fake tickets got in, and the entry became
very fast.

------------------------------------------------------------------------

## How Uniqueness Works in MongoDB

In MongoDB, every document gets a unique _id. By default, it is
an ObjectId, which is 12 bytes long. It is made up of three
things:

1.  **Timestamp (4 bytes):** shows when the record was created.
2.  **Machine + Process Identifier (5 bytes):** represents the server
    and process creating the record.
3.  **Counter (3 bytes):** increases whenever a new document is added.

Together these three parts make sure no two IDs are ever the same. For
example, even if two tickets are created at the exact same second on the
same server, the counter makes them different. And if multiple servers
were creating tickets at the same time, the machine and process
identifier would still make them unique.
