Here’s a fully consolidated Markdown file with all frontend and backend setup steps in one place, organized, easy to read, and with commands ready to copy-paste:

⸻

Tijarah Project – Local Development Setup

This guide explains how to set up the frontend and backend of the Tijarah project locally.

⸻

1. Frontend Setup

Step 1: Clone Repository and Switch to QA Branch

git clone https://github.com/TijarahV2/tijarah-backend.git
cd tijarah-backend
git checkout qa


⸻

Step 2: Install Dependencies

npm install --legacy-peer-deps


⸻

Step 3: Configure Environment

cp .env.local.example .env.local

Update .env.local:

NEXT_PUBLIC_PRODUCTION_API_URL=http://localhost:3003

Update next.config.js:

NEXT_PUBLIC_PRODUCTION_API_URL = "http://localhost:3003";
NEXT_PUBLIC_FRONTEND_URL = "http://localhost:3000";


⸻

2. Backend Setup

Step 1: Clone Repository and Switch to QA Branch

Skip if already cloned for frontend setup.

git clone https://github.com/TijarahV2/tijarah-backend.git
cd tijarah-backend
git checkout qa


⸻

Step 2: Update Backend Files

a) apps/kafka-consumer/src/consumer/memory-monitor.ts
	•	After line 24, add:

new Error('Memory threshold exceeded')

	•	Replace line 38 with:

.error(
  'Memory still high after GC, restarting process',
  new Error('Memory still high after GC'),
)

b) apps/change-stream/src/change-stream.module.ts
	•	Add imports:

import { EventEmitterModule } from '@nestjs/event-emitter';
import { S3Module } from '@the-backend/s3/s3.module';
import { EnvoyModule } from '@the-backend/envoy/envoy.module';

	•	Update the @Module imports:

@Module({
  imports: [
    DatabaseModule,
    LoggerModule,
    KafkaModule,
    ScheduleModule.forRoot(),
    EventEmitterModule.forRoot(),
    S3Module,
    EnvoyModule,
  ],
})


⸻

Step 3: Configure MongoDB
	•	Add your MongoDB URL in development.env.
	•	If using MongoDB Atlas, comment out the following line in libs/database/src/database.module.ts:

// replicaSet: config.databases.main.replicaSet, // Commented out for Atlas SRV connections


⸻

3. Docker Compose Setup

Step 1: Create Development Docker Compose File

cp docker-compose.yaml docker-compose-dev.yaml


⸻

Step 2: Update Environment Variables

Edit docker-compose-dev.yaml for all services:

NODE_ENV=development
TJ_ENV=development


⸻

Step 3: Run Backend Services

docker compose -f docker-compose-dev.yaml up


⸻

Notes
	1.	Ensure both frontend and backend are using the qa branch.
	2.	Update .env.local and next.config.js to point to the correct local URLs.
	3.	Apply backend code modifications before starting Docker Compose.
	4.	MongoDB Atlas users must comment out the replica set line as mentioned above.

⸻

If you want, I can also create a “fully automated single script version” in Markdown where a developer can copy-paste everything, and even the file edits for backend will be applied automatically using inline commands.

Do you want me to make that?
