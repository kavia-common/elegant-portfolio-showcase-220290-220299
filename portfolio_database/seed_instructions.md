# Portfolio Database Seeding and Indexing (MongoDB)

This guide provides copy-pasteable mongosh one-liners to create collections, insert minimal seed data, and create indexes for the portfolio database.

IMPORTANT
- Do NOT edit db_connection.txt.
- All commands below read the connection command directly from db_connection.txt.
- Run each command as a single line in your shell (bash/sh).

Prerequisites
- Ensure MongoDB is running and credentials are valid.
- Verify db_connection.txt exists in this folder.

Check connection command:
cat db_connection.txt

Tip: The file contains a full mongosh command like:
mongosh mongodb://appuser:dbuser123@localhost:5000/myapp?authSource=admin


1) Create Collections
Run these to create required collections: about, projects, experience, contacts.

bash -lc "$(cat db_connection.txt) --eval 'db.createCollection(\"about\")'"
bash -lc "$(cat db_connection.txt) --eval 'db.createCollection(\"projects\")'"
bash -lc "$(cat db_connection.txt) --eval 'db.createCollection(\"experience\")'"
bash -lc "$(cat db_connection.txt) --eval 'db.createCollection(\"contacts\")'"


2) Insert Minimal Seed Data

About (single minimal doc)
bash -lc "$(cat db_connection.txt) --eval 'db.about.insertOne({ name: \"John Doe\", title: \"Full-Stack Developer\", summary: \"I build delightful, performant web applications.\", updatedAt: new Date() })'"

Projects (2-3 items, separate one-liners)
bash -lc "$(cat db_connection.txt) --eval 'db.projects.insertOne({ title: \"Animated Portfolio\", slug: \"animated-portfolio\", description: \"Modern, animated personal portfolio built with React and Tailwind CSS.\", tech: [\"React\", \"Tailwind\"], order: 1, featured: true, url: \"https://example.com/portfolio\", repo: \"https://github.com/example/portfolio\", createdAt: new Date() })'"
bash -lc "$(cat db_connection.txt) --eval 'db.projects.insertOne({ title: \"API Backend\", slug: \"api-backend\", description: \"Express API serving portfolio data with MongoDB.\", tech: [\"Node.js\", \"Express\", \"MongoDB\"], order: 2, featured: false, url: \"https://example.com/api\", repo: \"https://github.com/example/api-backend\", createdAt: new Date() })'"
bash -lc "$(cat db_connection.txt) --eval 'db.projects.insertOne({ title: \"Design System\", slug: \"design-system\", description: \"Reusable UI components and tokens for consistent branding.\", tech: [\"Storybook\", \"TypeScript\"], order: 3, featured: false, url: \"https://example.com/design-system\", repo: \"https://github.com/example/design-system\", createdAt: new Date() })'"

Experience (2-3 items, separate one-liners)
bash -lc "$(cat db_connection.txt) --eval 'db.experience.insertOne({ company: \"Acme Corp\", role: \"Senior Developer\", startDate: ISODate(\"2022-01-01T00:00:00Z\"), endDate: null, summary: \"Leading frontend architecture and performance initiatives.\", highlights: [\"Built component library\", \"Reduced LCP by 35%\"], createdAt: new Date() })'"
bash -lc "$(cat db_connection.txt) --eval 'db.experience.insertOne({ company: \"TechSoft\", role: \"Full-Stack Engineer\", startDate: ISODate(\"2020-05-01T00:00:00Z\"), endDate: ISODate(\"2021-12-31T00:00:00Z\"), summary: \"Delivered end-to-end web solutions.\", highlights: [\"GraphQL adoption\", \"CI/CD automation\"], createdAt: new Date() })'"
bash -lc "$(cat db_connection.txt) --eval 'db.experience.insertOne({ company: \"Startup XYZ\", role: \"Frontend Developer\", startDate: ISODate(\"2018-06-01T00:00:00Z\"), endDate: ISODate(\"2020-04-30T00:00:00Z\"), summary: \"Shipped MVP and growth features.\", highlights: [\"A/B testing framework\", \"Design system rollout\"], createdAt: new Date() })'"

Contacts (optional structure example; generally inserted via app form)
bash -lc "$(cat db_connection.txt) --eval 'db.contacts.insertOne({ name: \"Test User\", email: \"test@example.com\", message: \"Hello from seed.\", createdAt: new Date() })'"


3) Create Indexes
Create the following indexes one by one.

Projects: order ascending
bash -lc "$(cat db_connection.txt) --eval 'db.projects.createIndex({ order: 1 })'"

Projects: featured ascending (boolean)
bash -lc "$(cat db_connection.txt) --eval 'db.projects.createIndex({ featured: 1 })'"

Experience: startDate descending
bash -lc "$(cat db_connection.txt) --eval 'db.experience.createIndex({ startDate: -1 })'"

Contacts: createdAt descending
bash -lc "$(cat db_connection.txt) --eval 'db.contacts.createIndex({ createdAt: -1 })'"


4) Verification
Run these to verify counts and indexes.

Count documents
bash -lc "$(cat db_connection.txt) --eval 'printjson({ about: db.about.countDocuments({}), projects: db.projects.countDocuments({}), experience: db.experience.countDocuments({}), contacts: db.contacts.countDocuments({}) })'"

List indexes
bash -lc "$(cat db_connection.txt) --eval 'db.projects.getIndexes()'"
bash -lc "$(cat db_connection.txt) --eval 'db.experience.getIndexes()'"
bash -lc "$(cat db_connection.txt) --eval 'db.contacts.getIndexes()'"

Optional: Preview a few documents
bash -lc "$(cat db_connection.txt) --eval 'db.projects.find().sort({ order: 1 }).limit(5).toArray()'"
bash -lc "$(cat db_connection.txt) --eval 'db.experience.find().sort({ startDate: -1 }).limit(5).toArray()'"

Troubleshooting
- If connection fails, confirm MongoDB is running and that db_connection.txt contains a valid mongosh command.
- Ensure your shell is bash/sh compatible for bash -lc usage.
- If authentication errors appear, verify credentials in startup.sh and the users in MongoDB.
