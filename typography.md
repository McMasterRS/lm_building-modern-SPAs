---
layout: default
title: Typography
nav_order: 5
---

# Typography

McMaster recommends the use of the Roboto family of fonts on all websites associated with the university. Roboto is a neo-grotesque sans-serif typeface family developed by Google. There are multiple variants of the Roboto font that are used for different headlines, subheads and body text when designing a new webpage. In this section, we will learn how to use the MUI Typography component to define different heading styles.

### Create `theme.ts`
In the root directory of your project, create a new directory named `config`. You can create the folder using the command line (`mkdir config`) or the GUI.
Navigate to the newly created `config` directory, and create a new file called `theme.ts` in this directory.

Add the following code snippet to  `theme.ts`:
```
import {Roboto, Roboto_Condensed} from "next/font/google";

const roboto = Roboto({
    weight: ['300', '700'],
    style: ['normal', 'italic'],
    subsets: ['latin'],
    display: 'swap',
})

const roboto_condensed = Roboto_Condensed({
    weight: ['400', '700'],
    style: ['normal', 'italic'],
    subsets: ['latin'],
    display: 'swap',
})

declare module '@mui/material/Typography' {
    interface TypographyPropsVariantOverrides {
        settingTitle: true;
    }
}

const themeOptions = {
    typography: {
        h1: {
            fontFamily: roboto_condensed.style.fontFamily,
            fontSize: '50pt',
        },
        h2: {
            fontFamily: roboto_condensed.style.fontFamily,
            fontSize: '28pt',
            fontWeight: 400,
        },
        h3: {
            fontFamily: roboto_condensed.style.fontFamily,
            fontSize: '20pt',
        },
        h4: {
            fontFamily: roboto.style.fontFamily,
            fontSize: '13pt',
            fontWeight: 900,
        },
        button: {
            fontFamily: roboto_condensed.style.fontFamily,
            fontWeight: 700,
        },
        settingTitle: {
            fontFamily: roboto_condensed.style.fontFamily,
            fontSize: '15pt',
        },
    },
}

export default themeOptions
```

In this code snippet, we start by importing the Roboto font variants that we need using the `next/font/google` package. We then define the different typography variants that can be used in our application. The heading styles conform to the McMaster Digital Brand Standards. The `button` and `settingTitle` typographies define the font style to use for text located in buttons and setting titles respectively. We will cover styling buttons and the "Settings" page in later sections of this learning module.

### Creating a Theme Provider
Create a `template.tsx` file in the `app` directory.

Open the `template.tsx` file and add the following statements:
```
'use client';  
  
import CssBaseline from "@mui/material/CssBaseline";  
import React from "react";
import {createTheme, ThemeProvider} from '@mui/material/styles'  
import themeOptions from '@/config/theme'
```
Add the `Template` function:
```
export default function Template({children}: {children?: React.ReactNode} ) {
	const theme = createTheme({ 
		...themeOptions  
	});


	return (
	        <>
				<ThemeProvider theme={theme}>
					<CssBaseline />
					{children}
				</ThemeProvider>
	        </>
	    )
}
```
Notice that the theme uses the `themeOptions` defined in and imported from `theme.ts`.

### Using the Typography Component
Open the `page.tsx` file and add the following import statement to import the MUI Typography component:
```
import Typography from '@mui/material/Typography'
```
Delete line 15 and replace it with the following line of code:
```
<Typography variant="h1">Hello World!</Typography>
```

Go back the browser. The "Hello World" text should now use the `h1` style defined in `theme.ts`:
![styled-hello-world](assets/img/styled-hello-world.png)
You can use any the typography styles defined in `theme.ts` by specifying the variant in the `Typography` component. You can also define additional styles and use them in your website.
