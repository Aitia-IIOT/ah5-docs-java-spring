# Translation Providers

## Interface Translation Providers

The interface translation providers are individual [application systems](../../help/definitions.md#application-system) that are capable

- to cooperate with the [Translation Manager Support System](../../support_systems/translation_manager.md) by providing the **interfaceBridgeManagement** service and
- to cooperate with data model translation providers by consuming the **dataModelTranslation** service

in order to create a translation bridge between consumer and provider systems.

| Public Interface Translation Providers |
| ---------------------------- |
| [InterfaceTranslatorToGenericHTTP:octicons-link-external-16:](https://github.com/Aitia-IIOT/ah5-app-aitia-interface-translator-to-generic-http-java-spring) |

## Data Model Translation Providers

The data model translation providers are individual [application systems](../../help/definitions.md#application-system) that are capable to cooperate with interface translation providers by providing the **dataModelTranslation** service in order to translate data model _"A"_ to data model _"B"_.

| Public Data Model Translation Providers |
| ---------------------------- |
| [SemanticAITranslator:octicons-link-external-16:](https://github.com/Aitia-IIOT/ah5-app-aitia-datamodel-translator-python-wrapper-java-spring) |