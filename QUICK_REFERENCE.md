# Developer Quick Reference

## Project Structure at a Glance

```
ticketmaster/
├── frontend/                 # Next.js React application
├── backend/                  # Express.js API server
├── docker/                   # Docker configurations
├── docs/                     # Documentation
├── docker-compose.yml        # Local development setup
├── README.md                 # Project overview
└── IMPLEMENTATION_SUMMARY.md # What's been built
```

## Quick Commands

### Frontend
```bash
cd frontend
npm install              # Install dependencies
npm run dev             # Start development server (http://localhost:3000)
npm run build           # Build for production
npm run start           # Start production server
npm run lint            # Run ESLint
npm run type-check      # Check TypeScript
```

### Backend
```bash
cd backend
npm install              # Install dependencies
npm run dev             # Start with hot reload (http://localhost:5000)
npm run build           # Compile TypeScript
npm run migrate         # Run database migrations
npm run prisma:generate # Generate Prisma client
```

### Docker
```bash
docker-compose up -d                    # Start all services
docker-compose down                     # Stop all services
docker-compose logs -f backend          # View backend logs
docker-compose exec backend npm run ... # Run commands in container
```

## Environment Variables

### Critical Variables (.env.local)
```
# Frontend
NEXT_PUBLIC_API_URL=http://localhost:5000
NEXTAUTH_SECRET=<min-32-character-secret>

# Backend
DATABASE_URL=postgresql://user:password@localhost:5432/ticketmaster_dev
JWT_SECRET=<secret-key>
STRIPE_SECRET_KEY=sk_test_...
```

See `.env.example` for complete list.

## API Endpoints Summary

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| POST | /api/auth/signup | - | Create account |
| POST | /api/auth/login | - | Login user |
| GET | /api/auth/me | ✓ | Get current user |
| GET | /api/events | - | List events |
| GET | /api/events/:id | - | Get event details |
| POST | /api/events | ✓ | Create event |
| POST | /api/bookings | ✓ | Create booking |
| GET | /api/bookings/my-bookings | ✓ | Get user bookings |
| GET | /api/tickets/my-tickets | ✓ | Get user tickets |
| POST | /api/payments/intent | ✓ | Create payment intent |

Full API documentation: See `docs/API.md`

## Database Models

```
User ──┬─→ Event (organizer)
       └─→ Booking ──┬─→ Event
                     ├─→ Ticket ──┬─→ Seat
                     │            └─→ Event
                     └─→ Review ──→ Event

Venue ─→ Section ─→ Seat (status tracking)
         ↓
         SeatLock (real-time locking)

PromoCode (discount management)
```

## File Structure

### Frontend Key Files
```
frontend/
├── app/
│   ├── layout.tsx                    # Root layout
│   ├── (main)/page.tsx              # Homepage
│   ├── (auth)/login/page.tsx        # Login page
│   ├── (auth)/signup/page.tsx       # Signup page
│   ├── dashboard/page.tsx           # User dashboard
│   ├── api/auth/[...nextauth].ts    # Auth configuration
│   └── providers.tsx                # App providers
├── components/
│   ├── ui/                          # Base components
│   ├── auth/                        # Auth components
│   ├── events/                      # Event components
│   └── dashboard/                   # Dashboard components
├── lib/
│   ├── api/
│   │   ├── events.ts               # Event API calls
│   │   └── client.ts               # Axios instance
│   └── utils.ts                    # Utilities
├── styles/
│   └── globals.css                 # Global styles
└── public/                         # Static assets
```

### Backend Key Files
```
backend/
├── src/
│   ├── index.ts                     # Server entry point
│   ├── routes/
│   │   ├── auth.ts                 # Auth endpoints
│   │   ├── events.ts               # Event endpoints
│   │   ├── bookings.ts             # Booking endpoints
│   │   ├── tickets.ts              # Ticket endpoints
│   │   └── payments.ts             # Payment endpoints
│   ├── middleware/
│   │   ├── auth.ts                 # Auth middleware
│   │   ├── errorHandler.ts         # Error handling
│   │   └── requestLogger.ts        # Logging
│   ├── utils/
│   │   ├── jwt.ts                  # JWT utilities
│   │   ├── password.ts             # Password hashing
│   │   ├── email.ts                # Email sending
│   │   └── qrcode.ts               # QR code generation
│   └── config/
│       ├── database.ts             # Prisma client
│       └── stripe.ts               # Stripe setup
├── prisma/
│   └── schema.prisma               # Database schema
└── package.json
```

## Common Workflows

### Adding a New Event Endpoint

1. **Create route in** `backend/src/routes/events.ts`
2. **Add middleware** if authentication required
3. **Test with curl or Postman**
4. **Update API documentation**

Example:
```typescript
// backend/src/routes/events.ts
router.post('/featured', async (req: AuthRequest, res: Response) => {
  try {
    const featured = await prisma.event.findMany({
      where: { featured: true },
      take: 5
    });
    res.json(featured);
  } catch (error) {
    res.status(500).json({ error: 'Failed' });
  }
});
```

### Adding a New Frontend Page

1. **Create file** in `frontend/app`
2. **Add route** with layout group if needed
3. **Import components** and hook up API calls
4. **Add styling** with Tailwind

Example:
```tsx
// frontend/app/(main)/events/page.tsx
'use client';

import { EventGrid } from '@/components/events/event-grid';
import { useQuery } from '@tanstack/react-query';
import { fetchEvents } from '@/lib/api/events';

export default function EventsPage() {
  const { data: events } = useQuery({
    queryKey: ['events'],
    queryFn: fetchEvents
  });

  return (
    <div className="container mx-auto px-4 py-8">
      <EventGrid events={events || []} isLoading={false} />
    </div>
  );
}
```

### Adding a New Component

1. **Create** `components/feature/my-component.tsx`
2. **Export component** function
3. **Add TypeScript interfaces**
4. **Use in pages**

Example:
```tsx
// components/events/event-card.tsx
import { Button } from '@/components/ui/button';

interface EventCardProps {
  title: string;
  date: string;
}

export function EventCard({ title, date }: EventCardProps) {
  return (
    <div className="border rounded-lg p-4">
      <h3>{title}</h3>
      <p>{date}</p>
      <Button>View</Button>
    </div>
  );
}
```

## Debugging Tips

### Frontend
```bash
# Clear cache
rm -rf .next

# Check logs
npm run dev  # See server logs

# Browser DevTools
- Network tab: Check API calls
- Console: Check for errors
- React DevTools: Check state
```

### Backend
```bash
# View logs
pm2 logs

# Check database
psql -U ticketmaster -d ticketmaster_dev

# Test endpoint
curl http://localhost:5000/api/events
```

### Docker
```bash
# Exec into container
docker-compose exec backend bash
docker-compose exec postgres psql -U ticketmaster

# Check logs
docker-compose logs -f
```

## Performance Checklist

- [ ] Add database indexes for frequently queried fields
- [ ] Implement API response pagination
- [ ] Cache frequently accessed data with Redis
- [ ] Optimize images (use Next.js Image component)
- [ ] Enable gzip compression
- [ ] Implement lazy loading
- [ ] Monitor API response times
- [ ] Set up monitoring/logging

## Security Checklist

- [ ] Use HTTPS in production
- [ ] Set secure headers (CORS, CSP, HSTS)
- [ ] Implement rate limiting
- [ ] Validate and sanitize inputs
- [ ] Use parameterized queries (Prisma does this)
- [ ] Keep dependencies updated
- [ ] Use environment variables for secrets
- [ ] Implement CSRF protection if needed
- [ ] Set secure cookie flags

## Deployment Checklist

- [ ] Update environment variables for production
- [ ] Run database migrations
- [ ] Build both frontend and backend
- [ ] Set up SSL/HTTPS
- [ ] Configure DNS
- [ ] Enable monitoring
- [ ] Set up backups
- [ ] Configure CI/CD
- [ ] Test critical workflows
- [ ] Plan rollback strategy

## Documentation Files

| Document | Purpose |
|----------|---------|
| README.md | Project overview |
| SETUP.md | Installation & setup |
| API.md | API documentation |
| DATABASE.md | Database schema |
| DEPLOYMENT.md | Deployment guide |
| FRONTEND_COMPONENTS.md | UI component guide |
| IMPLEMENTATION_SUMMARY.md | What's been built |

## Useful Resources

- [Next.js Docs](https://nextjs.org/docs)
- [Express Docs](https://expressjs.com)
- [Prisma Docs](https://prisma.io/docs)
- [Tailwind CSS](https://tailwindcss.com/docs)
- [Stripe Docs](https://stripe.com/docs)
- [PostgreSQL Docs](https://www.postgresql.org/docs)

## Getting Help

1. Check the relevant documentation file in `/docs`
2. Review similar implementation in codebase
3. Check error logs and stack traces
4. Search project comments with grep
5. Refer to dependency documentation

---

**Last Updated**: May 4, 2026
**Project**: TicketHub - Event Ticketing Platform
