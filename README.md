---
icon: robot
---

# Port11 Interview Next.js- Knowledge Transfer Document

### Project Overview

This project is a Next.js-based interview platform that allows users to take\
tests, complete coding challenges, and participate in interviews. The\
application includes features such as user authentication, proctoring\
capabilities, coding environments, and test monitoring.

### Tech Stack

* **Framework**: Next.js 15
* **UI**: React 19, TailwindCSS
* **State Management**: Redux Toolkit with Redux Persist
* **Database**: Prisma with PostgreSQL
* **UI Components**: Various Radix UI components, shadcn-like UI component  \
  system
* **Code Editor**: CodeMirror
* **Media Handling**: Media recording capabilities for audio/video
* **Authentication**: Custom auth system

### Application Structure

#### Directory Structure

```
src/
├── api/           # API utilities and endpoints
├── app/           # Next.js App Router routes
├── assets/        # Static assets
├── components/    # UI components
├── data/          # Mock/sample data
├── hooks/         # Custom React hooks
├── interface/     # TypeScript interfaces
├── lib/           # Utility libraries
├── redux/         # Redux store and slices
└── utils/         # Utility functions
```

### Routes

#### Main Routes

1. **/** - Homepage
   * Entry point of the application
2. **/login** - Authentication
   * Handles user login
   * Redirects authenticated users to /jobs/available
   * Uses `LandingPage` component from `@/components/auth`
3. **/begin-test** - Test Initialization
   * Starting point for tests/interviews
   * Loads test data based on URL parameters (job\_id, task\_id, etc.)
   * Renders the `TestScreen` component from the proctoring system
   * Handles loading test configurations and user data
4. **/test-screen** - Test Environment
   * Main interface for taking tests
   * Integrates with the proctoring system
   * Uses `HeadsUpWrapper` component for proctoring
5. **/port11-interview-agent** - AI-based Interview Agent
   * Likely handles AI-assisted interviews
6. **(restricted-resource)/** - Protected Routes
   * Contains routes that require authentication

### Component Structure

#### Core Components

**Authentication Components**

* **LandingPage**: Main authentication entry point
* Other auth-related components in `src/components/auth/`

**Proctoring Components**

* **HeadsUpWrapper**: Wrapper for proctoring capabilities
* Other proctoring components in `src/components/proctoring/`

**UI Components**

* Shadcn-like UI component system in `src/components/ui/`
* Common UI elements like buttons, inputs, modals, etc.

**Test-related Components**

* Components for displaying and managing tests
* Code editor components for coding exercises
* Test status and progress indicators

### Component Interaction Flow

```
                          ┌───────────────┐
                          │  App Layout   │
                          └───────┬───────┘
                                  │
              ┌──────────────────┬┴─────────────────┐
              │                  │                  │
       ┌──────▼─────┐    ┌───────▼─────┐    ┌───────▼────────┐
       │ LandingPage│    │  BeginTest  │    │  TestScreen    │
       └──────┬─────┘    └───────┬─────┘    └───────┬────────┘
              │                  │                  │
     ┌────────▼────────┐ ┌──────▼───────┐  ┌───────▼────────┐
     │ Authentication  │ │  TestScreen  │  │ HeadsUpWrapper │
     │   Components    │ │  Component   │  │   (Proctoring) │
     └─────────────────┘ └──────────────┘  └────────────────┘
```

### Authentication Flow

1. User navigates to `/login`
2. `LandingPage` component renders the login interface
3. Upon successful authentication, the user is redirected to `/jobs/available`
4. Authentication status is stored in cookies and managed through Redux

### Test Flow

1. User selects a job/test from the available options
2. User is directed to `/begin-test` with appropriate parameters
3. `BeginTest` page loads test configuration and user data
4. The test interface is rendered with the `TestScreen` component
5. Proctoring is initiated with `HeadsUpWrapper` if required
6. User completes the test and submits responses
7. Results are processed and stored

### State Management

The application uses Redux Toolkit for state management:

* User authentication state
* Test progress and answers
* UI state (themes, preferences)
* Persisted state through Redux Persist

### API Integration

The application interacts with backend services for:

* User authentication and profile management
* Loading test data and configurations
* Submitting test answers and receiving feedback
* Compiling and executing code for programming challenges
* Proctoring data transmission

### UI Theming

The application supports light and dark modes using:

* TailwindCSS for styling
* next-themes for theme management
* Custom theme components in `src/components/themes/`

### TestScreen Implementation Details

The TestScreen component is a complex and comprehensive implementation that\
handles the core functionality of the test-taking experience. Located in`proctoringSystem/src/components/TestScreen/TestScreen.tsx`, it integrates\
various features including:

#### Core Functionality

1. **Code Editor Integration**
   * Uses a custom CEditor component for writing and editing code
   * Maintains code state and persists it in sessionStorage
   * Provides syntax highlighting and code formatting
2. **Test Problem Display**
   * Renders the test question and requirements
   * Organizes content in tabs for better navigation
   * Supports markdown rendering for rich content
3. **Compilation and Execution**
   * Integrates with the backend compiler API
   * Handles compilation requests and response parsing
   * Displays compilation errors and execution results
4. **Test Progress Tracking**
   * Tracks time spent on questions
   * Monitors completion status
   * Manages navigation between questions

#### Proctoring Features

1. **Face Detection and Tracking**
   * Uses face-api.js for face detection
   * Tracks facial movements and positioning
   * Detects when the user looks away from the screen
2. **Audio Monitoring**
   * Monitors ambient noise levels
   * Detects unauthorized conversations
   * Records audio samples when suspicious activity is detected
3. **Behavioral Monitoring**
   * Tracks keyboard and mouse events
   * Monitors tab switching and window focus
   * Detects attempts to copy/paste or access unauthorized resources
4. **Calibration System**
   * Initial calibration to establish baseline metrics
   * Adjusts detection thresholds based on user's environment
   * Provides visual feedback during calibration
5. **Screenshot Capture**
   * Takes periodic screenshots for review
   * Captures additional screenshots when suspicious activity is detected
   * Converts captures to base64 for transmission to the server

#### Event Management

1. **Warning System**
   * Detects potential violations
   * Displays warnings to the user
   * Tracks warning counts and types
2. **Notification Management**
   * Shows popup notifications for important events
   * Manages notification visibility and timing
   * Provides user feedback on system status
3. **Session Tracking**
   * Monitors test session duration
   * Handles session interruptions and resumptions
   * Manages fullscreen mode and visibility changes

#### Communication with Backend

1. **API Integration**
   * Sends test results to the server
   * Reports proctoring events and violations
   * Retrieves hints and additional information
2. **Data Transmission**
   * Manages the format and encoding of transmitted data
   * Handles authentication and authorization
   * Implements error handling and retries

#### Component Architecture

The TestScreen component uses several React patterns:

1. **State Management**
   * Uses React useState for local state
   * Implements useRef for persistent references
   * Leverages useEffect for lifecycle management
2. **Context Integration**
   * Integrates with HeadsUpContext for proctoring state
   * Provides context for child components
   * Shares state across the component tree
3. **Callback Handling**
   * Implements numerous callback functions for events
   * Manages async operations with proper error handling
   * Uses debouncing and throttling for performance

### Agentic AI Architecture

The agentic AI system is implemented in the `proctoringSystem/src/agentConfigs`\
directory and provides intelligent, autonomous agents for interview scenarios.

#### Core Structure

1. **Agent Configuration System**
   * Located in `proctoringSystem/src/agentConfigs`
   * Modular design with different agent types
   * Configurable prompts and behavior patterns
2. **Tech Sales Interview Agents**
   * Primary agent set defined in `techSalesInterview` directory
   * Multiple specialized agents working together
   * Coordinated by an aggregator component

#### Agent Types

1. **Greeting Agent** (`greeting.ts`)
   * Handles the initial interaction with the candidate
   * Sets the tone and explains the interview process
   * Collects preliminary information
2. **Technical Interviewer** (`technicalInterviewer.ts`)
   * Conducts technical portions of the interview
   * Asks domain-specific questions
   * Evaluates technical responses and adapts questioning
3. **Conclusion Interviewer** (`conclusionInterviewer.ts`)
   * Wraps up the interview process
   * Provides summary and next steps
   * Gathers final candidate feedback
4. **Aggregator** (`aggregator.ts`)
   * Coordinates between different agent types
   * Manages the flow of the interview
   * Determines when to switch between agents

#### Agent Configuration

Each agent is configured with:

1. **System Prompts**: Define the agent's role and behavior
2. **Response Templates**: Structure for generating consistent responses
3. **Question Libraries**: Pre-defined questions and follow-ups
4. **Evaluation Logic**: Rules for assessing candidate responses

#### Integration with Front-end

The agent system integrates with the front-end through:

1. **Event Context**: Listens for and responds to user interactions
2. **Transcript Context**: Maintains the interview transcript
3. **WebSocket Communication**: Real-time messaging between agents and UI
4. **State Management**: Tracking interview progress and candidate performance

#### Job Data Integration

The system incorporates specific job details through:

1. **Job Definition Files**: In `jobData.ts`
2. **Dynamic Content Loading**: Adjusts interview focus based on job   \
   requirements
3. **Skill Mapping**: Connects candidate responses to required skills

This architecture creates a flexible, intelligent interview system capable of\
conducting realistic, adaptive interviews without human intervention, while\
still maintaining the ability to evaluate candidate responses effectively.

### Notes for Future Development

1. The proctoring system is integrated but will be updated in future iterations
2. The application uses a mix of client and server components for optimal   \
   performance
3. The codebase follows modern Next.js patterns with the App Router architecture
4. Redux is used for client-side state that needs to persist across page   \
   navigations
