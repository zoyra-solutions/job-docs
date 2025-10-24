# Employer User Guide

## Welcome to Recruitment Platform

This guide will help you get started with using the Recruitment Platform as an employer. You'll learn how to post jobs, manage applications, track commissions, and optimize your recruitment process.

## Getting Started

### 1. Account Setup

1. **Sign Up**: Create your company account at [platform-url]/register
2. **Verify Email**: Check your email and click the verification link
3. **Complete Profile**: Add your company details, logo, and contact information
4. **Set Up Payment Method**: Connect your bank account for commission payments

### 2. Creating Your First Job Posting

#### Step 1: Define Job Details
- **Job Title**: Clear, specific title (e.g., "Senior React Developer")
- **Description**: Detailed job requirements, responsibilities, and benefits
- **Location**: Specific city/district in Vietnam
- **Job Type**: Full-time, Part-time, Contract, etc.
- **Salary Range**: Set competitive salary range in VND

#### Step 2: Set Requirements
- **Skills Required**: List technical and soft skills needed
- **Experience Level**: Specify years of experience required
- **Education**: Minimum education level if required
- **Language Requirements**: Specify language proficiency

#### Step 3: Configure Commission Rules
Choose how to incentivize recruiters:

**Fixed Amount**: Pay a fixed commission per successful hire
```json
{
  "ruleType": "fixed",
  "amount": 2000000  // 2 million VND per hire
}
```

**Percentage**: Pay percentage of first month's salary
```json
{
  "ruleType": "percentage",
  "percentage": 15  // 15% of first month's salary
}
```

**Staged Payment**: Pay in installments
```json
{
  "ruleType": "staged",
  "stages": [
    {"milestone": "contract_signed", "percentage": 50},
    {"milestone": "3_months", "percentage": 50}
  ]
}
```

#### Step 4: Set Payment Policy
- **Post-Paid**: Pay commissions after candidate starts working
- **Escrow**: Deposit commission amount upfront for faster payouts

### 3. Managing Applications

#### Application Pipeline
Your job postings will have a visual pipeline:

1. **Applied** → New applications from recruiters
2. **Shortlisted** → Candidates you're interested in
3. **Interviewed** → Candidates you've interviewed
4. **Offered** → Candidates who've received job offers
5. **Hired** → Successfully hired candidates

#### Reviewing Candidates
- **CV Review**: Examine uploaded CVs and documents
- **Skill Assessment**: Check if skills match requirements
- **Background Check**: Verify education and experience claims
- **Reference Check**: Contact previous employers if needed

#### Interview Management
- **Schedule Interviews**: Set up phone, video, or in-person interviews
- **Track Progress**: Update interview stages and results
- **Add Notes**: Keep detailed notes on each candidate
- **Rate Candidates**: Use star ratings for quick reference

### 4. Commission Management

#### Automatic Commission Calculation
The platform automatically calculates commissions when:
- Candidates pass interviews ✅
- Employment contracts are signed ✅
- Staged milestones are reached ✅

#### Commission Dashboard
Monitor your commission expenses:
- **Pending Commissions**: Awaiting payment
- **Paid Commissions**: Successfully processed
- **Disputed Commissions**: Under review
- **Total Spent**: Monthly/yearly commission totals

#### Payment Processing
- **Automatic Payments**: Set up auto-payment for trusted recruiters
- **Manual Review**: Review and approve each commission
- **Bulk Payments**: Process multiple commissions at once
- **Payment History**: Track all commission payments

### 5. Performance Analytics

#### Key Metrics to Track
- **Time to Hire**: Average days from posting to hiring
- **Cost per Hire**: Total recruitment cost divided by hires
- **Application Quality**: Percentage of qualified applications
- **Recruiter Performance**: Track which recruiters send best candidates

#### Dashboard Insights
- **Conversion Funnel**: See where candidates drop off
- **Source Analysis**: Which channels bring best candidates
- **Seasonal Trends**: Identify hiring patterns
- **Budget Tracking**: Monitor recruitment spending

### 6. Best Practices

#### Writing Effective Job Posts
- **Clear Title**: Use specific job titles (avoid "Developer")
- **Detailed Description**: Include day-to-day responsibilities
- **Competitive Salary**: Research market rates for your area
- **Company Culture**: Highlight what makes your company special
- **Growth Opportunities**: Mention career development paths

#### Optimizing Recruitment Process
- **Fast Response**: Respond to applications within 24 hours
- **Clear Communication**: Keep candidates informed throughout process
- **Structured Interviews**: Use consistent interview questions
- **Feedback Loop**: Provide constructive feedback to recruiters

#### Building Recruiter Relationships
- **Fair Commission Rates**: Offer competitive commission rates
- **Clear Requirements**: Provide detailed job specifications
- **Regular Communication**: Keep recruiters updated on progress
- **Performance Recognition**: Acknowledge top-performing recruiters

## Advanced Features

### AI-Powered Matching
- **Smart Suggestions**: Get AI-recommended candidates for your jobs
- **CV Parsing**: Automatically extract candidate information
- **Skill Matching**: Find candidates with exact skill matches
- **Location Optimization**: Prioritize local candidates

### Real-Time Collaboration
- **Live Chat**: Communicate with recruiters instantly
- **Status Updates**: Get notified of application changes
- **Document Sharing**: Share contracts and documents securely
- **Video Calls**: Conduct interviews directly through platform

### Mobile App Access
- **On-the-Go Management**: Manage jobs from your phone
- **Push Notifications**: Get instant alerts for new applications
- **Offline Access**: Review applications without internet
- **QR Code Sharing**: Share job postings via QR codes

## Troubleshooting

### Common Issues

**Applications Not Coming In**
- Check if job is published (not in draft mode)
- Review commission rates - may be too low
- Ensure job requirements aren't too restrictive
- Promote job posting to increase visibility

**High Candidate Drop-off**
- Review interview process for bottlenecks
- Check if salary expectations are realistic
- Ensure clear communication throughout process
- Consider candidate experience improvements

**Commission Disputes**
- Keep detailed records of candidate performance
- Document all communication with recruiters
- Use platform's dispute resolution process
- Set clear expectations upfront

## Support

### Getting Help
- **Help Center**: [platform-url]/help
- **Live Chat**: Available 9 AM - 6 PM (GMT+7)
- **Email Support**: support@yourplatform.com
- **Phone Support**: +84 xxx xxx xxxx

### Training Resources
- **Video Tutorials**: Step-by-step guides for all features
- **Webinars**: Weekly training sessions
- **Documentation**: Comprehensive user guides
- **Community Forum**: Connect with other employers

## Success Tips

### Maximize Platform Benefits
1. **Complete Your Profile**: Detailed company profiles get more attention
2. **Use AI Features**: Leverage AI matching for better candidates
3. **Build Relationships**: Develop long-term partnerships with top recruiters
4. **Track Metrics**: Use analytics to optimize your recruitment strategy
5. **Stay Active**: Regular platform usage improves visibility

### Scale Your Hiring
1. **Template Jobs**: Create templates for frequently posted positions
2. **Recruiter Network**: Build a network of reliable recruiters
3. **Process Automation**: Use platform features to automate repetitive tasks
4. **Data-Driven Decisions**: Use analytics to improve hiring outcomes

---

**Happy Hiring!** 🎯

For additional support, visit our Help Center or contact our support team.