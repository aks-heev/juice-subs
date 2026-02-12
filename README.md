# Juice Subs - Fresh Juice Subscription Service

A modern, secure web application for managing fresh juice subscription services with Supabase authentication, role-based access control, and comprehensive form validation.

## 🚀 Latest Features

### Authentication & Security
- **Supabase Auth Integration**: Full email/password authentication with secure session management
- **Protected Routes**: `ProtectedRoute` component guards Dashboard and Admin pages
- **Role-Based Access Control**: Admin routes restricted to users with `user_metadata.role === 'admin'`
- **Row Level Security (RLS)**: User-specific data isolation via `user_id` foreign key constraints
- **Secure Database Schema**: Subscriptions linked directly to `auth.users(id)`

### Subscription Plans
Five flexible subscription plans with discounts ranging from 10-25% OFF:
- **Trial Weekly** - Try before you commit
- **Weekly Single** - Your favorite juice, delivered weekly
- **Weekly Variety** - All juices in rotation, delivered weekly
- **Monthly Single** - One juice type, delivered monthly
- **Monthly Variety** - All juices in rotation, delivered monthly

*Variety plans automatically rotate through all available juices using average pricing*

### Form Validation
Comprehensive client-side validation powered by `utils/validation.js`:
- **Phone Numbers**: 10-digit Indian format validation (`^[6-9]\d{9}$`)
- **Email Addresses**: RFC-compliant email validation
- **Names**: Letters and spaces only
- **Addresses**: Minimum 10 characters required
- **Dates**: No past dates allowed for subscription start
- **Real-time Feedback**: Inline error messages with instant clearing

### User Experience
- **Loading States**: Beautiful `LoadingSpinner` component with multiple size variants
- **Toast Notifications**: Context-based toast system for success/error feedback throughout the app
- **Multi-Step Subscription Flow**: 
  1. Select Plan
  2. Choose Juice (skipped for variety plans)
  3. Enter Delivery Details
  4. Confirm Subscription

### Code Quality Improvements
- ✅ **No Inline Styles**: All CSS extracted to dedicated files (Auth.css, Subscribe.css, etc.)
- ✅ **PropTypes**: Type safety added to all components
- ✅ **Organized Structure**: Components organized into `common/`, `layout/`, and `features/`
- ✅ **Clean Codebase**: Removed unused cart functionality and localStorage fallbacks

## 🏗️ Architecture

### Context System
```jsx
// AuthContext: Manages Supabase authentication state
const { user, signIn, signOut, isAdmin } = useAuth()

// AppContext: Application state with auth integration
const { user, isAdmin } = useAuth()
const { subscriptions } = useApp() // Auto-filtered by role
```

### Database Architecture
- Subscriptions automatically filtered based on user role
- Admins see all subscriptions
- Customers see only their own subscriptions
- RLS policies enforce data isolation at the database level

## 🛠️ Tech Stack

- **Frontend**: React 18 with Vite
- **Routing**: React Router v7
- **Backend**: Supabase (Auth + PostgreSQL)
- **Icons**: Lucide React
- **Date Handling**: date-fns
- **Styling**: CSS Modules

## 📦 Installation

1. Clone the repository:
```bash
git clone https://github.com/aks-heev/juice-subs.git
cd juice-subs
```

2. Install dependencies:
```bash
npm install
```

3. Set up environment variables:
```bash
cp .env.example .env
```
Edit `.env` and add your Supabase credentials:
```
VITE_SUPABASE_URL=your_supabase_url
VITE_SUPABASE_ANON_KEY=your_supabase_anon_key
```

4. Set up the database:
```bash
# Run the SQL commands in supabase_setup.sql in your Supabase SQL editor
```

5. Start the development server:
```bash
npm run dev
```

## 🗂️ Project Structure

```
src/
├── components/
│   ├── common/          # Reusable components (Button, Input, Toast, LoadingSpinner)
│   ├── layout/          # Layout components (Navbar, Footer)
│   └── features/        # Feature-specific components (JuiceCard, SubscriptionCard)
├── pages/               # Page components (Home, Dashboard, Admin, Subscribe, Auth)
├── context/             # React Context providers (AuthContext, AppContext, ToastContext)
├── lib/                 # Utilities and configurations (supabase.js)
├── styles/              # CSS files
└── utils/               # Helper functions (validation.js)
```

## 🔒 Security Features

### Before (Critical Issues)
```jsx
// ❌ localStorage pseudo-auth
const user = JSON.parse(localStorage.getItem('juice_user'))

// ❌ Unprotected routes - anyone could access admin panel
// ❌ No validation - any data accepted
// ❌ All subscriptions fetched when no user logged in
```

### After (Secure Implementation)
```jsx
// ✅ Real Supabase authentication
const { data: { user } } = await supabase.auth.getUser()

// ✅ Protected routes with role checks
<Route path="/admin" element={
  <ProtectedRoute requireAdmin>
    <Admin />
  </ProtectedRoute>
} />

// ✅ Comprehensive validation on all forms
// ✅ RLS policies enforcing data access at database level
```

## 🎯 Key Improvements from Previous Version

| Category | Before | After |
|----------|--------|-------|
| Authentication | localStorage only | Supabase Auth with sessions |
| Admin Access | Open to everyone | Role-based with proper checks |
| Form Validation | None | Comprehensive with real-time feedback |
| Error Handling | Console logs only | User-facing toast notifications |
| Loading States | None | Spinners throughout |
| Code Organization | Inline styles, mixed concerns | Modular CSS, clean architecture |
| Type Safety | None | PropTypes on all components |
| Data Security | All data exposed | RLS policies + user isolation |

## 👥 User Roles

### Customer
- View personal dashboard
- Browse juice offerings
- Create subscriptions
- Manage delivery details
- View own subscription history

### Admin
- Access admin panel
- View all subscriptions across users
- Manage subscription status
- View analytics and reports

## 📝 License

MIT License - feel free to use this project for learning or commercial purposes.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

---

Built with ❤️ using React and Supabase