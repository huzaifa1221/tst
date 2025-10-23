# Frontend Setup
 
### Clone repository and switch to qa branch
git clone https://github.com/TijarahV2/tijarah-backend.git
cd tijarah-backend
git checkout qa
 
### Install dependencies
npm install --legacy-peer-deps
 
### Configure environment
cp .env.local.example .env.local
 
# Update environment variables in .env.local:
NEXT_PUBLIC_PRODUCTION_API_URL=http://localhost:3003
 
# Update next.config.js:
 NEXT_PUBLIC_PRODUCTION_API_URL = "http://localhost:3003"
 NEXT_PUBLIC_FRONTEND_URL = "http://localhost:3000"
 
############################################
# Backend Setup
############################################
 
# Clone repository and switch to qa branch
git clone https://github.com/TijarahV2/tijarah-backend.git
cd tijarah-backend
git checkout qa
 
# Update backend files:
 
## apps/kafka-consumer/src/consumer/memory-monitor.ts
After line 24, add: new Error('Memory threshold exceeded')
Replace line 38 with:
.error(
   'Memory still high after GC, restarting process',
   new Error('Memory still high after GC'),
)
 
## apps/change-stream/src/change-stream.module.ts
# Add imports:
import { EventEmitterModule } from '@nestjs/event-emitter';
import { S3Module } from '@the-backend/s3/s3.module';
import { EnvoyModule } from '@the-backend/envoy/envoy.module';
 
# Update @Module imports:
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
 
# Configure MongoDB:
- Add MongoDB URL in development.env
- If using MongoDB Atlas, comment out the following line in libs/database/src/database.module.ts:
// replicaSet: config.databases.main.replicaSet, // Commented out for Atlas SRV connections
 
############################################
# Docker Compose Setup
############################################
 
# Create a development docker-compose file
cp docker-compose.yaml docker-compose-dev.yaml
 
# Update environment variables in docker-compose-dev.yaml for all services:
NODE_ENV=development
TJ_ENV=development
 
# Run backend services
docker compose -f docker-compose-dev.yaml up
