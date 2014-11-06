---
layout: howtom2devgde_chapters
title: Magento 2 Introduction to Modules
---
 
<h1 id="m2arch-module-intro">{{ page.title }}</h1>

<p><a href="{{ site.githuburl }}m2devgde/arch/____.md" target="_blank"><em>Help us improve this page</em></a>&nbsp;<img src="{{ site.baseurl }}common/images/newWindow.gif"/></p>

<h2 id="arch-modules-overview">Overview</h2>
Magento is an application that is built up of several different types of components: themes, modules, and libraries.

Themes and modules are the units of customization in Magento, themes for user experience and modules for business features. Both have a lifecycle allowing them to be installed, deleted, disabled, etc.
Themes

Themes allow you to customize the look-and-feel of the Magento application. Themes generally provide no new business features of their own, other than branding and user experience. They relate to each other through inheritance, allowing you to customize an existing theme by setting it as the parent and then defining any desired customizations in the new theme.

Themes are located in the /app/themes folder of a Magento installation, and each theme contains a theme.xml file, which hold the name and version of that theme, as well as the name of the parent theme, if any. Themes are also divided by area, allowing you define themes that customize either the storefront or admin sections of the Magento application independently.
Modules

Modules, on the other hand, encapsulate a particular business feature or set of features. They can relate to and depend on each other in a variety of ways, but should be as independent as possible to maximum flexibility when customizing a site. And while modules primarily define new business features, or customizations to existing ones, they also define a default user interface for those features, which can be customized by themes.

Modules live in the /app/modules folder of a Magento installation, in a directory with the following PSR-0 compliant format: /app/modules/<vendor>/<module_name>, e.g. the Customer module of Magento can be found at /app/modules/Magento/Customer. Inside of this folder, you will find all of the code and configuration related to this module, including the etc/module.xml file, which contains the name and version of the module, as well as any dependencies.
Libraries

Libraries consist of reusable logic that is often useful across multiple applications, including application frameworks. They don't provide any independent business features, but act as building blocks for modules and themes.

Libraries are placed in the /lib folder and are also organized by vendor to be PSR-0 compliant.


<h2 id="arch-modules-terms">Terms Used</h2>

Module

:	A logical group (that is, a directory containing blocks, controllers, helpers, models, and so on related to the specific feature or a widget). Module is a part of the application layer. A module is designed to work independently and not to intervene into work of other functionality.

Application layer

: Implements business logic. This layer is built on top of the framework layer, which defines the role of an application component in Magento, defines standards for the interactions among components, and implements system-level request and response objects and routing. Introducing layers means that business logic and system-level processes are pulled apart in application and framework layers correspondingly, so changes in the business logic will not influence sustainability of your store.

Modularity approach: Implies that every module encapsulates a feature and has minimum dependencies on other modules.

This section briefs about main tools for working with modules in Magento, introduces the modules decoupling details, and describes some modules you might need when adjusting and extending your online store.

<h2 id="AG-into-mods-specific">Specific Modules</h2>


Checkout Module

: The Multishipping feature and the Terms and Conditions feature were moved from Checkout module to separate modules. Thus, the third-part developer can disable these functionalies without influencing other parts of the application. Also, the dependencies between decoupled modules and other modules were eliminated or made more explicit.
Catalog Module

Several features were moved out from Catalog module to separate modules. For instance, configurable product, grouped product, and layered navigation were encapsulated in separate modules.
Sales Module

The recurring profile feature (also known as recurring payment) was encapsulated in a separate module. Thus, the third-party developer can disable the recurring profile (recurring payment) functionality without influencing other parts of the application. The dependencies between the recurring profile (recurring payment) module and other modules were eliminated or made more explicit.

Apart from decoupling of the recurring profile (recurring payment) feature, the elements related to the Google Checkout functionality were removed (due to its elimination by Google).

As a result of the refactoring, RecurringPayment abstract module was created. This module contains the classes and methods moved out from Sales module as well as relevant elements that ensure using the recurring profile (recurring payment) functionality by the payment methods.
Shipping Module

Shipping module was also affected by the decoupling changes. The shipping related operations were encapsulated in Shipping module and the shipping methods themselves were moved to separate modules (also known as carrier modules). To decrease dependencies between Shipping module and other modules, the abstract interface module - Carrier - was introduced. The shipping methods' modules interact (and have dependency connection) with Carrier module only. And the latter, in its turn, interacts with other modules within the application, for instance, Product, Sales, RMA, Core, Backend, and so on.

Thus, the carrier modules do not interact with the rest of the application directly and the third-party developer can disable any shipping method without influencing other parts of the application.
Payment Module

TBD
Exploring Newly Created and Decoupled Modules

To learn more about the newly created or decoupled modules in Magento, please follow the links

Backend module serves to adjust the admin panel of your store

Bundle module serves to handle the bundle product type

Catalog module serves to handle the product lists both in the admin panel and storefront.

CatalogInventory module helps to handle the product stock

Checkout module serves to customize the checking out process in the storefront

CheckoutAgreements module serves to inform a customer about conditions of purchase

Core module

ConfigurableProduct module serves to handle the configurable product type

Customer module serves to handle the customer accounts both in the admin panel and storefront.

DHL module serves to integrate the DHL shipping method.

Downloadable module serves to handle the downloadable product type

Email module serves to handle the email templates

FedEx module serves to integrate the FedEx shipping method.

GiftCard module serves to handle the gift card product type

GroupedProduct module serves to handle the grouped product type.

ImportExport module serves to handle the import and export data to/from Magento

Indexer module

LayeredNavigation module serves to handle the layered navigation in the storefront.

Multishipping module serves to handle the shipping to several addresses.

OfflinePayment module serves to handle the offline payment methods, such as Cash on Delivery, Purchase Order, Bank Transfer, and Check/Money Order.

OfflineShipping module serves to handle the offline shipping methods, such as: Free Shipping, Flat rate, Table Rates, Store Pickup.

Payment module helps to handle the payment methods.

RecurringPayment module serves to handle the recurring payments

Sales module helps to handle the orders, invoices, credit memos, and shipments both in the admin panel and storefront.

ScheduledImportExport module serves to arrange scheduled import and export of the data (for instance, product and customer information) to/from Magento.

Shipping module helps to handle the shipment related data and the shipping methods.

Store module

Tax module serves to handle taxes.

Translation module contains configurations, parsers, and scripts for creating translation tables.

UPS

USPS

Weee module serves to handle the fixed product taxes, in particular, calculate the Waste Electrical and Electronic Equipment Directive tax in the online store