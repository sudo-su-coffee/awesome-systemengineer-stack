# Healthcare & Telemedicine Architecture (Practo)

Building for the healthcare industry requires entirely different architectural paradigms compared to standard consumer apps because the legal penalties for data breaches are severe.

## 1. HIPAA Compliance & E2E Encryption
You cannot legally store a patient's medical diagnosis or chat history with a doctor in plain text in PostgreSQL.
- **The Architecture**: You must implement **End-to-End Encryption (E2EE)** (like WhatsApp).
- When a patient sends an image of a prescription, it is encrypted on their phone using a public key. The Go backend routes the scrambled data to the doctor.
- The Go backend literally cannot read the image even if it wants to. Only the doctor's private key (stored securely on their device) can decrypt the image. If your database is hacked, the hackers get nothing but mathematical garbage.

## 2. Audit Trails (The "Immutable Law")
In an e-commerce app, if an admin changes a price, it's fine. In a healthcare app, if a doctor changes a diagnosis on a record, it must be legally auditable forever.
- Similar to Fintech, you use **Event Sourcing**.
- Any change to a medical record generates a cryptographically signed hash (sometimes stored on a private blockchain or an immutable AWS QLDB ledger). This proves in a court of law that a record was not tampered with.

## 3. The Slot Booking Engine (Concurrency)
When a famous doctor opens up 10 slots for tomorrow, and 500 patients try to book them at 9:00 AM, the database must flawlessly handle the locking.
- We use **Redis Redlock** or Postgres `SELECT FOR UPDATE`.
- The moment a patient clicks "Book 9:00 AM", the row is locked for 5 minutes. If they don't complete the payment gateway transaction in 5 minutes, a Go background worker releases the lock and gives the slot to the next person in line.
