kanaloa-docs/
├── .github/
│   └── workflows/
│       └── docs-validator.yml     # Automated link checker & linting pipeline
├── config/
│   └── sidebar.json               # Defines the global navigation structure
├── docs/
│   ├── index.md                   # Portal landing page
│   ├── quick-start.md             # The "First 5 Minutes" guide
│   ├── changelog.md               # What's New / chronological feed
│   ├── faq.md                     # Support deflection knowledge base
│   │
│   ├── tutorials/                 # Task-oriented "How-to" guides
│   │   ├── README.md
│   │   ├── create-data-report.md
│   │   ├── configure-auv-sync.md
│   │   └── manage-team-permissions.md
│   │
│   └── api/                       # Reference materials for power users/devs
│       ├── openapi.yaml           # Source OpenAPI specification file
│       └── reference.md           # Interactive sandbox & endpoint documentation
└── public/
    └── assets/                    # Shared media and schematics
        └── images/