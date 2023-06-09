# What is a transparency card?
An AI system includes not only the technology, but also the people who will use it, the people who will be affected by it, and the environment in which it is deployed. Creating a system that is fit for purpose requires an understanding of how the technology works, its capabilities and limitations, and how to achieve the best performance. Advanced Analytica's Transparency Cards are intended to help indviduals understand how AI technology works, the choices AI system owners make that influence system performance and behaviour, and the importance of thinking about the system as a whole, including the technology, the people, the environment and the law. 

We help our client draft Transparency Cards when developing, deploying and operating their AI systems, so they can be shared with the people who will use or be affected by their AI systems.

Advanced Analytica's Transparency Cards are part of a broader effort for AI Governance to put the ICO's AI principles into practice. To find out more, see the AI Governance Hub.

## Introduction to Named Entity Recognition and Personal Identifiable Data (PID)
Natural Langauge Processing frameworks support named entity recognition to identify and categorise information in your text. These include general entities such as Product and Event and Personal Identifiable Data (PID) entities. A wide variety of personal entities such as names, organisations, addresses, phone numbers, financial account numbers or codes and government and country or region specific identification numbers can be recognised. A subset of these personal entities is protected health information (PHI). In addition, the PII recognition supports the ability to specify specific entity categories you want in the response and redact PII entities in the response. The PII entities will be replaced by asterisks in the redactedText property of the response.

Read example NER request and example response to see how to send text to the service and what to expect back.

## Use cases

Where we need to recognise categories of named entities:

- **Enhance search capabilities** - Building knowledge graphs based on entities detected in documents to enhance document search.

- **Enhance or automate business processes** - For example, when reviewing insurance claims, recognized entities like name and location could be highlighted to facilitate the review. Or a support ticket could be generated with a customer's name and company automatically from an email.

Where we need to recognise categories of PID entities for specific reasons:

- **Apply sensitivity labels** - Based on the results from the PID service, a public sensitivity label must/should/could be applied to documents where no PID entities are detected. For documents where UK addresses and phone numbers are recognised, a confidential label must/should/could be applied. A highly confidential label might be used for documents where bank account numbers are recognised.

- **Redact some categories of personal information from documents to protect privacy** - Where customer contact records are accessible to first line support representatives, the company must/should/could redact unnecessary customer's personal information from customer history to preserve the customer's privacy.

- **Redact personal information in order to reduce unconscious bias** - For example, during a company's resume review process, they may want to block name, address and phone number to help reduce unconscious gender or other biases.

- **Replace personal information in source data for machine learning to reduce unfairness** – For example, if we want to remove names that might reveal gender when training a machine learning model, we use the service to identify them and replace them with generic placeholders for model training.

## Considerations when choosing a use case
### We do not use

- **PID only**  - We do not use for automatic redaction or information classification scenarios – Any scenario where failures to redact personal information could expose individuals to the risk of identity theft and physical or psychological harms must include meaningful human review.

- **NER and PII**  - Do not use for scenarios that use personal information for a purpose that consent was not obtained for - For example, a company has resumes from past job applicants. The applicants did not give their consent to be contacted for promotional events when they submitted their resumes. Based on this scenario, both NER and PII services should not be used to identify contact information for the purpose of inviting the past applicants to a trade show.

- **NER and PII**  - Customers are prohibited from using of this service to harvest personal information from publicly available content without consent from person(s) whom are the subject of the personal information.

- **NER and PII**  - Do not use for scenarios that replace personal information in text with the intent to mislead people.
Characteristics and limitations

Depending on each scenario, input data and the entities we need extract can experience different levels of performance. The following sections are designed to help you understand key concepts about performance as they apply to using the Azure Cognitive Service for language NER and PII services.

## Understanding and measuring performance of NER
Since both false positive and false negative errors can occur, it is important to understand how both types of errors might affect our overall system. With Named Entity Recognition (NER), a false positive occurs when an entity is not present in the text, but is recognised and returned by the system. A false negative is when an entity is present in the text, but is not recognised and returned by the system.

## Understanding performance for PID
In redaction scenarios, for example, false negatives could lead to personal information leakage. For redaction scenarios, we consider a process for human review to account for this type of error. For sensitivity label scenarios, both false positives and false negatives could lead to misclassification of documents. The audience may be unnecessarily limited for documents labelled as confidential where a false positive occurred. PID could be leaked where a false negative occurred and a public label is applied.

We adjust the threshold for confidence score our system uses to tune the system. When it is more important to identify all potential instances of PID, we use a lower threshold. This means that you may get more false positives (non- PII data being recognized as PII entities), but fewer false negatives (PII entities not recognized as PII). If it is more important for your system to recognize only true PII data, you can use a higher threshold. Threshold values may not have consistent behavior across individual categories of PII entities. Therefore, it is critical that you test your system with real data it will process in production.

## System limitations and best practices for enhancing performance
We make sure we understand all the entity categories for NER and PID that can be recognised by the system. Depending on each scenario, or data may include other information that could be considered personal but is not covered by the categories the service currently supports.

Context is important for all entity categories to be correctly recognized by the system, as it often is for humans to recognize an entity. For example, without context a ten-digit number is just a number. However, given context like "You can reach me at my office phone number 2345678901," both the system and a human can recognize the ten-digit number as a phone number. Always include context when sending text to the system to obtain the best possible performance.

Person names in particular require linguistic context. Send as much context as possible for better person name detection.

For conversational data, consider sending more than a single turn in the conversation to ensure higher likelihood that the required context is included with the actual entities.
In the following conversation, if you send a single row at a time, the passport number will not have any context associated with it and the EU Passport Number PII category will not be recognized.

Hi, how can I help you today?
I want to renew my passport
Sure, what is your current passport number?
Its 123456789, thanks.

However, if you send the whole conversation it will be recognized because the context is included.

Sometimes multiple entity categories can be recognized for the same entity. If we take the previous example:

Hi, how can I help you today?
I want to renew my passport
Sure, what is your current passport number?
Its 123456789, thanks.

Several different countries have the same format for passport numbers, so several different specific entity categories may be recognized. In some cases, using the highest confidence score may not be sufficient to choose the right entity class. If your scenario depends on the specific entity category being recognized, you may need to disambiguate the result elsewhere in your system either through a human review or additional validation code. Thorough testing on real life data can help you identify if you're likely to see multiple entity categories for recognized for your scenario.

Not all entity categories are supported in all languages for both NER and PII. Be sure to check the entity type article for the entities in the language you want to detect.

Many international PII entities are supported. By default, the entity categories returned are those that match the language code sent with the API call. If you expect entities from locales other than the one specified, you will need to specify them with the piiCategories parameter. Learn more about how to specify what your response will include in the API reference. Learn more about the categories supported for each locale in the named entity types documentation.

In PII redaction scenarios, if you are using the version of the API that includes the optional parameter piiCategories, it is important that you consider all the PII categories that could be present in your text. If you are redacting only specific entity categories or the default entity categories for a specific locale, other PII entity categories that unexpectedly appear in your text will be leaked. For example, if you have sent the EN-US locale and not specified any optional PII categories and a German Driver's License Number is present in your text, it will be leaked. To prevent this you would need to specify the German Driver's License Number category in the piiCategories parameter. In addition, if you have specified one or more categories using the piiCategories parameter for the specified locale, be aware that those are the only categories that would be redacted. For example, if you have sent the EN-US locale and have specified U.S. Social Security Number (SSN) as the PII category for redaction, then any other EN-US categories such as U.S. Driver's License Number or U.S. Passport Number would be leaked if they appear in the input text.

Since the PII service returns PII categories that match the language code in the call, consider verifying the language the input text is in if you're not sure what language or locale it will be. You can use the Language Detection feature to do this.

The PII service only takes text as an input. If you are redacting information from documents in other formats, make sure to carefully test your redaction code to ensure identified entities are not accidentally leaked.

