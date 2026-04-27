# Backend Testing And Operations Conventions

## Test Placement

Unit specs should live alongside the code they validate.

Conventions:

- place `*.spec.ts` beside the source file or inside the same feature directory
- keep end-to-end tests under `test/` for full-application bootstraps
- mirror module boundaries so the owner of a feature can find its tests quickly

## Unit Testing Strategy

Unit tests should focus on service behavior first.

Priority order:

1. services with branching business logic
2. controllers only where routing or translation behavior is meaningful
3. helper mappers and rule functions where incorrect behavior is easy to miss

Conventions:

- create Nest testing modules only with dependencies the unit needs
- mock Prisma, storage, email, and external integrations explicitly
- verify outcomes and side effects, not private implementation details
- keep tests narrow and intention-revealing

```typescript
// src/client/client.service.spec.ts
describe('ClientService', () => {
  let service: ClientService;
  let prisma: DeepMockProxy<PrismaService>;

  beforeEach(async () => {
    prisma = mockDeep<PrismaService>();

    const module = await Test.createTestingModule({
      providers: [
        ClientService,
        { provide: PrismaService, useValue: prisma },
        { provide: EmailService,  useValue: { sendClientRemovedNotification: jest.fn().mockResolvedValue(undefined) } },
      ],
    }).compile();

    service = module.get(ClientService);
  });

  it('throws NotFoundException when client does not exist', async () => {
    prisma.client.findFirst.mockResolvedValue(null);

    await expect(service.removeClient('biz-1', 'missing-id')).rejects
      .toThrow(NotFoundException);
  });

  it('deletes the client when found', async () => {
    const client = { id: 'client-1', businessId: 'biz-1' } as any;
    prisma.client.findFirst.mockResolvedValue(client);
    prisma.client.delete.mockResolvedValue(client);

    await service.removeClient('biz-1', 'client-1');

    expect(prisma.client.delete).toHaveBeenCalledWith({ where: { id: 'client-1' } });
  });
});
```

## End-To-End Testing

E2E tests should prove that the real HTTP surface is wired correctly.

Good candidates:

- auth flow boundaries
- critical onboarding endpoints
- billing webhook or checkout initiation paths
- resource ownership rules that are easy to misconfigure across guard and service layers

Keep E2E coverage selective. It should validate confidence-critical flows, not replace service tests.

## Migrations And Seeds

Database changes should be deliberate and reproducible.

Conventions:

- keep Prisma schema changes paired with migrations
- run local migration application before merging backend data-model changes
- keep seeds idempotent where possible, especially for foundational reference data
- separate broad seeds from targeted phase scripts when the seed dataset is large or optional

## Environment-Aware Commands

The documented commands in `package.json` should remain the single source of truth for normal workflow.

Expected commands:

```bash
npm run build
npm run start:dev
npm run start:local
npm run lint
npm test
npm run test:cov
npm run test:e2e
npm run migrate:local
npm run seed:local
```

Conventions:

- prefer scripted commands over ad hoc shell invocations in docs and team workflow
- keep `.env.local`-aware commands explicit for local development
- avoid hiding important production behavior behind undocumented one-off scripts

## Observability And Logging

Logging should support debugging and operations without becoming noise.

Conventions:

- create a logger per service when useful, following the current `Logger(ClassName.name)` pattern
- log key state transitions, failure points, and external integration failures
- do not log secrets, tokens, or sensitive personal data
- rely on interceptors or shared infrastructure for request-level logging where appropriate

```typescript
// declare at the top of every service that benefits from structured logs
private readonly logger = new Logger(ClientService.name);

// use the appropriate level
this.logger.log('Client removed', { clientId, businessId });
this.logger.warn('Invitation already accepted', { invitationId });
this.logger.error('S3 upload failed', error.stack);
```

## Review Expectations

Backend changes should not merge if they introduce:

- controller-heavy business logic
- raw Prisma records returned directly to API callers
- missing auth or authorization on protected endpoints
- data-access duplication across modules
- missing tests for non-trivial service behavior