# Project Structure

src
├── main.tsx
├── app/
│  ├── config/
│  │  └── firebase.ts
│  ├── providers/
│  │  └── app.tsx
│  └── routes/
│      ├── index.tsx
│      ├── public.tsx
│      └── protected.tsx
├── features/
│  ├── feature-one/
│  ├── feature-two/
│  ├── auth/
│  │  ├── ui/
│  │  ├── types/
│  │  ├── context/
│  │  ├── components/
│  │  └── services/
│  ├── shared/
│  │  ├── ui/
│  │  ├── types/
│  │  ├── context/
│  │  ├── utils/
│  │  └── services/
├── index.css
├── vite-env.d.ts
└── assets/

## Features

Each feature folder contains the following structure:

```bash
feature-name/
│  ├── components/
│  │    ├── ComponentName.tsx
│  │    └── index.ts
│  ├── hooks/
│  │    └── useHookName.ts
│  ├── types/
│  │    └── typeName.ts
│  └── services/
│  │    └── serviceName.ts
│  └── pages/
│      └── FeaturePage.tsx
└── FeatureName.tsx
```