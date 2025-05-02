Port11 Interview Agents - Knowledge Transfer
Port11 Interview Agent Architecture
Project Overview
This project is a sophisticated demonstration of advanced agentic patterns built on top of OpenAI's Realtime API. It showcases a Next.js application that enables voice conversations with AI agents, with several key innovations:
Sequential agent handoffs through a defined agent graph (inspired by OpenAI Swarm)
Background escalation to more capable models (o1 preview) for complex decisions
State machine-based prompting for accurate information collection and authentication
The application serves as a prototype platform for building multi-agent realtime voice applications that can be customized in minutes.
Architecture Overview
1. Application Structure
The application follows Next.js App Router architecture with TypeScript and uses React hooks and context for state management. Key directories include:
src/
├── app/
│   ├── agentConfigs/      # Agent definitions and configuration
│   ├── api/               # API routes for session management
│   ├── components/        # UI components
│   ├── contexts/          # React context providers for state management
│   ├── hooks/             # Custom React hooks
│   ├── lib/               # Utility functions and libraries
│   ├── App.tsx            # Main application component
│   ├── types.ts           # TypeScript type definitions
│   ├── page.tsx           # Main page component
│   └── layout.tsx         # Root layout component
2. Routing and Navigation
The application uses Next.js App Router for routing:
The main entry point is src/app/page.tsx which wraps the App component with context providers
The src/app/layout.tsx defines the root HTML structure
Navigation between agents happens via internal state changes rather than URL routing
The agent configuration is selected via URL query parameters
3. Core Components
App.tsx (Main Application)
The heart of the application, handling:
WebRTC connection initialization and management
Agent selection and switching
Audio streaming and user interface state
Real-time communication with the OpenAI API
UI Components
Transcript.tsx: Displays conversation history between user and agents
Events.tsx: Shows the event log for debugging
BottomToolbar.tsx: Controls for audio, connection, and agent selection
4. State Management
The application uses React Context for state management:
TranscriptContext
Manages conversation history, including:
Messages between user and agents
Breadcrumbs for tracking agent transitions
Tool calls and responses
EventContext
Tracks all events in the system:
Client-side events (actions taken by the user)
Server-side events (responses from the API)
Connection and session management events
5. Agent Configuration System
The core innovation of this project is its flexible agent configuration system:
// Example agent definition from the techSalesInterview flow
const managerAgent: AgentConfig = {
  name: "manager",
  publicDescription:
    "Agent that orchestrates the interview process by managing transitions between specialized agents.",
  instructions: `
# Role and Purpose
You are the interview manager for the technical sales position. Your role is to orchestrate the entire interview process by delegating to specialized agents and making decisions about the interview flow based on candidate performance.
# Process Flow
1. Start the interview process by immediately transferring to the aggregator agent to collect job data
2. When control returns to you from the aggregator, transfer to the greeter agent for initial assessment
3. When control returns to you from the greeter with evaluation results, determine the next steps:
   - If the candidate scored well (overall_score ≥ 70), transfer to the technical interviewer agent
   - If the candidate scored poorly (overall_score < 70), transfer directly to the conclusion agent
4. When control returns to you from the technical interviewer (if applicable), transfer to the conclusion agent
  `,
  tools: [],
  downstreamAgents: [
    aggregatorAgent,
    greetingAgent,
    technicalInterviewerAgent,
    conclusionInterviewerAgent,
  ],
};
Agents are defined with:
Name: Unique identifier for the agent
Public Description: Context provided for agent transfers
Instructions: Prompt defining the agent's behavior
Tools: Functions the agent can call
Downstream Agents: Other agents this agent can transfer to
The example above shows a manager agent from the techSalesInterview flow, which orchestrates the entire interview process. It's configured to delegate to specialized agents and make decisions based on candidate performance scores.
6. Communication Flow
Client-Server Connection:
WebRTC connection established with OpenAI's Realtime API
Audio streaming occurs bidirectionally through this connection
Control messages sent via data channel
Agent Interaction Flow:
User speaks or types message
Message sent to current agent
Agent processes and responds
Agent may call tools or transfer to another agent
UI updates with transcript and events
Agent Transfers:
Agents can transfer control to downstream agents
Transfer occurs via a special "transfer" tool
UI shows breadcrumb of transition
New agent takes over the conversation
7. Advanced Features
Agent Handoffs
Agents can transfer to other agents based on user needs
Transfer preserves conversation context
Allows specialized agents for different parts of the conversation
Background Escalation
Complex decisions can be escalated to more capable models
Example: o1-mini preview used to verify return eligibility in retail scenario
Prompt State Machines
Agents can follow a defined state for accurate information collection
Character-by-character confirmation for sensitive data
Useful for authentication flows
Example Use Cases
The project includes several pre-built agent configurations:
Simple Example: Basic greeter and haiku writer agents
Front Desk Authentication: Authentication flow with character-by-character confirmation
Customer Service Retail: Complex return flow with authentication, policy lookup, and escalation
Development and Extension
To create new agent configurations:
Create a new configuration file in src/app/agentConfigs/
Define your agents with appropriate instructions and tools
Configure agent transfers via downstreamAgents
Add to the index.ts to make it available in the UI
Technical Implementation Details
Real-time Connection
Uses WebRTC for real-time audio streaming
RTCDataChannel for control messages
Session established via ephemeral keys from API
Audio Processing
Voice activity detection for automatic speech detection
Push-to-talk option for noisy environments
Audio playback control
Agent Decision Making
Tools provide functionality like information lookup and authentication
State tracking for complex flows
Model escalation for high-stakes decisionsPort11 Interview Agents - Knowledge Transfer
Port11 Interview Agent Architecture
Project Overview
This project is a sophisticated demonstration of advanced agentic patterns built on top of OpenAI's Realtime API. It showcases a Next.js application that enables voice conversations with AI agents, with several key innovations:
Sequential agent handoffs through a defined agent graph (inspired by OpenAI Swarm)
Background escalation to more capable models (o1 preview) for complex decisions
State machine-based prompting for accurate information collection and authentication
The application serves as a prototype platform for building multi-agent realtime voice applications that can be customized in minutes.
Architecture Overview
1. Application Structure
The application follows Next.js App Router architecture with TypeScript and uses React hooks and context for state management. Key directories include:
src/
├── app/
│   ├── agentConfigs/      # Agent definitions and configuration
│   ├── api/               # API routes for session management
│   ├── components/        # UI components
│   ├── contexts/          # React context providers for state management
│   ├── hooks/             # Custom React hooks
│   ├── lib/               # Utility functions and libraries
│   ├── App.tsx            # Main application component
│   ├── types.ts           # TypeScript type definitions
│   ├── page.tsx           # Main page component
│   └── layout.tsx         # Root layout component
2. Routing and Navigation
The application uses Next.js App Router for routing:
The main entry point is src/app/page.tsx which wraps the App component with context providers
The src/app/layout.tsx defines the root HTML structure
Navigation between agents happens via internal state changes rather than URL routing
The agent configuration is selected via URL query parameters
3. Core Components
App.tsx (Main Application)
The heart of the application, handling:
WebRTC connection initialization and management
Agent selection and switching
Audio streaming and user interface state
Real-time communication with the OpenAI API
UI Components
Transcript.tsx: Displays conversation history between user and agents
Events.tsx: Shows the event log for debugging
BottomToolbar.tsx: Controls for audio, connection, and agent selection
4. State Management
The application uses React Context for state management:
TranscriptContext
Manages conversation history, including:
Messages between user and agents
Breadcrumbs for tracking agent transitions
Tool calls and responses
EventContext
Tracks all events in the system:
Client-side events (actions taken by the user)
Server-side events (responses from the API)
Connection and session management events
5. Agent Configuration System
The core innovation of this project is its flexible agent configuration system:
// Example agent definition from the techSalesInterview flow
const managerAgent: AgentConfig = {
  name: "manager",
  publicDescription:
    "Agent that orchestrates the interview process by managing transitions between specialized agents.",
  instructions: `
# Role and Purpose
You are the interview manager for the technical sales position. Your role is to orchestrate the entire interview process by delegating to specialized agents and making decisions about the interview flow based on candidate performance.
# Process Flow
1. Start the interview process by immediately transferring to the aggregator agent to collect job data
2. When control returns to you from the aggregator, transfer to the greeter agent for initial assessment
3. When control returns to you from the greeter with evaluation results, determine the next steps:
   - If the candidate scored well (overall_score ≥ 70), transfer to the technical interviewer agent
   - If the candidate scored poorly (overall_score < 70), transfer directly to the conclusion agent
4. When control returns to you from the technical interviewer (if applicable), transfer to the conclusion agent
  `,
  tools: [],
  downstreamAgents: [
    aggregatorAgent,
    greetingAgent,
    technicalInterviewerAgent,
    conclusionInterviewerAgent,
  ],
};
Agents are defined with:
Name: Unique identifier for the agent
Public Description: Context provided for agent transfers
Instructions: Prompt defining the agent's behavior
Tools: Functions the agent can call
Downstream Agents: Other agents this agent can transfer to
The example above shows a manager agent from the techSalesInterview flow, which orchestrates the entire interview process. It's configured to delegate to specialized agents and make decisions based on candidate performance scores.
6. Communication Flow
Client-Server Connection:
WebRTC connection established with OpenAI's Realtime API
Audio streaming occurs bidirectionally through this connection
Control messages sent via data channel
Agent Interaction Flow:
User speaks or types message
Message sent to current agent
Agent processes and responds
Agent may call tools or transfer to another agent
UI updates with transcript and events
Agent Transfers:
Agents can transfer control to downstream agents
Transfer occurs via a special "transfer" tool
UI shows breadcrumb of transition
New agent takes over the conversation
7. Advanced Features
Agent Handoffs
Agents can transfer to other agents based on user needs
Transfer preserves conversation context
Allows specialized agents for different parts of the conversation
Background Escalation
Complex decisions can be escalated to more capable models
Example: o1-mini preview used to verify return eligibility in retail scenario
Prompt State Machines
Agents can follow a defined state for accurate information collection
Character-by-character confirmation for sensitive data
Useful for authentication flows
Example Use Cases
The project includes several pre-built agent configurations:
Simple Example: Basic greeter and haiku writer agents
Front Desk Authentication: Authentication flow with character-by-character confirmation
Customer Service Retail: Complex return flow with authentication, policy lookup, and escalation
Development and Extension
To create new agent configurations:
Create a new configuration file in src/app/agentConfigs/
Define your agents with appropriate instructions and tools
Configure agent transfers via downstreamAgents
Add to the index.ts to make it available in the UI
Technical Implementation Details
Real-time Connection
Uses WebRTC for real-time audio streaming
RTCDataChannel for control messages
Session established via ephemeral keys from API
Audio Processing
Voice activity detection for automatic speech detection
Push-to-talk option for noisy environments
Audio playback control
Agent Decision Making
Tools provide functionality like information lookup and authentication
State tracking for complex flows
Model escalation for high-stakes decisions