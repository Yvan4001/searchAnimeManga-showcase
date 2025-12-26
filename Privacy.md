# Privacy Policy

**Last updated: December 26, 2025**

This Privacy Policy describes how **Search Anime Manga** ("we", "our", or "us") collects, uses, and protects your information when you use our Discord bot service.

## Data Controller

**Data Controller:** Yvan‑Simon – Sole Proprietorship  
**Address:** 150 Grande rue de Saint Clair, 69300 Caluire et Cuire, France  
**Contact:** contact@sivagames.com  
**Privacy Contact:** contact@sivagames.com

## Overview

We are committed to protecting your privacy and being transparent about how we handle your data. This bot is designed with privacy in mind, collecting only the minimal data necessary to provide our news aggregation service.

## What Data We Collect

### Data Collected Automatically
When you use our Discord bot, we collect only the minimal data needed to operate the service:

- **Discord User ID**: Your unique Discord identifier (used to execute commands and map subscriptions)
- **Discord Server ID**: The unique identifier of the Discord server where the bot is used
- **Discord Channel ID**: The unique identifier of the channel where commands are executed
- **Command Usage Data (aggregated)**: Non‑identifying usage metrics to improve service quality
- **Subscription Status & Metadata**: For premium users we store subscription state and minimal metadata (e.g., a subscription status entry and non-sensitive Stripe identifiers) strictly to manage access

### Data We Do NOT Collect
We do NOT collect or retain sensitive personal or financial data. In particular, we do not collect:

- Your Discord username or display name
- Message content (except explicit commands directed to the bot)
- Personal information beyond the limited identifiers listed above
- Voice data or media files
- Location information
- Raw payment credentials (e.g., card numbers, bank account details)

Note: To support premium features we may retain non-sensitive payment metadata (subscription status and non-sensitive Stripe identifiers). We do not store full payment details; all payment processing is performed by Stripe.

## How We Use Your Data

### Primary Purposes
We use the collected data only for necessary and specific purposes:

1. **Provide Bot Functionality**: Execute commands and deliver content requested by users
2. **Channel Management**: Create and manage dedicated channels when requested
3. **Subscription Management**: Create Stripe Checkout sessions linked to Discord IDs, update subscription status, and assign or revoke premium access automatically
4. **Reconciliation & Reliability**: Run daily reconciliation between Stripe subscription state and our `subscriptionStatus` entries to ensure billing consistency and to correct missed webhook events
5. **Service Optimization**: Improve bot performance and user experience using aggregated metrics
6. **Rate Limiting & Abuse Prevention**: Prevent abuse and ensure fair usage for all users

### Legal Basis for Processing
Our processing is based on:
- **Contract Performance**: Necessary to provide the bot service you've requested
- **Legitimate Interest**: Ensuring proper operation and preventing abuse

## Data Sharing and Third Parties

### External Services
Our bot interacts with a small set of third-party services that help provide functionality and process minimal operational data:

- **Discord API**: To send messages, manage channels, and receive command events.
- **Jikan (MyAnimeList) API**: Primary source for anime and manga data used to fulfill search requests.
- **AniList GraphQL API**: Secondary data source and rate-limit fallback to ensure continuity when primary APIs are constrained.
- **Stripe**: Payment processing for premium subscriptions (we do not store raw payment credentials; Stripe handles card data and secure processing).
- **MongoDB Atlas**: Persistent storage for minimal identifiers and `subscriptionStatus` entries.
- **Redis**: Caching layer for popular requests to reduce latency and preserve API quotas (TTL = 10 min).
- **Azure Functions & Azure Key Vault**: Serverless compute for webhook handling and scheduled reconciliation jobs; Key Vault stores secrets securely.

Each third party has its own privacy policy; consult those services for details about how they process data.

### Data Sharing Policy
We do NOT:
- Sell your data to third parties
- Share your data for marketing purposes
- Provide your data to advertisers
- Use your data for profiling or automated decision-making

## Data Retention

### Retention Period
- **Active Servers**: Identifiers and related data (including subscription status for premium users) are retained only while the bot remains in your Discord server or while your subscription is active.
- **Automatic Deletion**: All associated data is automatically deleted within **24 hours** after the bot leaves a server or after a subscription is terminated and reconciliation confirms no active subscription remains.
- **No Permanent Storage**: We do not maintain permanent records of user interactions beyond what is necessary for active service operation and billing reconciliation.

### Data Deletion Process
When the bot leaves a server or a subscription ends:
1. Associated Discord IDs and subscription status entries are marked for deletion
2. Automated cleanup processes remove the data within 24 hours after the bot leaves the server or after reconciliation confirms the subscription is terminated
3. No backup copies are retained beyond necessary operational backups (which are also deleted in accordance with this policy)

We run a daily reconciliation job (Azure Timer Trigger) to compare Stripe subscription states with our `subscriptionStatus` entries in MongoDB and correct any inconsistencies.

## Your Privacy Rights

Under GDPR and applicable privacy laws, you have the right to:

### Access Rights
- Request information about what personal data we hold about you
- Receive a copy of your personal data in a structured format

### Correction Rights
- Request correction of inaccurate personal data
- Request completion of incomplete personal data

### Deletion Rights
- Request deletion of your personal data ("right to be forgotten")
- Request immediate removal when the bot leaves your server

### Restriction Rights
- Request restriction of processing under certain circumstances
- Object to processing based on legitimate interests

### Portability Rights
- Request transfer of your data to another service provider
- Receive your data in a machine-readable format

## How to Exercise Your Rights

To exercise any of your privacy rights:

1. **Email us**: Send a request to contact@sivagames.com
2. **Include**: Your Discord User ID and specific request details
3. **Response Time**: We will respond within 30 days
4. **Verification**: We may request verification of your identity

## Data Security

### Security Measures
We implement appropriate technical and organizational measures:

- **Encryption in Transit**: Data transmission is encrypted using Discord's secure protocols and HTTPS when interacting with external APIs
- **Secrets Management**: All API keys and Stripe secrets are encrypted and stored in **Azure Key Vault**; secrets are never stored in plain text in the runtime environment
- **Webhook Validation**: Stripe webhook events are verified using Stripe's signature header to prevent fraudulent or spoofed events
- **Access Controls**: Limited access to data on a need-to-know basis
- **Regular Updates**: Bot software and dependencies are regularly updated for security
- **Monitoring & Audit**: Automated monitoring for unusual activity and logging for incident investigation

### Limitations
While we strive to protect your data, no internet transmission is 100% secure. We cannot guarantee absolute security of data transmitted to our service. Note that payment card numbers and other raw payment credentials are never stored by us; they are processed and stored only by Stripe.

## International Data Transfers

- **Server Location**: Our bot operates from servers within the European Union
- **Discord**: Data is processed through Discord's infrastructure (see Discord's Privacy Policy)
- **Safeguards**: Appropriate safeguards are in place for any data transfers outside the EU

## Children's Privacy

### Age Requirements
- **Minimum Age**: 13 years old (Discord's minimum age requirement)
- **Parental Consent**: Users under 18 should obtain parental consent
- **Child Protection**: We do not knowingly collect data from children under 13

If we become aware that we have collected data from a child under 13, we will delete it immediately.

## Rate Limiting and Abuse Prevention

### Fair Usage Policy
- **Rate Limits**: Maximum 30 requests per minute to ensure fair usage
- **Abuse Detection**: Automated systems monitor for unusual usage patterns
- **Temporary Restrictions**: May be applied for excessive usage

## Content and Copyright

### Anime & Manga Content
- **Sources**: Anime and manga data is fetched from MyAnimeList (via the Jikan API) and AniList (GraphQL). We link to original source pages when available.
- **Ownership**: Descriptions, images, and metadata remain the property of their respective rights holders (studios, publishers, authors).
- **Usage & Fair Use**: Content is presented for informational and community use with appropriate attribution; we do not use content for commercial redistribution beyond the bot's service.
- **No Modification**: We do not claim or materially alter original content; outputs may be formatted or truncated for display purposes.
- **Copyright Concerns**: If you believe content is used improperly, contact `contact@sivagames.com` and we will investigate and take appropriate action.

## Cookies and Tracking

### No Cookies
Our Discord bot does not use cookies or similar tracking technologies.

### Discord Tracking
Any tracking is handled by Discord according to their privacy policy.

## Updates to This Privacy Policy

### Notification of Changes
- **Material Changes**: We will provide at least 30 days' notice
- **Minor Updates**: Posted immediately with updated date
- **Continued Use**: Constitutes acceptance of updated policy

### Version History
You can request previous versions of this privacy policy by contacting us.

## Compliance

### Regulatory Compliance
This privacy policy complies with:
- **GDPR** (General Data Protection Regulation)
- **French Data Protection Laws**
- **Discord's Developer Terms of Service**

### Regular Reviews
We regularly review and update our privacy practices to ensure continued compliance.

## Contact Information

### Privacy Inquiries
For any privacy-related questions or requests:

- **Email**: support@sivagames.com
- **Subject Line**: "Privacy Policy - Anime Manga Search Bot"
- **Response Time**: Within 30 days

### Data Protection Authority
If you're not satisfied with our response, you can contact your local data protection authority.

### Emergency Contact
For urgent privacy concerns, contact us immediately at security@sivagames.com with "URGENT PRIVACY" in the subject line.

## Additional Information

### Open Source
This bot's source code may be available for review (subject to licensing terms).

### Community
Join our support community for updates and assistance (if applicable).

### Service Status
Check our service status and announcements through our official channels.

---

**Effective Date**: December 26, 2025  
**Document Version**: 1.0  
**Review Date**: March 1, 2026

*This privacy policy is written in English. In case of conflicts between translations, the English version prevails.*