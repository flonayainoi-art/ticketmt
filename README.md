# TicketHub - Event Ticketing Platform

A modern, full-stack event ticketing web application inspired by Ticketmaster with original design and branding.

## 🎯 Core Features

### User Features
- **Authentication**: Email/password and Google OAuth login
- **Event Discovery**: Advanced search and filtering (location, date, category, price range)
- **Event Details**: Rich media, descriptions, venue information, ratings
- **Seat Selection**: Interactive visual seat map with real-time availability
- **Checkout Flow**: Shopping cart, secure payment processing with Stripe
- **Ticket Management**: Digital QR code tickets, email confirmations
- **User Dashboard**: View tickets, booking history, saved events, profile management

### Organizer Features
- **Event Management**: Create, edit, publish events
- **Pricing Tiers**: Multiple ticket types with different pricing
- **Seat Configuration**: Custom venue layouts and capacity management
- **Analytics**: Sales analytics, attendance tracking, revenue reports
- **Promo Management**: Create and manage discount codes

### Admin Features
- **Platform Analytics**: User metrics, revenue insights
- **Content Moderation**: Event and user management
- **System Health**: Performance monitoring

## 🏗️ Tech Stack

### Frontend
- **Framework**: Next.js 14+ (React 18+)
- **Styling**: Tailwind CSS + Radix UI components
- **State Management**: React Context + TanStack Query
- **Authentication**: NextAuth.js
- **Forms**: React Hook Form + Zod validation
- **Animation**: Framer Motion
- **Visualization**: Chart.js for analytics

### Backend
- **Runtime**: Node.js
- **Framework**: Express.js
- **API**: REST with OpenAPI documentation
- **Database ORM**: Prisma
- **Authentication**: JWT + OAuth providers
- **Payment**: Stripe SDK
- **Email**: Nodemailer / SendGrid
- **QR Code**: QRCode library
- **Validation**: Joi / express-validator

### Database
- **Primary**: PostgreSQL
- **Caching**: Redis (optional, for real-time seat locking)
- **File Storage**: AWS S3 / Cloudinary (for images)

### DevOps
- **Containerization**: Docker
- **Orchestration**: Docker Compose
- **Cloud**: AWS / DigitalOcean / Vercel (frontend)

## 📁 Project Structure

```
ticketmaster/
├── frontend/                 # Next.js application
│   ├── app/                 # App router
│   ├── components/          # Reusable components
│   ├── lib/                 # Utilities and helpers
│   ├── public/              # Static assets
│   ├── styles/              # Global styles
│   └── package.json
├── backend/                 # Express API
│   ├── src/
│   │   ├── routes/          # API endpoints
│   │   ├── controllers/     # Business logic
│   │   ├── models/          # Database models
│   │   ├── middleware/      # Custom middleware
│   │   ├── services/        # Business services
│   │   ├── utils/           # Utilities
│   │   └── config/          # Configuration
│   ├── package.json
│   └── .env.example
├── docker/
│   ├── Dockerfile.frontend
│   ├── Dockerfile.backend
│   └── docker-compose.yml
├── docs/                    # Documentation
│   ├── API.md              # API documentation
│   ├── DATABASE.md         # Database schema
│   ├── SETUP.md            # Setup guide
│   └── DEPLOYMENT.md       # Deployment guide
└── .env.example            # Environment variables template
```

## 🚀 Quick Start

### Prerequisites
- Node.js 18+
- Docker & Docker Compose
- PostgreSQL 14+
- Git

### Local Development

1. **Clone and Setup**
```bash
# Install dependencies
cd frontend && npm install
cd ../backend && npm install
cd ..
```

2. **Environment Configuration**
```bash
cp .env.example .env.local
# Edit .env.local with your credentials
```

3. **Database Setup**
```bash
cd backend
npx prisma migrate dev
```

4. **Run Applications**
```bash
# Terminal 1 - Frontend
cd frontend
npm run dev

# Terminal 2 - Backend
cd backend
npm run dev
```

Frontend: http://localhost:3000
Backend API: http://localhost:5000

### Docker Compose
```bash
docker-compose up -d
```

## 📚 Documentation

- [API Reference](docs/API.md)
- [Database Schema](docs/DATABASE.md)
- [Setup Guide](docs/SETUP.md)
- [Deployment Guide](docs/DEPLOYMENT.md)

## 🔐 Security Features

- JWT token-based authentication
- OAuth 2.0 integration (Google, GitHub)
- Rate limiting on API endpoints
- CORS configuration
- Input validation and sanitization
- Secure password hashing (bcrypt)
- HTTPS enforcement in production
- CSP headers

## 📊 Advanced Features

- Real-time seat locking with Redis
- Event recommendation engine
- Dynamic pricing system
- Promo code and discount management
- Advanced analytics dashboard
- Email notifications
- QR code ticket verification
- Refund and cancellation management

## 🤝 Contributing

1. Fork the repository
2. Create feature branch: `git checkout -b feature/amazing-feature`
3. Commit changes: `git commit -m 'Add amazing feature'`
4. Push to branch: `git push origin feature/amazing-feature`
5. Open Pull Request

## 📝 License

This project is licensed under the MIT License - see LICENSE file for details.

## 💬 Support

For support, email support@tickethub.dev or open an issue on GitHub.

---

**Built with ❤️ for event organizers and attendees worldwide.**
