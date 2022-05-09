---
title: Localization of API Portal
linkTitle: Localization
weight: 60
date: 2019-07-30T00:00:00.000Z
description: You can localize the API Portal user interface to use another
  language, or change time and date formats.
---
## Change API Portal language

You can change the API Portal UI texts from the default English to another language.

### Install a language

User interface strings are stored in language-specific resource files. To add these resource files for a new language, you must install the language.

1. In the Joomla! Admin Interface (JAI) top navigation bar, click **Extensions > Language(s)**.
2. Click **Install Languages**.
3. Find and select the language to install, and click **Install**.
4. After the installation is finished, click **Extensions > Language(s) > Installed** and ensure that **Site** is selected at the top of the page.
5. To set the installed language as the default language of your API Portal, click the star button in the row for that language.

   ![Install language](/Images/APIPortal/joomla_install_lang.png)

   {{< alert title="Tip" color="primary" >}}You can change the default language of JAI independently from your API Portal. Select **Administrator** on the **Languages: Installed** page and click the star button in the row for the required language. You can also select any of the installed languages when you log in to JAI.{{< /alert >}}

### Add API Portal UI strings to the installed language

API Portal UI strings are not included in the installed language resource files, so you must add the required strings for the new language.

The UI strings are stored in initialization (`.ini`) files. Each language has its own `.ini` file that control the UI text shown in API Portal. By default, the API Portal UI strings are included only in the English (`en-GB.com_apiportal.ini` file). To change the language shown in API Portal, you must have a translation of the English `.ini` file for that language.

### Add a translated UI string file

All **File upload** sections have a link from which you can download the English version of the file. After that, you just need to translate the strings and upload the new file. You must ensure that the extension of the file is `.ini`, otherwise the upload will fail.

To upload language files:

1. In JAI, click **Components > API Portal > Additional Settings**.
2. Enable **Upload language file toggle**. The following options are shown:

   * **Language**: Select the language for which you are uploading translations. All installed languages except English are listed.
   * **Language files context**: Select whether the uploaded language files are for the **Site** or **Administrator** part of API Portal.
   * **API Portal component language file**: The main language file for API Portal component.
   * **Overridden language file**: Some texts in API Portal, such as the messages on the login pages, are stored in different filed and you must change and upload them separately.
   * **`sys.ini` language file**: This options is shown only when **Administrator** is selected as a language files context. This language file contains translations for the administrator components menu drop-down, menu, and component parameters.
3. Click **Save & Close**.

To translate labels or values of custom properties, you must add a new row to the translation `.ini` file, with the desired `key/value` pair, and ensure that the value of the key is in capital letters. For example, to translate a custom property with label "Environment" to French, add the following line to the translation file: `ENVIRONMENT="Environnement"`.

## Provide API Portal in multiple languages

You can provide API Portal in multiple languages at the same time to cater for your API consumers from different countries. The API consumers can use language-specific URLs to access API Portal in their own language.

Unlike when localizing API Portal to a single language, you are not simply substituting the language with another one. Instead, when providing several language versions of your API Portal, you must duplicate the API Portal elements and content for each language in addition to changing the UI texts.

You do not have to install a language to Joomla! before configuring the required components. However, it might help you to find the correct details when configuring a language.

### Add a new content language and main menu

To add a new content language and main menu, follow these steps:

1. In the JAI left navigation bar, click **Language(s) > Content Languages**.
2. Click **New** to add a new content language.

   {{< alert title="Note" color="primary" >}}If you have already installed a new language, a corresponding content language will already exist for that language and you can skip steps 3 to 7, and continue from step 8.{{< /alert >}}
3. On the **Details** tab, enter the **Title** and **Title Native** for the new language. The titles can be the same.
4. Enter the Joomla! **Language Tag**. Ensure that you enter the tag in the correct format. You must use "`-`" instead of "`_`" (for example, `fr-FR`).
5. In **URL Language Code**, enter the language identifier to use in the language-specific URL (for example, `fr`). The identifier must be unique for each language.
6. Set **Status** to **Unpublished**, and click **Save & Close**. For more details on the parameters, see the tutorial for [Joomla! Language Switcher](https://docs.joomla.org/J3.x:Setup_a_Multilingual_Site/Language_Switcher).
7. In the JAI top navigation bar, click **Menus > Manage > Add New Menu**.
8. In **Title**, enter the title to be shown in drop-down lists (for example, `Main menu (fr)`).
9. On the **Menu Details** tab, in **Menu Type**, enter a system identifier for the new main menu (for example, `mainmenufr`).
10. Click **Save & Close**.

### Clone the main menu items

You must duplicate the API Portal main menu for each language.

1. In the JAI top navigation bar, click **Menus** and select the main menu to clone.
2. To clone all the menu items at once, click the Check All Items check box and click **Batch**.
3. In **Set Language**, select the correct content language.
4. In **To Move or Copy**, locate the correct main menu for the language and select **Add to this menu**.
5. Select **Copy** to make a copy of the menu items.
6. Click **Process**.
7. In the JAI top navigation bar, click **Menus** and select the main menu you cloned the menu items to.
8. To set the default home page, click the star in the row for the **Home** menu item.
9. To rename the cloned menu items, click each menu item to edit it, and change **Menu Title** to the correct translation (for example, `Home(2)` to `Accueil`).
10. Click **Save & Close**.
11. Repeat for each menu item.

### Duplicate the page template styles

You must also duplicate your page template styles for each language. By default, API Portal uses the Purity III template style.

1. In the JAI left navigation bar, click **Templates > Styles**.
2. Click your template style (for example, `purity_III - Default`) to open it.
3. Click the arrow next to the **Save** button at the top left of the window, and select **Save as Copy**.
4. Edit the **Style Name** to indicate the language (for example, `purity_III - fr`).
5. Select the respective language as the **Default**. This sets this template as the default for pages using the selected language.
6. Click the **Navigation** tab and change the **Menu** to the correct main menu for the language.
7. Click the **Assignment** tab and assign the menu items from the correct language to the template. To select or deselect all menu items, click the toggle button next to the main menu title.
8. Click **Save & Close**.

### Duplicate the homepage template style

You must also duplicate your homepage template style for each language.

1. In the JAI left navigation bar, click **Templates > Styles**.
2. Click **apiportal-homepage** template style to open it.
3. Click the arrow next to the **Save** button at the top left of the window, and select **Save as Copy**.
4. Edit the **Style Name** to update the name of your new homepage.
5. Click the **Navigation** tab and change the **Menu** to the correct main menu for the language.
6. Click the **Assignment** tab and select only **Home** from the **Main menu** of the correct language.
7. Click **Save & Close**.

### Create homepage modules for the new language

On the Home page layout there are available positions where you can add modules. When installing a new language, it is a common practice to duplicate the **Home Page Banner** and the **Home tiles** modules, and translate them for the new language. To create a new module:

1. In the JAI top navigation bar, click **Extensions > Modules** and click **New**.
2. Select module type (for example, **Home Page Banner**).
3. Select **Position**  (for example, **api-home-banner**).
4. For the **Language**, select the new installed language.
5. On the **Menu assignment** tab, select **Only on the pages selected** from the **Module assignment** list.
6. Select the Home page of the new language from the **Menu selection** field.
7. Click **Save & Close**.

### Publish additional languages

1. Ensure that you have completed all the steps described in [Change API Portal language](#change-api-portal-language) for each language you want to publish.
2. In the JAI left navigation bar, click **Language(s) > Content Languages**.
3. Ensure that the **Status** for all the languages you want to publish is set to **Publish**.
4. In the JAI top navigation bar, click **Extensions > Plugins**, and ensure the **System - Language Filter** plugin is enabled.
5. Open your API Portal home page in a browser, and change the language code in the URL to one of the languages you have published. For example, change `https://<your API Portal URL>/en/` to `https://<your API Portal URL>/fr/`. You are redirected to the API Portal home page in that language.

## Provide a language switcher

You can provide API Portal in multiple languages, and enable your API consumers from different countries to change languages using a language switcher.

To enable a language switcher, you need to have a main menu for each language you are providing (including one for English), and an additional main menu for all languages. Similarly, you need to have template styles for each language (including one for English), and an additional template style for all languages.

### Install new languages

To install new languages, follow these steps:

1. In the JAI top navigation bar, click **Extensions > Language(s)**.
2. Click **Install Languages**.
3. Find and select the language to install, and click **Install**.
4. In the JAI left navigation bar, click **Language(s) > Content Languages**.
5. Set the **Status** of the new language to **Published**.

### Create main menus

To enable a language switcher, you need to have a main menu for each language you are providing (including one for English), and an additional main menu for all languages.

After following this process you will have the following main menus:

* Main menu - All
* Main menu - FR
* Main menu - EN

#### Create a main menu for all languages

First, create a main menu that will be the default main menu for all languages. This main menu will have only one menu item.

1. In the JAI top navigation bar, click **Menus > Manage > Add New Menu**.
2. In **Title**, enter the title to be shown in drop-down lists (for example, `Main menu - All`).
3. On the **Menu Details** tab, in **Menu Type**, enter a system identifier for the new main menu (for example, `mainmenuall`).
4. Click **Save & Close**.
5. To add a menu item to this main menu, click **Menus > Main menu - All > Add New Menu Item**.
6. Click **Save & Close**.

   ![Add menu item for Main menu -All](/Images/APIPortal/jai_mainmenuall_home.png)

#### Change the language of the original main menu to English

Next, change the language of the original main menu items from All to English:

1. In the JAI top navigation bar, click **Menus** and select `Main Menu`.
2. To change the language of all the menu items at once, click the Check All Items check box and click **Batch**.
3. In **Set Language**, select `English`.
4. Click **Process**.
5. In the JAI top navigation bar, click **Menus** and select `Main Menu`.
6. To set the default home page, click the star in the row for the **Home** menu item.

#### Create a main menu for the new language

Next, create a main menu for each new language and clone the menu items from the main menu. Follow these steps:

1. In the JAI top navigation bar, click **Menus > Manage > Add New Menu**.
2. In **Title**, enter the title to be shown in drop-down lists (for example, `Main menu - FR`).
3. On the **Menu Details** tab, in **Menu Type**, enter a system identifier for the new main menu (for example, `mainmenufr`).
4. Click **Save & Close**.
5. In the JAI top navigation bar, click **Menus** and select `Main Menu`.
6. To clone all the menu items at once, click the Check All Items check box and click **Batch**.
7. In **Set Language**, select the correct content language (for example, `French (FR)`).
8. In **To Move or Copy**, locate the correct main menu for the language and select **Add to this menu**.
9. Select **Copy** to make a copy of the menu items.
10. Click **Process**.
11. In the JAI top navigation bar, click **Menus** and select the main menu you cloned the menu items to (for example, `Main menu - FR`).
12. To set the default home page, click the star in the row for the **Home** menu item.

#### Rename the original main menu

Finally, rename the original main menu:

1. In the JAI top navigation bar, click **Menus > Manage**.
2. Select the row for the main menu and click **Edit**.
3. Change the **Title** to `Main menu - EN`.
4. Click **Save & Close**.

### Create page template styles

To enable a language switcher, you need to have template styles for each language you are providing (including one for English), and an additional template style for all languages.

By default, API Portal uses the Purity III template style. After following this process you will have the following template styles:

* purity_III - Default - English
* purity_III - Default - French
* purity_III - Default - All

#### Duplicate the template style for the new language

1. In the JAI left navigation bar, click **Templates > Styles**.
2. Click your template style (for example, `purity_III - Default`) to open it.
3. Click the arrow next to the **Save** button at the top left of the window, and select **Save as Copy**.
4. Edit the **Style Name** to indicate the language (for example, `purity_III - Default - French`).
5. Select the respective language as the **Default**. This sets this template as the default for pages using the selected language.
6. Click the **Navigation** tab, and change the **Menu** to the correct main menu for the language.
7. Click the **Assignment** tab, and assign the menu items from the correct language to the template. To select or deselect all menu items, click the toggle button next to the main menu title.
8. Click **Save & Close**.

#### Duplicate the template style for all languages

Next, repeat steps 2 to 8 of [Duplicate the template style for the new language](#duplicate-the-template-style-for-the-new-language) to duplicate the template style for all languages (for example , `purity_III - Default - All`).

#### Update the original template style for English

Finally, edit the original template style for the English language:

1. Click your template style (for example, `purity_III - Default`) to open it.
2. Edit the **Style Name** to indicate the English language (for example, `purity_III - Default - English`).
3. Select English as the **Default**. This sets this template as the default for pages using the English language.
4. Click the **Navigation** tab, and change the **Menu** to `Main menu - EN`.
5. Click the **Assignment** tab, and assign the menu items from the English language to the template. To select or deselect all menu items, click the toggle button next to the main menu title.
6. Click **Save & Close**.

### Enable the language switcher

To enable the language switcher, complete the following steps:

1. In the JAI top navigation bar, click **Extensions > Modules** and click **New**.
2. Click **Language Switcher**.
3. Enter a **Title** and for **Position** select `Purity iii > Footer 1`.
4. Click **Save & Close**.
5. In the JAI top navigation bar, click **Extensions > Plugins**, and ensure the **System - Language Filter** plugin is enabled.
6. Click the **System - Language Filter** plugin to open it.
7. Set **Automatic Language Change** to `No`.
8. Click **Save & Close**.

The language switcher is now available on the footer of each API Portal page. For example:

![Language switcher on footer](/Images/APIPortal/lang_switcher_example.png)