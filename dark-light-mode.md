---
layout: default
title: Dark and Light Mode
nav_order: 7
---

# Dark and Light Mode

A responsive website should follow the theme preference set by the user on their operating system (OS) or web browser. Material UI allows us to switch between light and dark themes based on user preference by using the `ThemeProvider` component and a toggle switch. In this section, we will add a light/dark mode toggle to our navigation bar and we will learn how to leverage the `useMediaQuery` hook and the `prefers-color-scheme` media query to enable dark mode automatically by checking the user's preference in their OS or browser settings.

### Modify `theme.ts`
We had previously set the primary and secondary colors of our theme in the `config/theme.ts` file. However, when using dark mode in our website, we will need to desaturate these colors to maintain a satisfactory level of contrast between the elements on screen and improve readability. Therefore, we will need to remove the primary and secondary color definitions from `theme.ts` since these values will now be set programmatically depending on the theme mode used.

**Delete** the following lines from `theme.ts`:
```
palette: {
    primary: {
        main: "#7a003c"
    },
    secondary: {
        main: "#fdbf57"
    }
},
```


### Modify `_app.tsx`
Open `pages/_app.tsx` and add the following import statements:
```
import React from 'react'
import useMediaQuery from '@mui/material/useMediaQuery'
```

Create and export the `ColorModeContext` constant, which will allow us to read and modify the theme mode of our website from the navigation bar. The following code should be added before the `App` function declaration:
```
export const ColorModeContext = React.createContext({
    toggleColorMode: () => {},
})
```

Next, we will read the value of the media `prefers-color-scheme` query and save it as a Boolean constant whose value is true if the user currently has dark mode enabled on their system. We will also use the React state hook to create a `mode` constant with a `null` initial value and a `setColor` function that is used to update the `mode` constant.

Add the following lines of the code at the top of the `App` function:
```
const prefersDarkMode = useMediaQuery('(prefers-color-scheme: dark)')

const [mode, setMode] = React.useState<'light' | 'dark' | null>(null)
```

Replace the `const theme` declaration with the following updated declaration:
```
const theme = React.useMemo(
    () =>
        createTheme({
            ...themeOptions,
            palette: {
                mode:
                    mode == null
                        ? prefersDarkMode
                            ? 'dark'
                            : 'light'
                        : mode,
                primary: {
                    main:
                        mode == null
                            ? prefersDarkMode
                                ? '#86174E'
                                : '#7a003c'
                            : mode == 'light'
                                ? '#7a003c'
                                : '#86174E',
                },
                secondary: {
                    main:
                        mode == null
                            ? prefersDarkMode
                                ? '#FDC566'
                                : '#fdbf57'
                            : mode == 'light'
                                ? '#fdbf57'
                                : '#FDC566',
                },
            },
        }),
    [mode, prefersDarkMode]
);
```
This updated declaration utilizes the React `useMemo` hook to create and cache a theme value. The value of the `mode` attribute is determined by examining the the value of the `themeMode` constant we created earlier. The diagram below explains the conditional logic used to determine the value of `mode`:
![logic-mode](assets/img/logic-mode.png)
When using dark mode, the primary and secondary colors of our theme are desaturated to retain readability and enhance contrast. The values of these colors are now determined programmatically as shown in the diagrams below:
![logic-mode](assets/img/logic-primary.png)
![logic-mode](assets/img/logic-secondary.png)

We will now make use of the React `useMemo` hook to calculate and cache the value of the `colorMode` constant. Add the following lines of code **after** the `theme` declaration:
```
 const colorMode = React.useMemo(
        () => ({
            toggleColorMode: () => {
                setThemeMode(prevMode => (prevMode == null ? (theme.palette.mode === 'dark' ? 'light' : 'dark') : prevMode === 'light' ? 'dark' : 'light'))
            },
        }),
        [theme]
    )
```

Finally, wrap the returned elements of the `App` function with the `ColorModeContext.Provider`:
```
return <>
        <ColorModeContext.Provider value={colorMode}>
            <ThemeProvider theme={theme}>
              <Navbar />
              <CssBaseline />
              <Component {...pageProps} />
          </ThemeProvider>
        </ColorModeContext.Provider>
    </>
```
Your `_app.tsx` file should now look like this:
```
import React from 'react'
import type { AppProps } from 'next/app'
import CssBaseline from '@mui/material/CssBaseline'
import {createTheme, ThemeProvider, useTheme} from '@mui/material/styles'
import themeOptions from '@/config/theme'
import Navbar from "@/components/Navbar/Navbar";
import useMediaQuery from '@mui/material/useMediaQuery'

export const ColorModeContext = React.createContext({
    toggleColorMode: () => {},
})

export default function App({ Component, pageProps }: AppProps) {
    const prefersDarkMode = useMediaQuery('(prefers-color-scheme: dark)')

    const [themeMode, setThemeMode] = React.useState<'light' | 'dark' | null>(null)

    const theme = React.useMemo(
        () =>
            createTheme({
                ...themeOptions,
                palette: {
                    mode:
                        themeMode == null
                            ? prefersDarkMode
                                ? 'dark'
                                : 'light'
                            : themeMode,
                    primary: {
                        main:
                            themeMode == null
                                ? prefersDarkMode
                                    ? '#86174E'
                                    : '#7a003c'
                                : themeMode == 'light'
                                    ? '#7a003c'
                                    : '#86174E',
                    },
                    secondary: {
                        main:
                            themeMode == null
                                ? prefersDarkMode
                                    ? '#FDC566'
                                    : '#fdbf57'
                                : themeMode == 'light'
                                    ? '#fdbf57'
                                    : '#FDC566',
                    },
                },
            }),
        [themeMode, prefersDarkMode]
    )

    const colorMode = React.useMemo(
        () => ({
            toggleColorMode: () => {
                setThemeMode(prevMode => (prevMode == null ? (theme.palette.mode === 'dark' ? 'light' : 'dark') : prevMode === 'light' ? 'dark' : 'light'))
            },
        }),
        [theme]
    )

    return <>
        <ColorModeContext.Provider value={colorMode}>
            <ThemeProvider theme={theme}>
              <Navbar />
              <CssBaseline />
              <Component {...pageProps} />
          </ThemeProvider>
        </ColorModeContext.Provider>
    </>
}
```

### Add Light/Dark Mode Toggle to Navigation Bar
We will now add a responsive toggle to our navigation bar that allows the user to easily switch between dark and light mode. 
Start by adding the following import statement to `Navbar.tsx`:
```
import {useTheme} from '@mui/material/styles'
import Brightness4Icon from '@mui/icons-material/Brightness4'
import Brightness7Icon from '@mui/icons-material/Brightness7'
import {ColorModeContext} from '@/pages/_app'
```

Next, we will use the `useTheme` hook to access the theme variables in `Navbar.tsx` in addition to the `useContext` hook to grab the current context value of the `ColorModeContext` imported from `pages/_app.tsx`.

Add the following two lines to the top of the `Navbar` function:
```
const theme = useTheme()
const colorMode = React.useContext(ColorModeContext)
```

We will now add the toggle button to the navigation bar. Add the following lines of code before the `Box` containing the settings icon:
```
<Box sx={{paddingRight: 1}}>
    <Tooltip
        title={
            theme.palette.mode === 'dark'
                ? 'Switch to Light Mode'
                : 'Switch to Dark Mode'
        }
    >
        <MacIconNavButton
            sx={{ml: 1}}
            onClick={colorMode.toggleColorMode}
            color="inherit"
        >
            {theme.palette.mode === 'dark' ? (
                <Brightness7Icon />
            ) : (
                <Brightness4Icon />
            )}
        </MacIconNavButton>
    </Tooltip>
</Box>
```
We are a use the custom `MacIconNavButton` component that we created earlier and the MUI `Tooltip` component. Notice that the icon and the tooltip message displayed change according to the value of the current theme.

Go back to your browser and try switching mode by using the sundial icon in the navigation bar.
![dark-mode](assets/img/dark-mode.png)

![light-mode](assets/img/light-mode.png)

The chosen mode is automatically applied to all pages of the website. Try navigating to "Page1", "Page 1" and the "Settings" page. They will all automatically use the chosen mode and the text and UI element colors will change to maintain readibility.

Try enabling dark mode on your system and then navigate to the website in a new tab or reload the current tab. The website should automatically use dark mode when you first load the page. Note that you can still manually switch to light mode using the sundial icon.
