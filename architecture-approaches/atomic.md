# Atomic design

1. [General information](#general-information)
2. [Structure example](#structure-example)

## General information

The main idea behind Atomic Design is to break down the user interface into smaller, reusable components that can be combined to create more complex UI elements and pages. The methodology is inspired by chemistry's atomic structure, where atoms combine to form molecules, which in turn combine to form organisms, and so on.

The five stages of Atomic Design are:

1. **Atoms**:
   Atoms are the smallest and simplest UI elements, such as buttons, input fields, labels, and icons. They are the building blocks of all other UI components and should be designed to be highly reusable.

2. **Molecules**:
   Molecules are combinations of two or more atoms that work together to create a more significant, reusable component. For example, a form input with a label and validation message is a molecule formed by combining several atoms.

3. **Organisms**:
   Organisms are more complex components that combine molecules and atoms to form functional and recognizable sections of a user interface. Examples of organisms could be a header, footer, or a card component with various content elements.

4. **Templates**:
   Templates represent the layout of a page and define the placement of organisms within the overall structure. They provide a framework for organizing content on a specific type of page.

5. **Pages**:
   Pages are instances where templates are filled with actual content to create fully functioning web pages or application screens.

The benefit of Atomic Design is that it promotes consistency, scalability, and reusability. By breaking down the UI into smaller components and adhering to a consistent naming and organizational structure, design and development teams can work more efficiently. Changes to individual atoms or molecules automatically propagate to higher-level components, ensuring a coherent user experience.

Atomic Design is not tied to any specific technology or framework and can be used with any frontend development stack, including React, Angular, or Vue.js. It's a design methodology that complements component-based architecture and helps teams create well-structured, maintainable, and consistent UI systems.

## Structure example

- src/

  - atoms/

    - button/
      - Button.tsx
    - input/
      - Input.tsx
    - icon/
      - Icon.tsx

  - molecules/

    - form-input/
      - FormInput.tsx
    - card/
      - Card.tsx

  - organisms/

    - header/
      - Header.tsx
    - footer/
      - Footer.tsx

  - templates/

    - blog-post-template/
      - BlogPostTemplate.tsx
    - product-page-template/
      - ProductPageTemplate.tsx

  - pages/

    - home-page/
      - HomePage.tsx
    - about-page/
      - AboutPage.tsx
    - contact-page/
      - ContactPage.tsx

  - styles/

    - global.css
    - variables.css
    - mixins.css
    - theme.ts

  - utils/

    - api.ts
    - helpers.ts
    - validation.ts

  - redux/

    - actions/
      - actionTypes.ts
      - userActions.ts
    - reducers/
      - userReducer.ts
    - store.ts

  - assets/

    - images/
      - logo.png
      - background.jpg
    - icons/
      - menu.svg
      - close.svg

  - index.tsx
  - App.tsx
