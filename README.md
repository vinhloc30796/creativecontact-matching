# Creative Contact - Matching Services

## Architectures

```mermaid
flowchart TD

    %% Data Collection Services
    prefDB[(Preference Rankings DB)]
    netDB[(Social Network DB)]
    
    %% Core Services
    prefSvc[Preference Processing Service]
    netSvc[Network Analysis Service]
    groupSvc[Group Formation Service]
    
    %% Data Flow
    prefDB --> prefSvc
    prefSvc --> netSvc
    netDB --> netSvc
    
    netSvc --> groupSvc
    
    %% Processing Details
    subgraph PrefProcessing[Preference Processing]
        prefSvc --> rankMat[Generate Ranking Matrix]
        rankMat --> affMat[Convert to Affinity Matrix]
    end
    
    subgraph NetAnalysis[Network Analysis]
        netSvc --> triads[Identify Potential Triads]
        triads --> triadScore[Score Triads]
    end
    
    subgraph GroupFormation[Group Formation]
        triadScore --> optimize[Optimize Groups]
        affMat --> optimize
        optimize --> validate[Validate Groups]
        validate --> final[Final Group Assignment]
    end

    classDef database fill:#ffe070,stroke:#000,color:#000
    classDef service fill:#c9aedd,stroke:#000,color:#000
    classDef process fill:#b1d8ab,stroke:#000,color:#000

    class prefDB,netDB database
    class prefSvc,netSvc,groupSvc service
    class rankMat,affMat,triads,triadScore,optimize,validate,final process
```

## User flow

```mermaid
flowchart TD

    %% Initial Steps
    start([Start]) --> login[Login/Register]
    login --> rankings[Submit Rankings]
    
    %% Ranking Process
    rankings --> viewList[View Artist List]
    viewList --> selectTop[Select Top Preferences]
    selectTop --> confirmRank[Confirm Rankings]
    
    %% Waiting Phase
    confirmRank --> wait[Wait for Group Formation]
    
    %% Results
    wait --> notify[Receive Notification]
    notify --> viewGroup[View Group Assignment]
    
    %% Post Assignment
    viewGroup --> viewProfiles[View Group Profiles]
    viewProfiles --> connect[Connect with Group]
    
    %% Optional Feedback
    connect --> feedback[Optional: Provide Feedback]
    
    %% States
    stateDone[Group Formation Complete]
    stateWait[Waiting for Others]
    
    confirmRank --> stateWait
    stateWait --> stateDone
    stateDone --> notify

    classDef userAction fill:#ffe070,stroke:#000,color:#000
    classDef systemState fill:#c9aedd,stroke:#000,color:#000
    classDef interaction fill:#b1d8ab,stroke:#000,color:#000

    class login,rankings,selectTop,viewProfiles,connect,feedback userAction
    class stateWait,stateDone systemState
    class viewList,confirmRank,wait,notify,viewGroup interaction
```

## Development

`just dev` to run the project.

### Environment

We use `uv` to manage our python environment. Cheatsheet below:

#### Environment management:

```bash
uv venv: Create a new virtual environment.
```

####  Project management:

```bash
uv init # Create a new Python project.
uv add # Add a dependency to the project.
uv remove # Remove a dependency from the project.
uv sync # Sync the project's dependencies with the environment.
uv lock # Create a lockfile for the project's dependencies.
uv run # Run a command in the project environment.
uv tree # View the dependency tree for the project.
uv build # Build the project into distribution archives.
uv publish # Publish the project to a package index.
```

### Deployment

We deploy with fly.io.

```bash