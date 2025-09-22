# Streem Frontend - Digital Work Instructions Platform

[![Version](https://img.shields.io/badge/version-4.9.11-blue.svg)](https://github.com/leucinetech/streem-frontend)
[![React](https://img.shields.io/badge/React-17.0.2-blue.svg)](https://reactjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-4.3.5-blue.svg)](https://www.typescriptlang.org/)
[![Material-UI](https://img.shields.io/badge/Material--UI-4.12.4-blue.svg)](https://v4.mui.com/)
[![Redux](https://img.shields.io/badge/Redux-4.1.0-purple.svg)](https://redux.js.org/)

## Overview

**Streem Frontend** is the interactive user interface for the **Digital Work Instructions (DWI)** platform, providing a modern React-based web application for manufacturing workflow execution, quality control, and process management. Built with TypeScript, Material-UI, and Redux, it delivers a responsive and intuitive experience for operators, supervisors, and administrators in industrial environments.

### Key Features
- **🎯 Interactive Workflow Execution** - Step-by-step digital work instructions with real-time guidance
- **📊 Real-time Data Collection** - Parameter capture with validations, media uploads, and peer verification
- **📱 Responsive Design** - Optimized for tablets, mobile devices, and desktop environments
- **🔒 Role-based Access Control** - Granular permissions for different user types and facilities
- **📈 Process Analytics** - Real-time dashboards and reporting for operational insights
- **🔄 Offline Capability** - Progressive Web App (PWA) features for unreliable network environments

### Business Impact
- **Paperless Operations** - Complete elimination of paper-based SOPs and checklists
- **Real-time Visibility** - Live tracking of job progress and parameter compliance
- **Quality Assurance** - Built-in verification workflows and exception handling
- **Audit Readiness** - Comprehensive audit trails for regulatory compliance (FDA 21 CFR Part 11, SOX, GMP)
- **Mobile-first Design** - Optimized for shop floor tablets and mobile devices

## Architecture Overview

### Frontend Architecture
```
┌─────────────────────┐
│   Presentation      │  React Components, Material-UI
│      Layer          │  (src/components/, src/views/)
├─────────────────────┤
│   State Management  │  Redux Store, Sagas, Actions
│      Layer          │  (src/store/, src/modules/)
├─────────────────────┤
│   Service Layer     │  API Services, HTTP Client
│                     │  (src/services/)
├─────────────────────┤
│   Utility Layer     │  Helpers, Constants, Types
│                     │  (src/utils/, src/types/)
├─────────────────────┤
│   Asset Layer       │  Styles, Images, Fonts
│                     │  (src/assets/, src/styles/)
└─────────────────────┘
```

### Component Structure
```
src/
├── components/          # Reusable UI components
│   ├── common/         # Generic components (buttons, inputs, modals)
│   ├── forms/          # Form components and field wrappers
│   ├── layout/         # Layout components (header, sidebar, navigation)
│   └── data/           # Data display components (tables, charts)
├── views/              # Page-level components
│   ├── auth/           # Authentication pages
│   ├── jobs/           # Job execution interface
│   ├── checklists/     # Process template management
│   ├── parameters/     # Parameter definition and collection
│   ├── reports/        # Analytics and reporting
│   └── settings/       # Configuration and user management
├── store/              # Redux state management
│   ├── auth/           # Authentication state
│   ├── jobs/           # Job execution state
│   ├── checklists/     # Process template state
│   ├── parameters/     # Parameter collection state
│   └── common/         # Shared application state
├── services/           # API communication layer
│   ├── api/            # HTTP client configuration
│   ├── auth/           # Authentication services
│   ├── jobs/           # Job-related API calls
│   └── common/         # Shared service utilities
├── types/              # TypeScript type definitions
│   ├── api/            # API response types
│   ├── common/         # Shared types and interfaces
│   └── store/          # Redux state types
├── utils/              # Utility functions and helpers
│   ├── constants/      # Application constants
│   ├── helpers/        # Common helper functions
│   └── formatters/     # Data formatting utilities
├── assets/             # Static assets
│   ├── images/         # Images and icons
│   ├── styles/         # Global styles and themes
│   └── fonts/          # Typography assets
└── i18n/               # Internationalization
    ├── locales/        # Translation files
    └── resources/      # i18n configuration
```

## Technology Stack

### Core Framework
- **React 17.0.2** - Modern React with hooks and functional components
- **TypeScript 4.3.5** - Type-safe development with enhanced IDE support
- **Material-UI 4.12.4** - Comprehensive React component library
- **Webpack 5** - Module bundling and build optimization

### State Management
- **Redux 4.1.0** - Predictable state container for JavaScript apps
- **Redux Saga 1.1.3** - Redux middleware for handling side effects
- **React Redux 7.2.4** - React bindings for Redux
- **Redux Toolkit** - Modern Redux with simplified setup

### Form Management & Validation
- **React Hook Form 7.12.2** - Performant forms with easy validation
- **Yup** - Schema-based form validation
- **Custom Validators** - Business-specific validation rules

### UI & Styling
- **Material-UI (MUI v4)** - Google's Material Design components
- **Emotion** - CSS-in-JS library for styling
- **React Spring** - Animation library for smooth interactions
- **Responsive Design** - Mobile-first approach with breakpoint system

### Data Visualization & Reports
- **React PDF 2.3.0** - PDF generation for reports and documentation
- **Recharts** - Composable charting library for React
- **D3.js** - Data visualization for complex charts and graphs

### Development & Build Tools
- **Webpack 5** - Module bundler with optimized build pipeline
- **Babel** - JavaScript transpilation for modern syntax support
- **ESLint** - Code linting and quality enforcement
- **Prettier** - Code formatting for consistent style
- **Jest** - Testing framework for unit and integration tests
- **React Testing Library** - Testing utilities for React components

### Progressive Web App (PWA)
- **Service Workers** - Offline functionality and caching
- **Web App Manifest** - Install prompt and app-like experience
- **IndexedDB** - Client-side storage for offline data

### Internationalization
- **React i18next** - Internationalization framework
- **ICU Message Format** - Advanced message formatting
- **Dynamic Language Loading** - Runtime language switching

## Quick Start

### Prerequisites
- **Node.js 16+** (LTS recommended)
- **npm 8+ or Yarn 1.22+**
- **Git** for version control
- **Backend API** running on `http://localhost:8080` (see backend README)

### Installation

```bash
# Clone the repository
git clone <repository-url>
cd streem-frontend-platform-develop

# Install dependencies
npm install
# or
yarn install

# Copy environment configuration
cp .env.example .env.local

# Update environment variables
# REACT_APP_API_BASE_URL=http://localhost:8080/v1
# REACT_APP_AUTH_SERVICE_URL=http://localhost:8080/auth
```

### Environment Configuration

Create `.env.local` file in the project root:

```bash
# API Configuration
REACT_APP_API_BASE_URL=http://localhost:8080/v1
REACT_APP_AUTH_SERVICE_URL=http://localhost:8080/auth

# Application Configuration
REACT_APP_APP_NAME=Streem DWI
REACT_APP_VERSION=4.9.11
REACT_APP_ENVIRONMENT=development

# Feature Flags
REACT_APP_ENABLE_PWA=true
REACT_APP_ENABLE_OFFLINE_MODE=true
REACT_APP_ENABLE_ANALYTICS=false

# External Services
REACT_APP_AWS_S3_BUCKET=your-s3-bucket
REACT_APP_AWS_REGION=us-east-1

# Development Options
REACT_APP_DEBUG_MODE=true
REACT_APP_MOCK_API=false
```

### Development Server

```bash
# Start development server
npm start
# or
yarn start

# Application will be available at http://localhost:3000
# Hot reload enabled for development
```

### Building for Production

```bash
# Create optimized production build
npm run build
# or
yarn build

# Build output will be in the 'build' folder
# Static files ready for deployment
```

### Running Tests

```bash
# Run all tests
npm test
# or
yarn test

# Run tests with coverage
npm run test:coverage

# Run tests in watch mode
npm run test:watch

# Run specific test file
npm test -- --testNamePattern="JobExecution"
```

## Development Workflow

### Project Structure Navigation

#### Core Application Files
```bash
src/App.tsx                    # Main application component
src/index.tsx                  # Application entry point
src/AppVersionCheck.tsx        # Version checking and update prompts
```

#### Component Development
```bash
# Create new component
src/components/common/NewComponent.tsx
src/components/common/NewComponent.test.tsx
src/components/common/NewComponent.stories.tsx
```

#### State Management
```bash
# Redux store structure
src/store/index.ts             # Store configuration
src/store/rootReducer.ts       # Root reducer
src/store/rootSaga.ts          # Root saga

# Feature-specific state
src/store/jobs/types.ts        # Job-related types
src/store/jobs/actions.ts      # Job actions
src/store/jobs/reducer.ts      # Job reducer
src/store/jobs/sagas.ts        # Job sagas
src/store/jobs/selectors.ts    # Job selectors
```

#### API Services
```bash
src/services/api/client.ts     # HTTP client configuration
src/services/jobs/jobsApi.ts   # Job-related API calls
src/services/auth/authApi.ts   # Authentication API calls
```

### Code Style Guidelines

#### TypeScript Best Practices
```typescript
// Use explicit types for props and state
interface JobCardProps {
  job: Job;
  onStartJob: (jobId: string) => void;
  isLoading?: boolean;
}

// Use proper type definitions
type JobStatus = 'NOT_STARTED' | 'IN_PROGRESS' | 'COMPLETED' | 'CANCELLED';

// Use enums for constants
enum ParameterType {
  SHOULD_BE = 'SHOULD_BE',
  CALCULATION = 'CALCULATION',
  INSTRUCTION = 'INSTRUCTION',
  RESOURCE = 'RESOURCE'
}
```

#### Component Structure
```typescript
import React, { useState, useEffect } from 'react';
import { useDispatch, useSelector } from 'react-redux';
import { makeStyles } from '@material-ui/core/styles';

// Types and interfaces
interface ComponentProps {
  // props definition
}

// Styles
const useStyles = makeStyles((theme) => ({
  // component styles
}));

// Component
const ComponentName: React.FC<ComponentProps> = ({ prop1, prop2 }) => {
  const classes = useStyles();
  const dispatch = useDispatch();
  
  // Component logic
  
  return (
    // JSX
  );
};

export default ComponentName;
```

#### Redux Patterns
```typescript
// Actions (using Redux Toolkit)
import { createAction } from '@reduxjs/toolkit';

export const fetchJobs = createAction<{ facilityId: string }>('jobs/fetchJobs');
export const fetchJobsSuccess = createAction<Job[]>('jobs/fetchJobsSuccess');
export const fetchJobsFailure = createAction<string>('jobs/fetchJobsFailure');

// Selectors
export const selectJobs = (state: RootState) => state.jobs.items;
export const selectJobsLoading = (state: RootState) => state.jobs.loading;
export const selectJobById = (jobId: string) => (state: RootState) =>
  state.jobs.items.find(job => job.id === jobId);
```

### Testing Strategy

#### Component Testing
```typescript
import { render, screen, fireEvent } from '@testing-library/react';
import { Provider } from 'react-redux';
import { store } from '../store';
import JobCard from './JobCard';

const renderWithRedux = (component: React.ReactElement) => {
  return render(
    <Provider store={store}>
      {component}
    </Provider>
  );
};

describe('JobCard', () => {
  it('should display job title and status', () => {
    const mockJob = {
      id: '1',
      title: 'Test Job',
      status: 'IN_PROGRESS'
    };
    
    renderWithRedux(<JobCard job={mockJob} onStartJob={jest.fn()} />);
    
    expect(screen.getByText('Test Job')).toBeInTheDocument();
    expect(screen.getByText('IN_PROGRESS')).toBeInTheDocument();
  });
});
```

#### Redux Testing
```typescript
import { configureStore } from '@reduxjs/toolkit';
import jobsReducer from '../store/jobs/reducer';
import { fetchJobsSuccess } from '../store/jobs/actions';

describe('Jobs Reducer', () => {
  it('should handle fetchJobsSuccess', () => {
    const store = configureStore({ reducer: { jobs: jobsReducer } });
    const mockJobs = [{ id: '1', title: 'Test Job' }];
    
    store.dispatch(fetchJobsSuccess(mockJobs));
    
    expect(store.getState().jobs.items).toEqual(mockJobs);
  });
});
```

### Build and Deployment

#### Build Configuration
```bash
# Production build with optimizations
npm run build

# Analyze bundle size
npm run build:analyze

# Build with specific environment
REACT_APP_ENVIRONMENT=staging npm run build
```

#### Docker Deployment

##### Building Docker Image

```bash
# Build with defaults
./docker/script.sh

# Build with custom backend URL
./docker/script.sh -a 'http://localhost:8080/v1'

# Build with environment variables
docker build \
  --build-arg REACT_APP_API_BASE_URL=http://api.production.com/v1 \
  --build-arg REACT_APP_ENVIRONMENT=production \
  -t streem-frontend:latest .
```

##### Running Docker Container

```bash
# Run with environment variables
docker run -p 80:80 \
  -e BACKEND_URL='http://localhost:8080/v1' \
  -e APP_ENVIRONMENT='production' \
  --name streem-frontend \
  leucine.azurecr.io/streem-dwi/frontend:latest

# Run with volume mounting for configuration
docker run -p 80:80 \
  -v ./config:/app/config \
  --name streem-frontend \
  streem-frontend:latest
```

#### Nginx Configuration

The application includes optimized Nginx configuration for production deployment:

```nginx
# nginx.conf (included in Docker image)
server {
    listen 80;
    server_name localhost;
    
    # Gzip compression
    gzip on;
    gzip_types text/plain text/css application/json application/javascript;
    
    # Security headers
    add_header X-Frame-Options DENY;
    add_header X-Content-Type-Options nosniff;
    add_header X-XSS-Protection "1; mode=block";
    
    # Static assets with long cache
    location /static/ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
    
    # API proxy (if needed)
    location /api/ {
        proxy_pass $BACKEND_URL;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
    
    # React app
    location / {
        try_files $uri $uri/ /index.html;
        expires -1;
        add_header Cache-Control "no-cache, no-store, must-revalidate";
    }
}
```

## Feature Development

### Job Execution Workflow

The core feature of the application is the job execution interface:

```typescript
// Job execution component structure
src/views/jobs/
├── JobList.tsx              # List of available jobs
├── JobDetail.tsx            # Job details and task list
├── TaskExecution.tsx        # Individual task execution
├── ParameterCollection.tsx  # Parameter data collection
└── JobCompletion.tsx        # Job completion and summary
```

#### Key Components
- **JobCard** - Job overview with status and actions
- **TaskStepper** - Step-by-step task navigation
- **ParameterInput** - Dynamic parameter collection forms
- **MediaUpload** - File and image upload components
- **VerificationModal** - Peer and self-verification workflows

### Parameter Types and Validation

```typescript
// Parameter type definitions
enum ParameterType {
  SHOULD_BE = 'SHOULD_BE',        // Expected vs actual value comparison
  CALCULATION = 'CALCULATION',     // Mathematical calculations
  INSTRUCTION = 'INSTRUCTION',     // Text instructions and acknowledgments
  RESOURCE = 'RESOURCE',          // File attachments and media
  SINGLE_SELECT = 'SINGLE_SELECT', // Single choice selection
  MULTI_SELECT = 'MULTI_SELECT',  // Multiple choice selection
  CHECKLIST = 'CHECKLIST',        // Sub-checklist execution
  SIGNATURE = 'SIGNATURE',        // Digital signature capture
  DATE_TIME = 'DATE_TIME',        // Date and time input
  NUMBER = 'NUMBER',              // Numeric input with validations
  YES_NO = 'YES_NO'               // Boolean confirmation
}

// Validation rules
interface ParameterValidation {
  required: boolean;
  min?: number;
  max?: number;
  pattern?: string;
  customValidator?: (value: any) => boolean | string;
}
```

### State Management Patterns

#### Redux Store Structure
```typescript
interface RootState {
  auth: AuthState;           // User authentication and permissions
  jobs: JobsState;           // Job execution state
  tasks: TasksState;         // Task-level state management
  parameters: ParametersState; // Parameter collection state
  facilities: FacilitiesState; // Multi-tenant facility data
  users: UsersState;         // User management
  checklists: ChecklistsState; // Process template state
  ui: UIState;               // UI state (modals, loading, etc.)
}
```

#### Async Actions with Redux Saga
```typescript
// Job execution saga
function* executeJobSaga(action: PayloadAction<{ jobId: string }>) {
  try {
    yield put(setJobLoading(true));
    const job: Job = yield call(jobsApi.getJob, action.payload.jobId);
    const tasks: Task[] = yield call(jobsApi.getJobTasks, action.payload.jobId);
    
    yield put(setCurrentJob(job));
    yield put(setJobTasks(tasks));
    yield put(setJobLoading(false));
  } catch (error) {
    yield put(setJobError(error.message));
    yield put(setJobLoading(false));
  }
}
```

## API Integration

### HTTP Client Configuration

```typescript
// API client setup
import axios from 'axios';

const apiClient = axios.create({
  baseURL: process.env.REACT_APP_API_BASE_URL,
  timeout: 30000,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Request interceptor for authentication
apiClient.interceptors.request.use((config) => {
  const token = localStorage.getItem('authToken');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// Response interceptor for error handling
apiClient.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      // Handle authentication error
      store.dispatch(logout());
    }
    return Promise.reject(error);
  }
);
```

### API Service Layer

```typescript
// Jobs API service
export const jobsApi = {
  getJobs: (facilityId: string): Promise<Job[]> =>
    apiClient.get(`/jobs?facilityId=${facilityId}`).then(res => res.data),
    
  getJob: (jobId: string): Promise<Job> =>
    apiClient.get(`/jobs/${jobId}`).then(res => res.data),
    
  startJob: (jobId: string): Promise<Job> =>
    apiClient.patch(`/jobs/${jobId}/start`).then(res => res.data),
    
  completeJob: (jobId: string, data: JobCompletionData): Promise<Job> =>
    apiClient.patch(`/jobs/${jobId}/complete`, data).then(res => res.data),
    
  submitParameter: (taskId: string, parameterId: string, value: any): Promise<void> =>
    apiClient.post(`/tasks/${taskId}/parameters/${parameterId}`, { value }).then(res => res.data),
};
```

## Performance Optimization

### Code Splitting and Lazy Loading

```typescript
// Route-based code splitting
import { lazy, Suspense } from 'react';

const JobsView = lazy(() => import('../views/jobs/JobsView'));
const ChecklistsView = lazy(() => import('../views/checklists/ChecklistsView'));
const ReportsView = lazy(() => import('../views/reports/ReportsView'));

// Suspense wrapper for lazy components
const AppRoutes = () => (
  <Suspense fallback={<LoadingSpinner />}>
    <Switch>
      <Route path="/jobs" component={JobsView} />
      <Route path="/checklists" component={ChecklistsView} />
      <Route path="/reports" component={ReportsView} />
    </Switch>
  </Suspense>
);
```

### Component Optimization

```typescript
// Memoized components for performance
import React, { memo, useMemo, useCallback } from 'react';

const JobCard = memo<JobCardProps>(({ job, onStartJob }) => {
  const handleStartJob = useCallback(() => {
    onStartJob(job.id);
  }, [job.id, onStartJob]);
  
  const jobStatusColor = useMemo(() => {
    return getStatusColor(job.status);
  }, [job.status]);
  
  return (
    // Component JSX
  );
});
```

### Bundle Optimization

```javascript
// webpack.config.js optimizations
module.exports = {
  optimization: {
    splitChunks: {
      chunks: 'all',
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          priority: 10,
        },
        mui: {
          test: /[\\/]node_modules[\\/]@material-ui[\\/]/,
          name: 'mui',
          priority: 20,
        },
      },
    },
  },
};
```

## Progressive Web App (PWA)

### Service Worker Configuration

```typescript
// Service worker for offline functionality
const CACHE_NAME = 'streem-dwi-v1';
const urlsToCache = [
  '/',
  '/static/js/bundle.js',
  '/static/css/main.css',
  '/manifest.json',
];

self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then((cache) => cache.addAll(urlsToCache))
  );
});

self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request)
      .then((response) => {
        // Return cached version or fetch from network
        return response || fetch(event.request);
      })
  );
});
```

### Offline Data Management

```typescript
// IndexedDB for offline data storage
import { openDB } from 'idb';

const dbPromise = openDB('streem-offline', 1, {
  upgrade(db) {
    db.createObjectStore('jobs');
    db.createObjectStore('parameters');
    db.createObjectStore('media');
  },
});

export const offlineStorage = {
  async setJob(jobId: string, job: Job) {
    const db = await dbPromise;
    await db.put('jobs', job, jobId);
  },
  
  async getJob(jobId: string): Promise<Job | undefined> {
    const db = await dbPromise;
    return await db.get('jobs', jobId);
  },
};
```

## Accessibility (a11y)

### WCAG 2.1 Compliance

```typescript
// Accessibility best practices
const AccessibleButton = ({ children, onClick, ...props }) => (
  <Button
    onClick={onClick}
    aria-label={props['aria-label']}
    role="button"
    tabIndex={0}
    onKeyPress={(e) => {
      if (e.key === 'Enter' || e.key === ' ') {
        onClick();
      }
    }}
    {...props}
  >
    {children}
  </Button>
);

// Screen reader support
const JobStatus = ({ status }) => (
  <div role="status" aria-live="polite">
    <span className="sr-only">Job status: </span>
    {status}
  </div>
);
```

### Focus Management

```typescript
// Focus management for modals and forms
import { useEffect, useRef } from 'react';

const Modal = ({ isOpen, onClose, children }) => {
  const modalRef = useRef<HTMLDivElement>(null);
  
  useEffect(() => {
    if (isOpen) {
      modalRef.current?.focus();
    }
  }, [isOpen]);
  
  return isOpen ? (
    <div
      ref={modalRef}
      role="dialog"
      aria-modal="true"
      tabIndex={-1}
      onKeyDown={(e) => {
        if (e.key === 'Escape') {
          onClose();
        }
      }}
    >
      {children}
    </div>
  ) : null;
};
```

## Technical Implementation Notes

### Material-UI v4 Specific Considerations

```typescript
// MUI v4 theme configuration
import { createMuiTheme } from '@material-ui/core/styles';

const theme = createMuiTheme({
  palette: {
    primary: {
      main: '#1976d2',
    },
    secondary: {
      main: '#dc004e',
    },
  },
  typography: {
    fontFamily: 'Inter, Arial, sans-serif',
  },
  zIndex: {
    // MUI default z-index = 1300
    modal: 1300,
    snackbar: 1400,
    tooltip: 1500,
  },
});
```

**Important Notes:**
- **MUI v4 z-index**: Default is 1300 - ensure custom components respect this hierarchy
- **React PDF Considerations**: Always use ternary operators when checking array lengths
  ```typescript
  // Correct for React PDF
  {items.length > 0 ? items.map(item => <Text key={item.id}>{item.name}</Text>) : null}
  
  // Incorrect - React PDF will render "0"
  {items.length && items.map(item => <Text key={item.id}>{item.name}</Text>)}
  ```

### Font Management

#### Production Fonts (.ttf for PDF Support)
- **Primary Font**: [Inter](https://fonts.google.com/specimen/Inter) - Use .ttf files for PDF compatibility
- **Location**: `src/assets/fonts/`
- **Configuration**: Imported in `src/styles/fonts.css`

```css
/* Font declarations for PDF support */
@font-face {
  font-family: 'Inter';
  src: url('../assets/fonts/Inter-Regular.ttf') format('truetype');
  font-weight: 400;
  font-style: normal;
}

@font-face {
  font-family: 'Inter';
  src: url('../assets/fonts/Inter-Bold.ttf') format('truetype');
  font-weight: 700;
  font-style: normal;
}
```

**TODO**: Replace Google Fonts with local .ttf files for PDF generation compatibility.

## Documentation Reference

**Important**: Always refer to the documentation version specified in `package.json`

### Core Dependencies Documentation
- **[Material-UI v4](https://v4.mui.com/)** - Component library and theming
- **[React Hook Form](https://react-hook-form.com/)** - Form management and validation
- **[React Redux](https://react-redux.js.org/)** - Redux bindings for React
- **[Redux Saga](https://redux-saga.js.org/)** - Redux side effect management
- **[React PDF](https://react-pdf.org/)** - PDF generation for reports

### Additional Resources
- **[TypeScript Handbook](https://www.typescriptlang.org/docs/)** - TypeScript best practices
- **[React Testing Library](https://testing-library.com/docs/react-testing-library/intro/)** - Component testing
- **[Webpack Documentation](https://webpack.js.org/concepts/)** - Build configuration
- **[PWA Guide](https://web.dev/progressive-web-apps/)** - Progressive Web App implementation

## Contributing

### Development Setup
1. **Fork the repository** and create a feature branch
2. **Follow TypeScript and ESLint** configuration for code quality
3. **Write comprehensive tests** for new components and features
4. **Update documentation** for component APIs and user guides
5. **Test accessibility** with screen readers and keyboard navigation

### Code Review Checklist
- [ ] TypeScript types properly defined
- [ ] Component properly memoized if needed
- [ ] Accessibility attributes included
- [ ] Tests cover main functionality
- [ ] No console errors or warnings
- [ ] Performance implications considered
- [ ] Mobile responsiveness verified

### Pull Request Process
1. Ensure all tests pass and code coverage is maintained
2. Update Storybook stories for new components
3. Verify PWA functionality if offline features are modified
4. Test with actual backend API integration
5. Update relevant documentation and README sections

## Production Deployment

### Environment-Specific Builds
```bash
# Staging environment
REACT_APP_ENVIRONMENT=staging \
REACT_APP_API_BASE_URL=https://api.staging.com/v1 \
npm run build

# Production environment
REACT_APP_ENVIRONMENT=production \
REACT_APP_API_BASE_URL=https://api.production.com/v1 \
npm run build
```

### Performance Monitoring
- **Bundle Analysis**: Use `npm run build:analyze` to monitor bundle size
- **Core Web Vitals**: Monitor LCP, FID, and CLS metrics
- **Error Tracking**: Integrate with error monitoring services
- **User Analytics**: Track user interactions and feature usage

### Security Considerations
- **Content Security Policy (CSP)** headers configured in nginx
- **XSS Protection** via Material-UI's built-in sanitization
- **HTTPS Enforcement** for all production deployments
- **Authentication Token** secure storage and rotation

## Support & Resources

### Getting Help
- **📖 Component Documentation**: Check Storybook for component APIs
- **🐛 Issue Reporting**: Use GitHub issues with detailed reproduction steps
- **💡 Feature Requests**: Discuss new features via GitHub discussions
- **🔧 Development Support**: Contact the frontend development team

### Useful Development Tools
- **React Developer Tools** - Browser extension for React debugging
- **Redux DevTools** - Browser extension for Redux state inspection
- **Lighthouse** - Performance and accessibility auditing
- **WAVE** - Web accessibility evaluation tool

For technical support and questions, please refer to the project's GitHub repository or contact the development team.

---

## License

Copyright © 2024 Leucine Technologies. All rights reserved.
