# Styling

1. [General information](#general)
2. [Theme change](#theme-change)
3. [Custom theme change](#custom-theme-change)
4. [Font change](#font-change)
5. [Terminal theme change](#terminal-theme)
6. [Terminal font change](#terminal-font)

## General information

1. **Personalization**: IDE styling allows developers to customize the appearance of their coding environment to match their preferences and workflow. Everyone has different tastes, and having a personalized theme can create a more enjoyable coding experience.
    
2. **Reduced Eye Strain**: A well-designed theme with suitable colors and contrast can reduce eye strain and fatigue during long coding sessions. Developers can choose color schemes that are easier on their eyes, making it more comfortable to work for extended periods.
    
3. **Improved Readability**: A well-chosen theme can improve code readability, making it easier to spot syntax errors, keywords, and different elements in the code. Clear distinction between different code elements can enhance code comprehension and navigation.
    
4. **Focus and Productivity**: A clean and visually appealing IDE can help developers stay focused and productive. By minimizing distractions and creating a pleasant work environment, developers can concentrate more on coding tasks.
    
5. **Identification and Error Detection**: IDE styling allows developers to set distinct colors for various code elements like comments, strings, functions, etc. This makes it easier to identify different parts of the code and quickly spot errors and potential issues.
    
6. **Consistency**: IDE styling can be part of a developer's standard setup, ensuring a consistent and familiar coding environment across different machines and projects. This can aid in quickly adapting to new development tasks and projects.
    
7. **Branding and Aesthetics**: For developers working in a team or showcasing their work to others, a consistent and well-designed IDE theme can contribute to the overall aesthetics and branding of their projects or organization.
    
8. **Motivation and Inspiration**: A visually appealing IDE with a favorite theme can be motivating and inspiring for developers. It can create a sense of ownership and pride in their coding environment.
    
Overall, IDE styling is not just about aesthetics but also about optimizing the development experience, reducing visual strain, and creating a coding environment that suits an individual developer's preferences and needs. With the numerous themes and customization options available, developers can choose the one that best enhances their productivity and enjoyment while coding.

In this cookbook chapter we pay attention to installation of a custom theme and fonts for editor and for terminal. It could help you to improve your efficiency as a developer and improve code readability.

## Theme change

1. Open VS Code: Launch Visual Studio Code on your computer.
2. Access the Command Palette: Use the keyboard shortcut `Ctrl+Shift+P` (Windows/Linux) or `Cmd+Shift+P` (Mac) to open the Command Palette.
3. Search for "Preferences: Color Theme": In the Command Palette, start typing "Preferences: Color Theme," and it should suggest the command as you type. Once you see it, select the option to open the Color Theme menu.
4. Choose a new theme: In the Color Theme menu, you'll see a list of available themes. Use the arrow keys to navigate through the list, and you'll see a live preview of each theme as you select it.
5. Apply the selected theme: Once you find a theme you like, press `Enter` to apply it. VS Code will immediately switch to the new theme.

If you have installed additional themes from the VS Code Marketplace, they will also appear in the Color Theme menu for you to choose from.

Additionally, you can also access the Color Theme menu through the Settings:

1. Open the Settings: Click on the gear icon (`⚙️`) in the lower-left corner of the VS Code window, and select "Settings" from the menu. Alternatively, you can use the keyboard shortcut `Ctrl+,` (Windows/Linux) or `Cmd+,` (Mac) to open Settings.
2. Navigate to Color Theme: In the Settings, use the search bar to look for "Color Theme." Click on the dropdown menu below "Color Theme" to select your desired theme.
3. Apply the selected theme: Once you choose the theme, the editor will immediately update to reflect the new theme.

## Custom theme change

Install an extension with the custom theme. E.g., we want to update our current theme to the Night Wolf theme.
Navigate to this [extenstion](https://marketplace.visualstudio.com/items?itemName=MaoSantaella.night-wolf) in the extensions tab and install this extension to your IDE.

Repeat actions from the previous step and update your theme in the Settings tab.

## Font change

1. Open VS Code: Launch Visual Studio Code on your computer.
2. Access the Settings: Click on the gear icon (`⚙️`) in the lower-left corner of the VS Code window, and select "Settings" from the menu. Alternatively, you can use the keyboard shortcut `Ctrl+,` (Windows/Linux) or `Cmd+,` (Mac) to open Settings.
3. Search for "Font": In the Settings, you'll see a search bar at the top. Type "Font" in the search bar to filter the settings related to the font.
4. Look for "Editor: Font Family": In the search results, you should find the setting called "Editor: Font Family." This setting defines the font used in the editor.
5. Edit the Font Family setting: Click on the pencil icon to the left of "Editor: Font Family" to edit it. You can now enter the name of the font you want to use for the editor. You can also specify a fallback font in case the primary font is not available on your system.
For example, you can set the font to "Consolas, 'Courier New', monospace" to use the "Consolas" font if available, and if not, it will use "Courier New" as a fallback.
6. Save the changes: Once you've entered your desired font or font family, press `Enter` to save the changes.
7. Optional: You might need to restart VS Code for the font changes to take effect fully.
    
After following these steps, you should see the new font being used in the editor. Feel free to experiment with different fonts and font families until you find one that suits your preferences and enhances your coding experience.

## Terminal theme change

For changing your terminal theme we use [Oh My Posh](https://ohmyposh.dev/) app.
In order to use it, follow these steps:

1. Install this application on your computer. You can find instructions [here](https://ohmyposh.dev/docs/installation/macos).
2. Create a new file with your custom theme. You can find instructions [here](https://ohmyposh.dev/docs/installation/customize).
3. Choose your custom theme [here](https://ohmyposh.dev/docs/themes).
4. For example, you decided to use the `powerlevel10k_rainbow` theme.
5. Navigate to the themes [list](https://github.com/JanDeDobbeleer/oh-my-posh/tree/main/themes).
6. Find your theme [here](https://github.com/JanDeDobbeleer/oh-my-posh/blob/main/themes/powerlevel10k_rainbow.omp.json).
7. Copy JSON and paste to your configuration file which you created in previous steps.
8. Notice that your terminal has changed.

## Terminal font change

To change fonts you should complete installation of the Oh My Posh app on your computer.
Then follow this [guide](https://ohmyposh.dev/docs/installation/fonts) for the font installation.

For example, you decided to choose SpaceMono NF font.
In order to use it, follow these steps:

1. Download this font package from Nerd Fonts [here](https://github.com/ryanoasis/nerd-fonts/tree/master/patched-fonts/SpaceMono).
2. Install each of the font style to your computer (open each of the file and click the `Install` button).
3. Search for "Terminal Font Family": In Settings, you'll see a search bar at the top. Type "Terminal Font Family" in the search bar to filter the settings related to the terminal font.

Here is a screenshot with changing terminal font:
![terminal font changing](https://github.com/uptechteam/fe-cookbook/assets/13544983/d6f5ded9-beb0-47f5-bc4b-2bc62e552efa)

Also, you can modify it in VS Code `settings.json` file:
![terminal font changing](https://github.com/uptechteam/fe-cookbook/assets/13544983/9333505c-7e03-4fed-b41a-589c7080b5a0)

After changing theme and font of your terminal in VS Code, it will look like this one on the screenshot below:
![Terminal theme](https://github.com/uptechteam/fe-cookbook/assets/13544983/15e14a5a-a9b5-4595-acbc-0d332bd02ac3)
