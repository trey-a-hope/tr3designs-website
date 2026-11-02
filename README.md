# tr3Designs Website

The marketing and catalog website for tr3Designs, a mobile e-commerce app that sold 3D-printed products, from jewelry and trinkets to ashtrays and cutlery. The site shows the product catalog, an about page with the mission statement and privacy policy, a contact form and a small admin page. It's an Angular 5 app from 2018.

## Pages

Routes are defined in `src/app/app-routing.module.ts`:

| Path | Component | What it shows |
| --- | --- | --- |
| `/` | `HomeComponent` | hero banner ("3D Printing has never been cooler.") |
| `/about` | `AboutComponent` | what tr3Designs is, the mission statement ("Display good energy through artistry.") and the privacy policy |
| `/products` | `ProductsComponent` | product list with name, caption and date, linking to a detail view with images and price (purchases happen in the mobile app) |
| `/contact` | `ContactComponent` | name, email and message (400 characters max) form |
| `/admin` | `AdminComponent` | table of customers from the Firestore `Users` collection (name, email, customer ID, FCM token) |
| `**` | `PageNotFoundComponent` | 404 page |

## How it works

- **Products** (`src/app/services/products.service.ts`) come from Stripe products, fetched through a PHP endpoint on `es.tr3umphant-designs.com`, or from the mock JSON in `src/assets/mock-data/products.json` that the page uses today. Prices are stored in cents and shown with Angular's currency pipe.
- **Contact** (`src/app/services/email.service.ts`) posts the form as JSON to a PHP mail script on the same host.
- **Admin** reads customers from Cloud Firestore with AngularFire, using the Firebase config in `src/environments/environment.ts`.
- Styling is Material Design for Bootstrap (`angular-bootstrap-md`) with Font Awesome icons.

## Tech stack

- Angular 5 with the Angular CLI (`.angular-cli.json`)
- Firebase JS SDK 4 / AngularFire (Firestore)
- Material Design for Bootstrap, Font Awesome
- Karma + Jasmine unit tests and Protractor e2e tests (CLI defaults)

## Getting started

Requires a Node.js version that Angular CLI 1.x supports (Node 8 or 10).

```sh
npm install
npm start          # ng serve on http://localhost:4200
npm run build      # ng build --prod, output in dist/
```

## Security note

`src/app/services/products.service.ts` contains a Stripe live secret key, which ships to every visitor's browser. Revoke that key in the Stripe dashboard and move Stripe calls behind a server. The `/admin` route also has no sign-in check.
