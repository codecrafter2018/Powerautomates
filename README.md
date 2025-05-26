# 📦 Power Automate Workflows for Dataverse Integration

This repository contains **two production-ready Microsoft Power Automate workflows (cloud flows)** designed to automate operations within Microsoft Dataverse, including:

1. ✅ Creation of VO Functions via external API
2. 📅 Automated visit confirmation for upcoming events

These workflows help eliminate manual processes, improve communication, and integrate Dataverse with third-party systems.

## 🚀 Features

- **HTTP API Integration**: RESTful endpoints for external system integration
- **Dataverse Automation**: Seamless CRUD operations with Microsoft Dataverse
- **Duplicate Prevention**: Built-in logic to prevent duplicate record creation
- **Automated Messaging**: Dynamic message generation and delivery
- **Secure Authentication**: Bearer token and environment variable support
- **Production Ready**: Error handling and comprehensive logging

## 🔁 Workflow 1: `create_vofunction`

### 🔧 Purpose

Allows external applications to securely create a VO Function record in Microsoft Dataverse using an HTTP POST request.

### ⚙️ How It Works

- **Trigger**: HTTP request received
- **Input**: JSON payload containing VO details (`vofunction_id`, `vofunction_name`, `description`, `type`, `source`)
- **Process**:
  - Parses input values using `Compose` actions
  - Searches Dataverse using `List rows` to check if a record with the given `vofunction_id` already exists
  - If it does not exist:
    - Creates a new VO Function record
    - Populates metadata like subject, priority, status, duration, description, and ownership
    - Sends a 200 OK response with success status
  - If a duplicate is found:
    - Returns a handled response indicating the record already exists

### 📥 Sample Input Payload

```json
{
  "vofunction_id": "VO123",
  "vofunction_name": "Technical Onboarding",
  "description": "Initial configuration and orientation tasks",
  "type": "Setup",
  "source": "ExternalApp"
}
```

### ✅ Use Case

Ideal for organizations that want to automate task creation in Dataverse from third-party apps, websites, or CRMs.

## 📅 Workflow 2: `VisitConfirmationforUserforUpcomingEvent`

### 🔧 Purpose

Automatically sends personalized visit confirmation messages to users based on task creation or update events in Dataverse.

### ⚙️ How It Works

- **Trigger**: When a row (task) is added, modified, or deleted in the `Task` table
- **Conditions**:
  - The task must not be marked as completed (`isTask_x = false` or similar)
- **Process**:
  - Retrieves related Contact details from Dataverse using FetchXML
  - Constructs a dynamic message using name, date, and task details
  - Sends this message to the user using an external API (e.g., Vapi, Twilio) via HTTP POST
  - Includes authentication via a secure environment variable (`Apikeyforvapi`) passed as a Bearer token
  - Updates the original task record to log that a confirmation message was sent

### 💬 Example Message

> "Hi Alex, this is Jane from MedConnect. We're confirming your upcoming appointment on Thursday at 3 PM for a routine checkup. Please let us know if you'd like to keep this appointment or reschedule."

### ✅ Use Case

Used in medical, education, or customer-service domains to automate outreach, reduce no-shows, and keep communication consistent.

## 📦 Installation

### Prerequisites

- Microsoft Power Automate license (Premium recommended)
- Access to Microsoft Dataverse environment
- Appropriate permissions to import solutions

### Step 1: Download the Workflow Package

1. Download the solution ZIP file: `DataverseWorkflows_1_0_0_0.zip`
2. Unzip the file to view the internal structure
3. If necessary, re-compress the solution folder into a `.zip` to match Power Automate import requirements

### Step 2: Import into Power Automate

1. Go to [Power Automate Portal](https://make.powerautomate.com/)
2. Navigate to **Solutions** → **Import**
3. Upload the ZIP file
4. Follow the import wizard:
   - Choose the appropriate **Environment**
   - Resolve all **connections** (e.g., HTTP, Dataverse connectors)
   - Finish the import

### Step 3: Configure Environment Variables

Set up the required environment variables:
- `Apikeyforvapi`: Your external API authentication token

## 🔐 Security Configuration

### For `create_vofunction`

This workflow exposes an HTTP endpoint. Recommended security methods:
- API Key in the header
- Azure API Management policies
- OAuth 2.0 / Azure AD for enterprise-grade security

### For `VisitConfirmationforUserforUpcomingEvent`

- Uses an external API that requires a secure `Authorization: Bearer <token>` header
- API key is stored in a secure **environment variable** named `Apikeyforvapi`

## 🛠 Technologies Used

- Microsoft Power Automate (Cloud Flows)
- Microsoft Dataverse
- HTTP Connector (Premium)
- Environment Variables
- JSON parsing, branching logic, conditions
- External REST APIs (for messaging integration)

## 📊 Supported Integrations

- **CRM Systems**: Salesforce, Dynamics 365
- **Messaging Services**: Twilio, Vapi
- **External APIs**: Any REST-compliant service
- **Database Systems**: SQL Server, Azure SQL

## 🔄 Workflow Architecture

```
External System → HTTP Request → Power Automate → Dataverse → Response
                                      ↓
                               Message Service → User Notification
```

## 📈 Performance Considerations

- **Rate Limits**: Be aware of Power Automate and Dataverse API limits
- **Bulk Operations**: Consider batch processing for large datasets
- **Error Handling**: Built-in retry logic and error notifications
- **Monitoring**: Use Power Automate analytics for performance tracking

## 🐛 Troubleshooting

### Common Issues

1. **Connection Errors**: Verify Dataverse and HTTP connections are properly configured
2. **Authentication Failures**: Check environment variables and API keys
3. **Duplicate Records**: Review the duplicate detection logic in `create_vofunction`
4. **Message Delivery**: Validate external API credentials and endpoints

### Debugging Tips

- Enable detailed logging in Power Automate
- Use the "Test" feature to validate workflows
- Check run history for error details
- Verify JSON payload structure matches expected format

## 🤝 Contributing

We welcome contributions! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines

- Follow Microsoft Power Automate best practices
- Include comprehensive error handling
- Document any new environment variables
- Test thoroughly before submitting

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.


## 🙏 Acknowledgments

- Microsoft Power Automate team for the excellent platform
- Community contributors and testers
- Open source projects that inspired this solution

---

**Made with ❤️ for the Power Platform community**
