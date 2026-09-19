# Field_Level_Encryption_PostgreSQL
Production-oriented PostgreSQL field-level encryption using AES-256-GCM, with deterministic encryption for Primary Keys, Foreign Keys, composite keys, and UNIQUE constraints. Includes automatic schema discovery, migration, backfill, encrypted data insertion, verification, and KMS integration.

Key Features
AES-256-GCM encryption for database fields
Deterministic encryption for Primary Keys, Foreign Keys, composite keys, and UNIQUE constraints
Automatic PostgreSQL schema discovery
Database migration and plaintext-to-ciphertext backfill
Encryption-aware data insertion
Foreign-key handling during migration
Key-version tracking and encryption verification
Support for KMS-based key management
Production-oriented configuration and documentation
Purpose
The implementation is intended to be deployed in a separate test environment so the backend/security team can review, test, modify, and extend the encryption workflow against their PostgreSQL environment.

Security Note
Production encryption keys, credentials, .env files, local KMS state, database dumps, and other sensitive information are intentionally excluded from this repository.

Testing
The implementation should be validated against the target test PostgreSQL environment before any production deployment.

Run Commands & Explanation:

Clone the repository:
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd <PROJECT_FOLDER>

Install dependencies:
npm install

Configure environment variables:
DATABASE_URL=postgresql://:@:/

KMS_PROVIDER=aws
AWS_KMS_KEY_ID=<TEST_KMS_KEY_ID>
AWS_REGION=<AWS_REGION>

Schema Discovery
Scan the PostgreSQL database:

npm run scan
node db/scripts/discoverSchema.js

Encryption Migration:

Perform the encryption migration:
node auto-encrypt-all.js

If the test environment is ready and the migration is approved:
node auto-encrypt-all.js --confirm

Encrypt Everything / CLI
Run the encryption command through the main application:

npm run encrypt-all
node secure-dashboard.js encrypt-all

Insert Encrypted Data:
Insert a new encrypted row:
Example:
node secure-dashboard.js insert-row --table=families --data='{"hhid":"HH-TEST-0001","name":"Test Family"}'

Recommended Testing Sequence:

git clone <YOUR_REPOSITORY_URL>
cd <PROJECT_FOLDER>

npm install

Configure test PostgreSQL + test KMS
Create .env locally
npm run scan

Review discovered schema
node auto-encrypt-all.js

If the dry-run is approved:
node auto-encrypt-all.js --confirm

Test encrypted application writes
node secure-dashboard.js insert-row --table=

--data='<JSON_DATA>'
Run tests
npm test
