# Google Cloud Pub/Sub: Quick Start - Command Line Lab

## Project Overview
This hands-on lab provides a command-line introduction to Google Cloud Pub/Sub, a scalable, durable event ingestion and delivery system. The lab focuses on mastering Pub/Sub fundamentals using the Google Cloud CLI (`gcloud`) without requiring any programming, making it ideal for system administrators and DevOps professionals.

## Lab Objectives
- Understand the core architecture of Pub/Sub messaging systems
- Master topic management: creation, listing, and deletion via CLI
- Implement subscription management for message consumption
- Practice message publishing and retrieval using command-line tools
- Learn to use flags and parameters for advanced Pub/Sub operations

## Solution Approach
The lab follows a progressive learning path:

1. **Environment Setup**: Access Google Cloud Console and activate Cloud Shell
2. **Topic Operations**: Create, list, and delete Pub/Sub topics
3. **Subscription Management**: Create subscriptions and understand topic-subscription relationships
4. **Message Lifecycle**: Publish messages and retrieve them using pull subscriptions
5. **Advanced Operations**: Use flags for batch message retrieval and acknowledgment

## Key Commands & Configurations

### Prerequisites
- Google Cloud Platform account with billing enabled
- Basic familiarity with command-line interfaces
- Access to Google Cloud Console

### Essential gcloud Commands

#### Topic Management
```bash
# Create a new topic
gcloud pubsub topics create myTopic

# List all topics in project
gcloud pubsub topics list

# Delete a topic
gcloud pubsub topics delete Test1
```

#### Subscription Operations
```bash
# Create subscription for a topic
gcloud pubsub subscriptions create --topic myTopic mySubscription

# List subscriptions for a specific topic
gcloud pubsub topics list-subscriptions myTopic

# Delete a subscription
gcloud pubsub subscriptions delete Test1
```

#### Message Publishing and Consumption
```bash
# Publish a message to topic
gcloud pubsub topics publish myTopic --message "Hello"

# Pull a single message (with auto-acknowledgment)
gcloud pubsub subscriptions pull mySubscription --auto-ack

# Pull multiple messages with limit flag
gcloud pubsub subscriptions pull mySubscription --limit=3
```

## Architecture Flow

```
┌─────────────┐     Create      ┌─────────────┐     Create      ┌─────────────────┐
│   Cloud     │────────────────▶│     Topic    │────────────────▶│   Subscription  │
│   Shell     │                 │   (myTopic)  │                 │  (mySubscription)│
└─────────────┘                 └─────────────┘                 └─────────────────┘
       │                              │                                   │
       │ Publish                      │                                   │ Pull
       │ "Hello"                      │                                   │ (--limit=3)
       ▼                              ▼                                   ▼
┌─────────────┐                ┌─────────────┐                   ┌─────────────────┐
│   Message   │                │  Message     │                   │   Output to     │
│   Queue     │                │  Storage     │                   │    Terminal     │
└─────────────┘                └─────────────┘                   └─────────────────┘
```

<img width="1449" height="718" alt="Screenshot 2026-01-12 181316" src="https://github.com/user-attachments/assets/cd033ffd-682a-4ea2-8038-3c59ab221d82" />
<img width="1442" height="739" alt="Screenshot 2026-01-12 181126" src="https://github.com/user-attachments/assets/72b61f71-6d7d-4b36-a9b7-a0a3ca9604a1" />
<img width="1461" height="701" alt="Screenshot 2026-01-12 181042" src="https://github.com/user-attachments/assets/94149bf6-8a04-4070-89fd-01210048c70b" /<img width="1453" height="724" alt="Screenshot 2026-01-12 181012" src="https://github.com/user-attachments/assets/4c91a22a-1a18-47ab-af3a-a2ce2d996138" />
>
<img width="1421" height="680" alt="Screenshot 2026-01-12 180703" src="https://github.com/user-attachments/assets/edfacbf6-8ce1-45b8-bd6a-bd7e68bf0136" />

## Key Learning Points

### Topic Fundamentals
- Topics act as named resources to which messages are sent
- Multiple publishers can send messages to the same topic
- Topics are regional resources that can be configured for specific regions

### Subscription Concepts
- Each subscription represents a stream of messages from a single topic
- Multiple subscriptions can be attached to one topic
- Subscribers receive messages independently of other subscribers

### Message Behavior
- **Default pull behavior**: Retrieves only one message at a time
- **Message consumption**: Once pulled with acknowledgment, messages are removed from the subscription
- **Batch operations**: Use `--limit` flag to retrieve multiple messages
- **Auto-acknowledgment**: `--auto-ack` flag automatically acknowledges received messages

### Important Limitations
- Messages can only be retrieved once per subscription pull
- Unacknowledged messages are redelivered after the acknowledgment deadline
- By default, `gcloud pubsub subscriptions pull` returns only one message

## Practical Exercises

### Exercise 1: Topic Lifecycle Management
```bash
# Create multiple topics
gcloud pubsub topics create myTopic
gcloud pubsub topics create Test1
gcloud pubsub topics create Test2

# Verify creation
gcloud pubsub topics list

# Clean up test topics
gcloud pubsub topics delete Test1
gcloud pubsub topics delete Test2
```

### Exercise 2: Subscription Management
```bash
# Create primary subscription
gcloud pubsub subscriptions create --topic myTopic mySubscription

# Create additional subscriptions
gcloud pubsub subscriptions create --topic myTopic Test1
gcloud pubsub subscriptions create --topic myTopic Test2

# List all subscriptions for topic
gcloud pubsub topics list-subscriptions myTopic
```

### Exercise 3: Message Publishing
```bash
# Publish simple message
gcloud pubsub topics publish myTopic --message "Hello"

# Publish personalized messages (replace placeholders)
gcloud pubsub topics publish myTopic --message "Publisher's name is <YOUR NAME>"
gcloud pubsub topics publish myTopic --message "Publisher likes to eat <FOOD>"
gcloud pubsub topics publish myTopic --message "Publisher thinks Pub/Sub is awesome"
```

### Exercise 4: Message Retrieval Patterns
```bash
# Single message retrieval
gcloud pubsub subscriptions pull mySubscription --auto-ack

# Multiple message retrieval
gcloud pubsub topics publish myTopic --message "Publisher is starting to get the hang of Pub/Sub"
gcloud pubsub topics publish myTopic --message "Publisher wonders if all messages will be pulled"
gcloud pubsub topics publish myTopic --message "Publisher will have to test to find out"

# Batch pull with limit
gcloud pubsub subscriptions pull mySubscription --limit=3
```

## Common Flags and Options

### Pull Command Flags
- `--auto-ack`: Automatically acknowledge received messages
- `--limit=N`: Set maximum number of messages to pull (default: 1)
- `--format=json`: Output in JSON format for programmatic processing
- `--wait`: Wait for messages to arrive if none are immediately available

### Publish Command Options
- `--message`: The message data payload
- `--attribute`: Add custom attributes to messages (key=value format)
- `--ordering-key`: Specify ordering key for ordered delivery

## Best Practices Demonstrated

1. **Resource Cleanup**: Always delete test resources to avoid unnecessary charges
2. **Naming Conventions**: Use descriptive names for topics and subscriptions
3. **Batch Operations**: Use flags for efficient bulk operations
4. **Verification**: Always list resources after creation/deletion to verify operations

## Relevant Documentation & Resources

- [Google Cloud Pub/Sub Documentation](https://cloud.google.com/pubsub/docs)
- [gcloud pubsub Command Reference](https://cloud.google.com/sdk/gcloud/reference/pubsub)
- [Pub/Sub Best Practices Guide](https://cloud.google.com/pubsub/docs/best-practices)
- [Pub/Sub Quotas and Limits](https://cloud.google.com/pubsub/quotas)
- [Cloud Shell Documentation](https://cloud.google.com/shell/docs)

## Troubleshooting Tips

### Common Issues and Solutions

1. **"Permission denied" errors**: Ensure you're using the correct project and have appropriate IAM roles
2. **Messages not appearing**: Wait a moment after publishing; Pub/Sub has eventual consistency
3. **Subscription not found**: Verify subscription name and ensure it's attached to the correct topic
4. **No messages in pull**: Check if messages have been acknowledged by previous pulls

### Verification Commands
```bash
# Check current project
gcloud config list project

# Verify authentication
gcloud auth list

# List all resources to verify state
gcloud pubsub topics list
gcloud pubsub subscriptions list
```

## Next Steps & Advanced Topics

After mastering these fundamentals, consider exploring:

1. **Push Subscriptions**: Configure webhook endpoints for automatic message delivery
2. **Message Filtering**: Create subscriptions with filters for specific message types
3. **Dead Letter Topics**: Handle undeliverable messages
4. **Ordering**: Implement ordered message delivery
5. **Schema Management**: Define and enforce message schemas
6. **Monitoring**: Set up Cloud Monitoring for Pub/Sub metrics

---
