# Contributing to Streem Frontend Platform

We welcome contributions to the Streem Frontend Platform! This document provides guidelines for contributing to our React TypeScript application for digital work instruction execution.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [Contribution Workflow](#contribution-workflow)
- [Coding Standards](#coding-standards)
- [Component Development](#component-development)
- [State Management](#state-management)
- [Testing Guidelines](#testing-guidelines)
- [Styling Guidelines](#styling-guidelines)
- [Performance Guidelines](#performance-guidelines)
- [Accessibility Guidelines](#accessibility-guidelines)
- [Pull Request Process](#pull-request-process)
- [Issue Reporting](#issue-reporting)

## Code of Conduct

This project adheres to a code of conduct that ensures a welcoming environment for all contributors. By participating, you are expected to uphold this code.

### Our Standards

- Use welcoming and inclusive language
- Be respectful of differing viewpoints and experiences
- Gracefully accept constructive criticism
- Focus on what is best for the community
- Show empathy towards other community members

## Getting Started

### Prerequisites

Before contributing, ensure you have:

- **Node.js 16+** (LTS recommended)
- **npm 8+ or Yarn 1.22+**
- **Git** for version control
- **Modern Browser** (Chrome, Firefox, Safari, Edge)
- **Code Editor** (VS Code recommended)
- **Backend API** running locally for full development

### Repository Structure

```
streem-frontend-platform/
├── public/                 # Static assets and HTML template
├── src/                    # Source code
│   ├── components/         # Reusable UI components
│   ├── views/             # Page-level components
│   ├── store/             # Redux state management
│   ├── services/          # API services and utilities
│   ├── types/             # TypeScript type definitions
│   ├── utils/             # Helper functions and constants
│   ├── assets/            # Images, styles, fonts
│   ├── i18n/              # Internationalization
│   └── styles/            # Global styles and themes
├── configs/               # Build and development configuration
├── scripts/               # Build and deployment scripts
├── docker/               # Docker configuration
└── test/                 # Test configuration and mocks
```

## Development Setup

### 1. Fork and Clone

```bash
# Fork the repository on GitHub
# Clone your fork
git clone https://github.com/YOUR_USERNAME/streem-frontend-platform.git
cd streem-frontend-platform

# Add upstream remote
git remote add upstream https://github.com/leucinetech/streem-frontend-platform.git
```

### 2. Install Dependencies

```bash
# Install dependencies
npm install
# or
yarn install

# Verify installation
npm run --version
```

### 3. Environment Configuration

```bash
# Copy environment template
cp .env.example .env.local

# Configure environment variables
REACT_APP_API_BASE_URL=http://localhost:8080/v1
REACT_APP_AUTH_SERVICE_URL=http://localhost:8080/auth
REACT_APP_ENVIRONMENT=development
```

### 4. Start Development Server

```bash
# Start development server
npm start
# or
yarn start

# Application available at http://localhost:3000
```

### 5. Verify Setup

```bash
# Run tests
npm test

# Build for production
npm run build

# Check code style
npm run lint
```

## Contribution Workflow

### 1. Create a Feature Branch

```bash
# Sync with upstream
git checkout main
git pull upstream main

# Create feature branch
git checkout -b feature/your-feature-name
```

### 2. Make Changes

- Follow [coding standards](#coding-standards)
- Write comprehensive tests
- Update documentation as needed
- Ensure accessibility compliance
- Test in multiple browsers

### 3. Commit Changes

```bash
# Stage changes
git add .

# Commit with descriptive message
git commit -m "feat(jobs): Add real-time job status updates

- Implement WebSocket connection for live status updates
- Add loading states and error handling
- Include optimistic UI updates
- Add comprehensive test coverage

Fixes #123"
```

### 4. Push and Create Pull Request

```bash
# Push to your fork
git push origin feature/your-feature-name

# Create pull request on GitHub
```

## Coding Standards

### TypeScript Best Practices

#### Type Definitions

```typescript
// Use interfaces for object shapes
interface JobCardProps {
  job: Job;
  onStartJob: (jobId: string) => void;
  onCompleteJob?: (jobId: string) => void;
  isLoading?: boolean;
  className?: string;
}

// Use types for unions and computed types
type JobStatus = 'NOT_STARTED' | 'IN_PROGRESS' | 'COMPLETED' | 'CANCELLED';
type JobActions = 'START' | 'PAUSE' | 'RESUME' | 'COMPLETE' | 'CANCEL';

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
import React, { useState, useEffect, useCallback, useMemo } from 'react';
import { useDispatch, useSelector } from 'react-redux';
import { makeStyles } from '@material-ui/core/styles';
import { Card, CardContent, Typography, Button } from '@material-ui/core';

// Types
interface JobCardProps {
  job: Job;
  onStartJob: (jobId: string) => void;
  isLoading?: boolean;
}

// Styles
const useStyles = makeStyles((theme) => ({
  root: {
    margin: theme.spacing(1),
    padding: theme.spacing(2),
    borderRadius: theme.shape.borderRadius,
  },
  title: {
    fontSize: '1.2rem',
    fontWeight: 'bold',
    marginBottom: theme.spacing(1),
  },
  actions: {
    display: 'flex',
    justifyContent: 'flex-end',
    gap: theme.spacing(1),
  },
}));

// Component
const JobCard: React.FC<JobCardProps> = ({ 
  job, 
  onStartJob, 
  isLoading = false 
}) => {
  const classes = useStyles();
  const dispatch = useDispatch();
  
  // Local state
  const [localLoading, setLocalLoading] = useState(false);
  
  // Selectors
  const currentUser = useSelector(selectCurrentUser);
  
  // Memoized values
  const canStartJob = useMemo(() => {
    return job.state === 'NOT_STARTED' && 
           currentUser?.permissions.includes('JOB_EXECUTE');
  }, [job.state, currentUser?.permissions]);
  
  // Callbacks
  const handleStartJob = useCallback(async () => {
    try {
      setLocalLoading(true);
      await onStartJob(job.id);
    } catch (error) {
      console.error('Failed to start job:', error);
    } finally {
      setLocalLoading(false);
    }
  }, [job.id, onStartJob]);
  
  // Effects
  useEffect(() => {
    // Component lifecycle logic
  }, []);
  
  return (
    <Card className={classes.root}>
      <CardContent>
        <Typography className={classes.title}>
          {job.title}
        </Typography>
        
        <Typography variant="body2" color="textSecondary">
          Status: {job.state}
        </Typography>
        
        <div className={classes.actions}>
          <Button
            variant="contained"
            color="primary"
            disabled={!canStartJob || isLoading || localLoading}
            onClick={handleStartJob}
          >
            {(isLoading || localLoading) ? 'Starting...' : 'Start Job'}
          </Button>
        </div>
      </CardContent>
    </Card>
  );
};

export default JobCard;
```

### Naming Conventions

```typescript
// Components: PascalCase
const JobExecutionView: React.FC = () => { };
const ParameterInput: React.FC<ParameterInputProps> = () => { };

// Files: PascalCase for components, camelCase for utilities
JobExecutionView.tsx
ParameterInput.tsx
apiClient.ts
formatUtils.ts

// Variables and functions: camelCase
const isJobRunning = true;
const handleJobStart = () => { };

// Constants: UPPER_SNAKE_CASE
const API_BASE_URL = process.env.REACT_APP_API_BASE_URL;
const MAX_RETRY_ATTEMPTS = 3;

// CSS classes: kebab-case
const useStyles = makeStyles({
  jobCard: { },
  jobCardTitle: { },
  'job-card-actions': { }
});
```

### File Organization

```typescript
// Component file structure
src/components/jobs/
├── JobCard/
│   ├── index.ts                 # Export barrel
│   ├── JobCard.tsx             # Component implementation
│   ├── JobCard.test.tsx        # Unit tests
│   ├── JobCard.stories.tsx     # Storybook stories
│   └── JobCard.styles.ts       # Styled components (if used)
└── JobList/
    ├── index.ts
    ├── JobList.tsx
    ├── JobList.test.tsx
    └── JobList.stories.tsx
```

### Import Order

```typescript
// 1. React and third-party libraries
import React, { useState, useEffect } from 'react';
import { useDispatch, useSelector } from 'react-redux';
import { makeStyles } from '@material-ui/core/styles';
import { Card, CardContent, Typography } from '@material-ui/core';

// 2. Internal utilities and services
import { apiClient } from '../../services/api';
import { formatDate } from '../../utils/formatters';

// 3. Types
import type { Job, JobStatus } from '../../types/job';
import type { RootState } from '../../store/types';

// 4. Relative imports
import './JobCard.css';
```

## Component Development

### Component Types

#### Functional Components with Hooks

```typescript
// Simple presentational component
const StatusBadge: React.FC<{ status: JobStatus }> = ({ status }) => {
  const getStatusColor = (status: JobStatus) => {
    switch (status) {
      case 'IN_PROGRESS': return 'primary';
      case 'COMPLETED': return 'success';
      case 'CANCELLED': return 'error';
      default: return 'default';
    }
  };
  
  return (
    <Chip 
      label={status} 
      color={getStatusColor(status)}
      size="small"
    />
  );
};

// Complex container component
const JobExecutionView: React.FC = () => {
  const dispatch = useDispatch();
  const { jobId } = useParams<{ jobId: string }>();
  
  // State
  const [currentTask, setCurrentTask] = useState<Task | null>(null);
  
  // Selectors
  const job = useSelector((state: RootState) => 
    selectJobById(state, jobId!)
  );
  const tasks = useSelector((state: RootState) => 
    selectTasksByJobId(state, jobId!)
  );
  
  // Effects
  useEffect(() => {
    if (jobId) {
      dispatch(fetchJob(jobId));
      dispatch(fetchJobTasks(jobId));
    }
  }, [dispatch, jobId]);
  
  // Event handlers
  const handleTaskComplete = useCallback((taskId: string) => {
    dispatch(completeTask({ jobId: jobId!, taskId }));
  }, [dispatch, jobId]);
  
  if (!job) {
    return <LoadingSpinner />;
  }
  
  return (
    <div>
      {/* Component JSX */}
    </div>
  );
};
```

### Component Patterns

#### Compound Components

```typescript
// Compound component pattern for complex UI
const JobExecutionStepper = {
  Root: ({ children, currentStep }: StepperRootProps) => (
    <div className="stepper-root">
      {React.Children.map(children, (child, index) =>
        React.cloneElement(child, { isActive: index === currentStep })
      )}
    </div>
  ),
  
  Step: ({ children, isActive, title }: StepProps) => (
    <div className={`step ${isActive ? 'active' : ''}`}>
      <h3>{title}</h3>
      {children}
    </div>
  ),
  
  Content: ({ children }: { children: React.ReactNode }) => (
    <div className="step-content">
      {children}
    </div>
  )
};

// Usage
<JobExecutionStepper.Root currentStep={currentStep}>
  <JobExecutionStepper.Step title="Task 1">
    <JobExecutionStepper.Content>
      <TaskExecution task={tasks[0]} />
    </JobExecutionStepper.Content>
  </JobExecutionStepper.Step>
</JobExecutionStepper.Root>
```

#### Render Props

```typescript
// Render props for data fetching
interface DataFetcherProps<T> {
  url: string;
  children: (data: {
    data: T | null;
    loading: boolean;
    error: string | null;
    retry: () => void;
  }) => React.ReactNode;
}

const DataFetcher = <T,>({ url, children }: DataFetcherProps<T>) => {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);
  
  const fetchData = useCallback(async () => {
    try {
      setLoading(true);
      setError(null);
      const response = await apiClient.get<T>(url);
      setData(response.data);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Unknown error');
    } finally {
      setLoading(false);
    }
  }, [url]);
  
  useEffect(() => {
    fetchData();
  }, [fetchData]);
  
  return (
    <>
      {children({ data, loading, error, retry: fetchData })}
    </>
  );
};

// Usage
<DataFetcher<Job> url={`/jobs/${jobId}`}>
  {({ data: job, loading, error, retry }) => {
    if (loading) return <LoadingSpinner />;
    if (error) return <ErrorMessage error={error} onRetry={retry} />;
    if (!job) return <NotFound />;
    return <JobDetails job={job} />;
  }}
</DataFetcher>
```

### Custom Hooks

```typescript
// Custom hook for job management
const useJob = (jobId: string) => {
  const dispatch = useDispatch();
  const job = useSelector((state: RootState) => selectJobById(state, jobId));
  const loading = useSelector(selectJobLoading);
  const error = useSelector(selectJobError);
  
  const startJob = useCallback(() => {
    dispatch(startJobAsync(jobId));
  }, [dispatch, jobId]);
  
  const completeJob = useCallback(() => {
    dispatch(completeJobAsync(jobId));
  }, [dispatch, jobId]);
  
  useEffect(() => {
    if (jobId && !job) {
      dispatch(fetchJob(jobId));
    }
  }, [dispatch, jobId, job]);
  
  return {
    job,
    loading,
    error,
    actions: {
      start: startJob,
      complete: completeJob
    }
  };
};

// Custom hook for form management
const useParameterForm = (parameter: Parameter) => {
  const [value, setValue] = useState(parameter.defaultValue || '');
  const [errors, setErrors] = useState<string[]>([]);
  const [touched, setTouched] = useState(false);
  
  const validate = useCallback((newValue: any) => {
    const validationErrors: string[] = [];
    
    if (parameter.required && !newValue) {
      validationErrors.push('This field is required');
    }
    
    if (parameter.validations) {
      // Custom validation logic
    }
    
    setErrors(validationErrors);
    return validationErrors.length === 0;
  }, [parameter]);
  
  const handleChange = useCallback((newValue: any) => {
    setValue(newValue);
    if (touched) {
      validate(newValue);
    }
  }, [touched, validate]);
  
  const handleBlur = useCallback(() => {
    setTouched(true);
    validate(value);
  }, [value, validate]);
  
  return {
    value,
    errors,
    touched,
    isValid: errors.length === 0,
    onChange: handleChange,
    onBlur: handleBlur,
    validate: () => validate(value)
  };
};
```

## State Management

### Redux Structure

```typescript
// Store configuration
import { configureStore } from '@reduxjs/toolkit';
import createSagaMiddleware from 'redux-saga';
import { persistStore, persistReducer } from 'redux-persist';
import storage from 'redux-persist/lib/storage';

import rootReducer from './rootReducer';
import rootSaga from './rootSaga';

const persistConfig = {
  key: 'root',
  storage,
  whitelist: ['auth', 'settings'], // Only persist these reducers
};

const persistedReducer = persistReducer(persistConfig, rootReducer);
const sagaMiddleware = createSagaMiddleware();

export const store = configureStore({
  reducer: persistedReducer,
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware({
      serializableCheck: {
        ignoredActions: ['persist/PERSIST', 'persist/REHYDRATE'],
      },
    }).concat(sagaMiddleware),
  devTools: process.env.NODE_ENV !== 'production',
});

sagaMiddleware.run(rootSaga);

export const persistor = persistStore(store);
export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

### Actions and Reducers

```typescript
// actions/jobs.ts
import { createAction } from '@reduxjs/toolkit';

export const fetchJobs = createAction<{ facilityId: string }>('jobs/fetchJobs');
export const fetchJobsSuccess = createAction<Job[]>('jobs/fetchJobsSuccess');
export const fetchJobsFailure = createAction<string>('jobs/fetchJobsFailure');

export const startJob = createAction<string>('jobs/startJob');
export const startJobSuccess = createAction<Job>('jobs/startJobSuccess');
export const startJobFailure = createAction<{ jobId: string; error: string }>('jobs/startJobFailure');

// reducers/jobs.ts
import { createReducer } from '@reduxjs/toolkit';
import * as actions from '../actions/jobs';

interface JobsState {
  items: Job[];
  loading: boolean;
  error: string | null;
  currentJob: Job | null;
}

const initialState: JobsState = {
  items: [],
  loading: false,
  error: null,
  currentJob: null,
};

export const jobsReducer = createReducer(initialState, (builder) => {
  builder
    .addCase(actions.fetchJobs, (state) => {
      state.loading = true;
      state.error = null;
    })
    .addCase(actions.fetchJobsSuccess, (state, action) => {
      state.loading = false;
      state.items = action.payload;
    })
    .addCase(actions.fetchJobsFailure, (state, action) => {
      state.loading = false;
      state.error = action.payload;
    })
    .addCase(actions.startJobSuccess, (state, action) => {
      const jobIndex = state.items.findIndex(job => job.id === action.payload.id);
      if (jobIndex !== -1) {
        state.items[jobIndex] = action.payload;
      }
      state.currentJob = action.payload;
    });
});
```

### Sagas

```typescript
// sagas/jobs.ts
import { call, put, takeEvery, takeLatest, select } from 'redux-saga/effects';
import { PayloadAction } from '@reduxjs/toolkit';
import * as actions from '../actions/jobs';
import * as api from '../services/api/jobs';

function* fetchJobsSaga(action: PayloadAction<{ facilityId: string }>) {
  try {
    const jobs: Job[] = yield call(api.getJobs, action.payload.facilityId);
    yield put(actions.fetchJobsSuccess(jobs));
  } catch (error) {
    yield put(actions.fetchJobsFailure(error.message));
  }
}

function* startJobSaga(action: PayloadAction<string>) {
  try {
    const jobId = action.payload;
    const job: Job = yield call(api.startJob, jobId);
    yield put(actions.startJobSuccess(job));
    
    // Navigate to job execution page
    // yield put(push(`/jobs/${jobId}/execute`));
  } catch (error) {
    yield put(actions.startJobFailure({ 
      jobId: action.payload, 
      error: error.message 
    }));
  }
}

export function* jobsSaga() {
  yield takeLatest(actions.fetchJobs.type, fetchJobsSaga);
  yield takeEvery(actions.startJob.type, startJobSaga);
}
```

### Selectors

```typescript
// selectors/jobs.ts
import { createSelector } from '@reduxjs/toolkit';
import { RootState } from '../store';

// Basic selectors
export const selectJobsState = (state: RootState) => state.jobs;
export const selectJobs = (state: RootState) => state.jobs.items;
export const selectJobsLoading = (state: RootState) => state.jobs.loading;
export const selectJobsError = (state: RootState) => state.jobs.error;
export const selectCurrentJob = (state: RootState) => state.jobs.currentJob;

// Memoized selectors
export const selectJobById = createSelector(
  [selectJobs, (state: RootState, jobId: string) => jobId],
  (jobs, jobId) => jobs.find(job => job.id === jobId)
);

export const selectJobsByStatus = createSelector(
  [selectJobs, (state: RootState, status: JobStatus) => status],
  (jobs, status) => jobs.filter(job => job.state === status)
);

export const selectJobsStats = createSelector(
  [selectJobs],
  (jobs) => ({
    total: jobs.length,
    notStarted: jobs.filter(job => job.state === 'NOT_STARTED').length,
    inProgress: jobs.filter(job => job.state === 'IN_PROGRESS').length,
    completed: jobs.filter(job => job.state === 'COMPLETED').length,
  })
);
```

## Testing Guidelines

### Test Structure

```
src/
├── components/
│   └── JobCard/
│       ├── JobCard.tsx
│       ├── JobCard.test.tsx
│       └── JobCard.stories.tsx
├── services/
│   └── api/
│       ├── jobsApi.ts
│       └── jobsApi.test.ts
├── store/
│   └── jobs/
│       ├── reducer.test.ts
│       ├── sagas.test.ts
│       └── selectors.test.ts
└── utils/
    ├── formatters.ts
    └── formatters.test.ts
```

### Component Testing

```typescript
// JobCard.test.tsx
import React from 'react';
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import { Provider } from 'react-redux';
import { ThemeProvider } from '@material-ui/core/styles';
import { configureStore } from '@reduxjs/toolkit';

import JobCard from './JobCard';
import { jobsReducer } from '../../store/jobs/reducer';
import { createTheme } from '../../styles/theme';

// Test utilities
const createTestStore = (initialState = {}) => {
  return configureStore({
    reducer: { jobs: jobsReducer },
    preloadedState: initialState,
  });
};

const renderWithProviders = (
  component: React.ReactElement,
  { initialState = {}, store = createTestStore(initialState) } = {}
) => {
  const theme = createTheme();
  
  return render(
    <Provider store={store}>
      <ThemeProvider theme={theme}>
        {component}
      </ThemeProvider>
    </Provider>
  );
};

// Mock data
const mockJob: Job = {
  id: 'job-1',
  title: 'Test Job',
  state: 'NOT_STARTED',
  checklistId: 'checklist-1',
  facilityId: 'facility-1',
  createdAt: '2025-09-22T10:00:00Z',
};

describe('JobCard', () => {
  const mockOnStartJob = jest.fn();
  
  beforeEach(() => {
    mockOnStartJob.mockClear();
  });
  
  it('should render job information correctly', () => {
    renderWithProviders(
      <JobCard job={mockJob} onStartJob={mockOnStartJob} />
    );
    
    expect(screen.getByText('Test Job')).toBeInTheDocument();
    expect(screen.getByText('Status: NOT_STARTED')).toBeInTheDocument();
    expect(screen.getByRole('button', { name: /start job/i })).toBeInTheDocument();
  });
  
  it('should call onStartJob when start button is clicked', async () => {
    renderWithProviders(
      <JobCard job={mockJob} onStartJob={mockOnStartJob} />
    );
    
    const startButton = screen.getByRole('button', { name: /start job/i });
    fireEvent.click(startButton);
    
    await waitFor(() => {
      expect(mockOnStartJob).toHaveBeenCalledWith('job-1');
    });
  });
  
  it('should disable start button when loading', () => {
    renderWithProviders(
      <JobCard job={mockJob} onStartJob={mockOnStartJob} isLoading={true} />
    );
    
    const startButton = screen.getByRole('button', { name: /starting/i });
    expect(startButton).toBeDisabled();
  });
  
  it('should not show start button for completed jobs', () => {
    const completedJob = { ...mockJob, state: 'COMPLETED' as JobStatus };
    
    renderWithProviders(
      <JobCard job={completedJob} onStartJob={mockOnStartJob} />
    );
    
    expect(screen.queryByRole('button', { name: /start job/i })).not.toBeInTheDocument();
  });
});
```

### Redux Testing

```typescript
// reducer.test.ts
import { jobsReducer } from './reducer';
import * as actions from './actions';

describe('jobs reducer', () => {
  const initialState = {
    items: [],
    loading: false,
    error: null,
    currentJob: null,
  };
  
  it('should handle fetchJobs action', () => {
    const action = actions.fetchJobs({ facilityId: 'facility-1' });
    const newState = jobsReducer(initialState, action);
    
    expect(newState.loading).toBe(true);
    expect(newState.error).toBe(null);
  });
  
  it('should handle fetchJobsSuccess action', () => {
    const jobs: Job[] = [
      { id: 'job-1', title: 'Job 1', state: 'NOT_STARTED' },
      { id: 'job-2', title: 'Job 2', state: 'IN_PROGRESS' },
    ];
    
    const action = actions.fetchJobsSuccess(jobs);
    const newState = jobsReducer(
      { ...initialState, loading: true },
      action
    );
    
    expect(newState.loading).toBe(false);
    expect(newState.items).toEqual(jobs);
  });
});

// sagas.test.ts
import { runSaga } from 'redux-saga';
import * as api from '../../services/api/jobs';
import { fetchJobsSaga } from './sagas';
import * as actions from './actions';

jest.mock('../../services/api/jobs');

describe('jobs sagas', () => {
  it('should fetch jobs successfully', async () => {
    const mockJobs: Job[] = [
      { id: 'job-1', title: 'Job 1', state: 'NOT_STARTED' },
    ];
    
    (api.getJobs as jest.Mock).mockResolvedValue(mockJobs);
    
    const dispatched: any[] = [];
    await runSaga(
      {
        dispatch: (action) => dispatched.push(action),
        getState: () => ({}),
      },
      fetchJobsSaga,
      actions.fetchJobs({ facilityId: 'facility-1' })
    ).toPromise();
    
    expect(api.getJobs).toHaveBeenCalledWith('facility-1');
    expect(dispatched).toContainEqual(actions.fetchJobsSuccess(mockJobs));
  });
});
```

### API Testing

```typescript
// jobsApi.test.ts
import axios from 'axios';
import MockAdapter from 'axios-mock-adapter';
import { getJobs, startJob } from './jobsApi';

describe('jobs API', () => {
  let mock: MockAdapter;
  
  beforeEach(() => {
    mock = new MockAdapter(axios);
  });
  
  afterEach(() => {
    mock.restore();
  });
  
  it('should fetch jobs successfully', async () => {
    const mockJobs = [
      { id: 'job-1', title: 'Job 1', state: 'NOT_STARTED' },
    ];
    
    mock.onGet('/jobs?facilityId=facility-1').reply(200, mockJobs);
    
    const result = await getJobs('facility-1');
    
    expect(result).toEqual(mockJobs);
  });
  
  it('should handle API errors', async () => {
    mock.onGet('/jobs?facilityId=facility-1').reply(500, {
      message: 'Internal Server Error',
    });
    
    await expect(getJobs('facility-1')).rejects.toThrow('Internal Server Error');
  });
});
```

### Integration Testing

```typescript
// JobExecution.integration.test.tsx
import React from 'react';
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import { rest } from 'msw';
import { setupServer } from 'msw/node';

import JobExecutionView from './JobExecutionView';
import { renderWithProviders } from '../../test/test-utils';

// Mock API server
const server = setupServer(
  rest.get('/api/v1/jobs/:jobId', (req, res, ctx) => {
    return res(
      ctx.json({
        id: req.params.jobId,
        title: 'Integration Test Job',
        state: 'NOT_STARTED',
      })
    );
  }),
  
  rest.patch('/api/v1/jobs/:jobId/start', (req, res, ctx) => {
    return res(
      ctx.json({
        id: req.params.jobId,
        title: 'Integration Test Job',
        state: 'IN_PROGRESS',
      })
    );
  })
);

beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());

describe('Job Execution Integration', () => {
  it('should complete full job execution workflow', async () => {
    renderWithProviders(<JobExecutionView />, {
      initialState: {
        router: { location: { pathname: '/jobs/job-1/execute' } },
      },
    });
    
    // Wait for job to load
    await waitFor(() => {
      expect(screen.getByText('Integration Test Job')).toBeInTheDocument();
    });
    
    // Start job
    fireEvent.click(screen.getByRole('button', { name: /start job/i }));
    
    // Verify job state changed
    await waitFor(() => {
      expect(screen.getByText('Status: IN_PROGRESS')).toBeInTheDocument();
    });
  });
});
```

## Styling Guidelines

### Material-UI v4 Theming

```typescript
// theme.ts
import { createMuiTheme } from '@material-ui/core/styles';

export const theme = createMuiTheme({
  palette: {
    primary: {
      main: '#1976d2',
      light: '#42a5f5',
      dark: '#1565c0',
    },
    secondary: {
      main: '#dc004e',
      light: '#f06292',
      dark: '#ad1457',
    },
    background: {
      default: '#f5f5f5',
      paper: '#ffffff',
    },
  },
  typography: {
    fontFamily: 'Inter, Arial, sans-serif',
    h1: {
      fontSize: '2.5rem',
      fontWeight: 600,
    },
    h2: {
      fontSize: '2rem',
      fontWeight: 600,
    },
    body1: {
      fontSize: '1rem',
      lineHeight: 1.6,
    },
  },
  spacing: 8, // 8px base spacing unit
  shape: {
    borderRadius: 8,
  },
  zIndex: {
    // MUI v4 default z-index = 1300
    modal: 1300,
    snackbar: 1400,
    tooltip: 1500,
  },
});
```

### Component Styling

```typescript
// Using makeStyles
import { makeStyles } from '@material-ui/core/styles';

const useStyles = makeStyles((theme) => ({
  root: {
    padding: theme.spacing(2),
    margin: theme.spacing(1),
    borderRadius: theme.shape.borderRadius,
    backgroundColor: theme.palette.background.paper,
    boxShadow: theme.shadows[2],
  },
  title: {
    ...theme.typography.h6,
    marginBottom: theme.spacing(1),
    color: theme.palette.primary.main,
  },
  actions: {
    display: 'flex',
    justifyContent: 'flex-end',
    gap: theme.spacing(1),
    marginTop: theme.spacing(2),
  },
  // Responsive styles
  [theme.breakpoints.down('sm')]: {
    root: {
      padding: theme.spacing(1),
    },
    actions: {
      flexDirection: 'column',
    },
  },
}));

// Component usage
const JobCard: React.FC<JobCardProps> = ({ job }) => {
  const classes = useStyles();
  
  return (
    <Card className={classes.root}>
      <Typography className={classes.title}>
        {job.title}
      </Typography>
      <div className={classes.actions}>
        <Button variant="contained" color="primary">
          Start Job
        </Button>
      </div>
    </Card>
  );
};
```

### CSS-in-JS Best Practices

```typescript
// Dynamic styles based on props
const useStyles = makeStyles((theme) => ({
  statusBadge: {
    padding: theme.spacing(0.5, 1),
    borderRadius: theme.shape.borderRadius,
    fontSize: '0.75rem',
    fontWeight: 'bold',
    textTransform: 'uppercase',
  },
  // Dynamic styles function
  statusBadgeColor: (props: { status: JobStatus }) => ({
    backgroundColor: 
      props.status === 'COMPLETED' ? theme.palette.success.main :
      props.status === 'IN_PROGRESS' ? theme.palette.primary.main :
      props.status === 'CANCELLED' ? theme.palette.error.main :
      theme.palette.grey[400],
    color: theme.palette.getContrastText(
      props.status === 'COMPLETED' ? theme.palette.success.main :
      props.status === 'IN_PROGRESS' ? theme.palette.primary.main :
      props.status === 'CANCELLED' ? theme.palette.error.main :
      theme.palette.grey[400]
    ),
  }),
}));

// Component with dynamic styles
const StatusBadge: React.FC<{ status: JobStatus }> = ({ status }) => {
  const classes = useStyles({ status });
  
  return (
    <span className={`${classes.statusBadge} ${classes.statusBadgeColor}`}>
      {status}
    </span>
  );
};
```

### Responsive Design

```typescript
const useStyles = makeStyles((theme) => ({
  container: {
    display: 'grid',
    gridTemplateColumns: 'repeat(auto-fit, minmax(300px, 1fr))',
    gap: theme.spacing(2),
    padding: theme.spacing(2),
    
    // Tablet styles
    [theme.breakpoints.down('md')]: {
      gridTemplateColumns: 'repeat(auto-fit, minmax(250px, 1fr))',
      gap: theme.spacing(1.5),
    },
    
    // Mobile styles
    [theme.breakpoints.down('sm')]: {
      gridTemplateColumns: '1fr',
      gap: theme.spacing(1),
      padding: theme.spacing(1),
    },
  },
  
  jobCard: {
    minHeight: 200,
    transition: theme.transitions.create(['transform', 'box-shadow']),
    
    '&:hover': {
      transform: 'translateY(-2px)',
      boxShadow: theme.shadows[8],
    },
    
    // Mobile interaction
    [theme.breakpoints.down('sm')]: {
      '&:hover': {
        transform: 'none', // Disable hover effects on mobile
      },
    },
  },
}));
```

## Performance Guidelines

### Component Optimization

```typescript
// Memoization strategies
import React, { memo, useMemo, useCallback } from 'react';

// Memoize expensive components
const JobCard = memo<JobCardProps>(({ job, onStartJob }) => {
  // Component implementation
}, (prevProps, nextProps) => {
  // Custom comparison function
  return prevProps.job.id === nextProps.job.id &&
         prevProps.job.state === nextProps.job.state;
});

// Memoize expensive calculations
const JobList: React.FC<JobListProps> = ({ jobs, filters }) => {
  const filteredJobs = useMemo(() => {
    return jobs.filter(job => {
      return (!filters.status || job.state === filters.status) &&
             (!filters.facility || job.facilityId === filters.facility);
    });
  }, [jobs, filters.status, filters.facility]);
  
  const handleJobStart = useCallback((jobId: string) => {
    // Handler implementation
  }, []);
  
  return (
    <div>
      {filteredJobs.map(job => (
        <JobCard key={job.id} job={job} onStartJob={handleJobStart} />
      ))}
    </div>
  );
};
```

### Bundle Optimization

```typescript
// Code splitting with React.lazy
import { lazy, Suspense } from 'react';

// Lazy load heavy components
const JobExecutionView = lazy(() => import('./JobExecutionView'));
const ReportsView = lazy(() => import('./ReportsView'));
const SettingsView = lazy(() => import('./SettingsView'));

// Route-based code splitting
const AppRoutes: React.FC = () => (
  <Suspense fallback={<LoadingSpinner />}>
    <Switch>
      <Route path="/jobs/:jobId/execute" component={JobExecutionView} />
      <Route path="/reports" component={ReportsView} />
      <Route path="/settings" component={SettingsView} />
    </Switch>
  </Suspense>
);

// Dynamic imports for utilities
const importChartLibrary = () => import('recharts');

const ChartComponent: React.FC = () => {
  const [ChartLib, setChartLib] = useState(null);
  
  useEffect(() => {
    importChartLibrary().then(lib => setChartLib(lib));
  }, []);
  
  if (!ChartLib) return <LoadingSpinner />;
  
  return <ChartLib.LineChart data={data} />;
};
```

### Virtual Scrolling

```typescript
// Virtual scrolling for large lists
import { FixedSizeList as List } from 'react-window';

interface VirtualJobListProps {
  jobs: Job[];
  onJobSelect: (job: Job) => void;
}

const VirtualJobList: React.FC<VirtualJobListProps> = ({ jobs, onJobSelect }) => {
  const Row = useCallback(({ index, style }: { index: number; style: React.CSSProperties }) => (
    <div style={style}>
      <JobCard job={jobs[index]} onStartJob={() => onJobSelect(jobs[index])} />
    </div>
  ), [jobs, onJobSelect]);
  
  return (
    <List
      height={600}
      itemCount={jobs.length}
      itemSize={120}
      width="100%"
    >
      {Row}
    </List>
  );
};
```

## Accessibility Guidelines

### WCAG 2.1 Compliance

```typescript
// Semantic HTML and ARIA attributes
const JobExecutionForm: React.FC = () => {
  const [currentStep, setCurrentStep] = useState(0);
  const steps = ['Setup', 'Execute', 'Review'];
  
  return (
    <form role="form" aria-label="Job execution form">
      {/* Progress indicator */}
      <nav aria-label="Job execution progress">
        <ol>
          {steps.map((step, index) => (
            <li
              key={step}
              aria-current={index === currentStep ? 'step' : undefined}
              aria-label={`Step ${index + 1}: ${step}${index === currentStep ? ' (current)' : ''}`}
            >
              {step}
            </li>
          ))}
        </ol>
      </nav>
      
      {/* Form content */}
      <section
        role="main"
        aria-labelledby="step-title"
        aria-describedby="step-description"
      >
        <h2 id="step-title">{steps[currentStep]}</h2>
        <p id="step-description">
          Complete the {steps[currentStep].toLowerCase()} step to continue.
        </p>
      </section>
    </form>
  );
};
```

### Focus Management

```typescript
// Focus management for modals and navigation
import { useRef, useEffect } from 'react';

const Modal: React.FC<ModalProps> = ({ isOpen, onClose, children }) => {
  const modalRef = useRef<HTMLDivElement>(null);
  const previousFocusRef = useRef<HTMLElement | null>(null);
  
  useEffect(() => {
    if (isOpen) {
      // Store previously focused element
      previousFocusRef.current = document.activeElement as HTMLElement;
      
      // Focus modal
      modalRef.current?.focus();
      
      // Trap focus within modal
      const handleKeyDown = (e: KeyboardEvent) => {
        if (e.key === 'Escape') {
          onClose();
        }
        
        if (e.key === 'Tab') {
          // Focus trapping logic
          const focusableElements = modalRef.current?.querySelectorAll(
            'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
          );
          
          if (focusableElements && focusableElements.length > 0) {
            const firstElement = focusableElements[0] as HTMLElement;
            const lastElement = focusableElements[focusableElements.length - 1] as HTMLElement;
            
            if (e.shiftKey && document.activeElement === firstElement) {
              e.preventDefault();
              lastElement.focus();
            } else if (!e.shiftKey && document.activeElement === lastElement) {
              e.preventDefault();
              firstElement.focus();
            }
          }
        }
      };
      
      document.addEventListener('keydown', handleKeyDown);
      
      return () => {
        document.removeEventListener('keydown', handleKeyDown);
        
        // Restore focus
        if (previousFocusRef.current) {
          previousFocusRef.current.focus();
        }
      };
    }
  }, [isOpen, onClose]);
  
  if (!isOpen) return null;
  
  return (
    <div
      role="dialog"
      aria-modal="true"
      aria-labelledby="modal-title"
      ref={modalRef}
      tabIndex={-1}
    >
      {children}
    </div>
  );
};
```

### Screen Reader Support

```typescript
// Live regions for dynamic content
const JobStatusNotification: React.FC<{ job: Job }> = ({ job }) => {
  const [announcement, setAnnouncement] = useState('');
  
  useEffect(() => {
    setAnnouncement(`Job ${job.title} status changed to ${job.state}`);
  }, [job.state, job.title]);
  
  return (
    <>
      {/* Visual indicator */}
      <Badge color={getStatusColor(job.state)}>
        {job.state}
      </Badge>
      
      {/* Screen reader announcement */}
      <div
        role="status"
        aria-live="polite"
        aria-atomic="true"
        className="sr-only"
      >
        {announcement}
      </div>
    </>
  );
};

// Alternative text for images and icons
const ParameterIcon: React.FC<{ type: ParameterType }> = ({ type }) => {
  const getIconAlt = (type: ParameterType) => {
    switch (type) {
      case 'SHOULD_BE': return 'Expected value parameter';
      case 'CALCULATION': return 'Calculation parameter';
      case 'INSTRUCTION': return 'Instruction parameter';
      default: return `${type} parameter`;
    }
  };
  
  return (
    <img
      src={`/icons/${type}.svg`}
      alt={getIconAlt(type)}
      role="img"
    />
  );
};
```

## Pull Request Process

### Before Submitting

- [ ] Code follows TypeScript and ESLint guidelines
- [ ] All tests pass (unit, integration, e2e)
- [ ] Components are accessible (WCAG 2.1)
- [ ] Performance impact assessed
- [ ] Mobile responsiveness verified
- [ ] Cross-browser testing completed
- [ ] Storybook stories updated

### Pull Request Template

```markdown
## Description

Brief description of the changes and their purpose.

## Type of Change

- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation update
- [ ] Performance improvement
- [ ] Accessibility improvement

## Testing

- [ ] Unit tests added/updated
- [ ] Integration tests added/updated
- [ ] E2E tests added/updated
- [ ] Manual testing completed
- [ ] Accessibility testing completed

## Browser Testing

- [ ] Chrome (latest)
- [ ] Firefox (latest)
- [ ] Safari (latest)
- [ ] Edge (latest)
- [ ] Mobile browsers (iOS Safari, Chrome Mobile)

## Performance

- [ ] Bundle size impact assessed
- [ ] Runtime performance verified
- [ ] Memory usage checked
- [ ] Network requests optimized

## Accessibility

- [ ] Keyboard navigation works
- [ ] Screen reader compatible
- [ ] Color contrast meets WCAG AA
- [ ] Focus management implemented
- [ ] ARIA attributes added where needed

## Screenshots

Include screenshots for UI changes.

## Breaking Changes

List any breaking changes and migration steps.

## Additional Notes

Any additional information for reviewers.
```

## Issue Reporting

### Bug Reports

```markdown
## Bug Description

Clear description of the bug and expected behavior.

## Steps to Reproduce

1. Go to '...'
2. Click on '....'
3. Scroll down to '....'
4. See error

## Expected Behavior

Description of what you expected to happen.

## Actual Behavior

Description of what actually happened.

## Environment

- **OS**: [e.g. Windows 10, macOS 12.0, Ubuntu 20.04]
- **Browser**: [e.g. Chrome 96, Firefox 94, Safari 15]
- **Node Version**: [e.g. 16.14.0]
- **App Version**: [e.g. 4.9.11]

## Console Logs

```
Include browser console output here
```

## Network Tab

Include relevant network requests/responses if applicable.

## Additional Context

Any additional information, configuration, or screenshots.
```

### Feature Requests

```markdown
## Feature Description

Clear description of the proposed feature.

## Use Case

Description of the user story or problem this feature would solve.

## Proposed Solution

Description of your proposed solution with mockups if possible.

## Alternative Solutions

Description of alternative solutions you've considered.

## Implementation Notes

Technical considerations for implementation.

## Additional Context

Any additional context, examples, or research.
```

## Development Tools

### VS Code Configuration

```json
// .vscode/settings.json
{
  "typescript.preferences.importModuleSpecifier": "relative",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "emmet.includeLanguages": {
    "typescript": "html",
    "typescriptreact": "html"
  }
}

// .vscode/extensions.json
{
  "recommendations": [
    "bradlc.vscode-tailwindcss",
    "esbenp.prettier-vscode",
    "ms-vscode.vscode-typescript-next",
    "dbaeumer.vscode-eslint",
    "ms-vscode.vscode-json"
  ]
}
```

### ESLint Configuration

```json
// .eslintrc.json
{
  "extends": [
    "react-app",
    "react-app/jest",
    "@typescript-eslint/recommended"
  ],
  "plugins": ["@typescript-eslint", "react-hooks"],
  "rules": {
    "@typescript-eslint/explicit-function-return-type": "off",
    "@typescript-eslint/explicit-module-boundary-types": "off",
    "@typescript-eslint/no-explicit-any": "warn",
    "@typescript-eslint/no-unused-vars": "error",
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn"
  }
}
```

### Prettier Configuration

```json
// .prettierrc
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false
}
```

## Resources

### Documentation

- **[React Documentation](https://reactjs.org/docs/getting-started.html)**
- **[TypeScript Handbook](https://www.typescriptlang.org/docs/)**
- **[Material-UI v4 Documentation](https://v4.mui.com/)**
- **[Redux Toolkit Documentation](https://redux-toolkit.js.org/)**
- **[Testing Library Documentation](https://testing-library.com/docs/)**

### Tools

- **[React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools)**
- **[Redux DevTools](https://chrome.google.com/webstore/detail/redux-devtools)**
- **[Lighthouse](https://developers.google.com/web/tools/lighthouse)**
- **[WAVE Web Accessibility Evaluator](https://wave.webaim.org/)**

## Questions?

If you have questions about contributing, please:

1. Check existing documentation and issues
2. Search Stack Overflow for React/TypeScript questions
3. Create a new issue with your question
4. Contact the frontend development team

Thank you for contributing to Streem Frontend Platform!