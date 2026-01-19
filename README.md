# Bitbucket Release Documentation Generator

This script generates comprehensive release documentation in DOCX format by connecting to a Bitbucket private server and retrieving commit and deployment information.

## Features

- **Connects to Bitbucket Server** via REST API
- **Finds commits** after the `prod-server` tag in master branch
- **Retrieves deployment folder changes** from all commits
- **Generates 5 DOCX files**:
  - `Implementation_plan_CHG.docx` - Deployment details with changed files
  - `PRE_test_plan_CHG.docx` - Pre-deployment testing checklist
  - `POST_test_plan_CHG.docx` - Post-deployment validation checklist
  - `Rollback_plan_CHG.docx` - Rollback procedures with release tags
  - `Code_change_Review_CHG.docx` - Code review with commit links

## Installation

### Prerequisites
- Python 3.7+
- Bitbucket Server with API access
- Credentials with read access to the repository

### Setup

1. Install dependencies:
```bash
pip install -r requirements.txt
```

2. Configure the script:
   - Open `bitbucket_release_docs.py`
   - Update the `main()` function with your settings:
     - `BITBUCKET_URL`: Your Bitbucket server URL
     - `USERNAME`: Your Bitbucket username
     - `PASSWORD`: Your password or API token
     - `PROJECT_KEY`: Your project key (e.g., "PROJ")
     - `REPO_SLUG`: Your repository slug
     - `IMPLEMENTATION_TEXT`: Customized implementation details
     - `ROLLBACK_TEXT`: Customized rollback instructions

## Usage

### Basic Usage
```bash
python bitbucket_release_docs.py
```

This will:
1. Connect to Bitbucket
2. Find the `prod-server` tag
3. Retrieve all commits after that tag
4. Get all changed files in the deployment folder
5. Generate 5 DOCX files in `./release_documents/` directory

### Advanced Usage

Use the class directly in your own script:

```python
from bitbucket_release_docs import BitbucketReleaseDocumentGenerator

generator = BitbucketReleaseDocumentGenerator(
    server_url="https://bitbucket.company.com",
    username="your_username",
    password="your_token",
    project_key="PROJ",
    repo_slug="my-repo"
)

# Generate documents with custom text
saved_files = generator.save_documents(
    output_dir="./my_docs",
    implementation_text="Custom implementation details",
    rollback_text="Custom rollback steps"
)

for filename, filepath in saved_files.items():
    print(f"Generated: {filepath}")
```

## API Authentication

You can use either:
- **Username/Password** - Direct credentials
- **Username/API Token** - Recommended for security (use token in password field)

To generate an API token in Bitbucket Server:
1. Go to Settings → Manage Account → API Tokens
2. Create a new token with repository read access
3. Use the token as the password parameter

## Document Details

### Implementation_plan_CHG.docx
- Includes implementation overview (customizable)
- Lists all files changed in the deployment folder
- Shows commit IDs associated with each file
- Deployment status tracking

### PRE_test_plan_CHG.docx
- Pre-deployment test checklist
- Includes: Code quality, unit tests, integration tests, security scanning
- Status tracking table (Pass/Fail/Notes)

### POST_test_plan_CHG.docx
- Post-deployment validation checklist
- Health checks and service verification
- Success criteria documentation
- Validation status tracking

### Rollback_plan_CHG.docx
- Release tag information from the prod-server commit
- Rollback strategy section (customizable text)
- Step-by-step rollback procedures
- Triggers for when rollback is needed

### Code_change_Review_CHG.docx
- Table of all commits with details
- **Clickable hyperlinks** to each commit in Bitbucket
- Code review checklist items
- Links format: `https://bitbucket.company.com/projects/PROJ/repos/repo-name/commits/{commit_id}`

## Output

All documents are created in the `release_documents/` directory with:
- Proper formatting and styling
- Tables for easy data organization
- Checklists for action items
- Professional headers and footers with timestamps
- Hyperlinked commits for easy reference

## Error Handling

The script includes error handling for:
- Connection failures
- Missing tags or commits
- API rate limiting
- File permission issues

Errors are logged to console with descriptive messages.

## Troubleshooting

### "prod-server tag not found"
- Verify the tag exists in your Bitbucket repository
- Check that the tag name is exactly "prod-server"

### Connection Timeout
- Verify your Bitbucket URL is correct
- Check network connectivity to Bitbucket server
- Verify credentials are correct

### Permission Denied
- Ensure your user has read access to the repository
- Check that API token has appropriate permissions

## Notes

- The script filters commits from the master branch after the prod-server tag
- Only files in the "deployment" folder are included in the deployment files list
- Commit links are generated based on your Bitbucket server URL
- Timestamps are added to all documents
- All documents include variable text placeholders for customization
