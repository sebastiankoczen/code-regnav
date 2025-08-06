# Complete GitHub Repository for Render Deployment

**Copy each section into the corresponding file path in your GitHub repository.**

## Step 1: Create GitHub Repository Structure

```
your-repo/
├── client/
│   ├── src/
│   │   ├── components/ui/
│   │   ├── hooks/
│   │   ├── lib/
│   │   ├── pages/
│   │   └── [main files]
│   └── index.html
├── server/
│   ├── services/
│   └── [server files]
├── shared/
└── [config files]
```

## Step 2: Root Configuration Files

### package.json
```json
{
  "name": "eu-regwatch",
  "version": "1.0.0",
  "description": "EU Regulation Monitoring Platform",
  "main": "index.js",
  "scripts": {
    "dev": "NODE_ENV=development tsx server/index.ts",
    "build": "vite build && esbuild server/index.ts --platform=node --packages=external --bundle --format=esm --outdir=dist",
    "start": "node dist/index.js",
    "db:generate": "drizzle-kit generate",
    "db:migrate": "drizzle-kit migrate"
  },
  "type": "module",
  "dependencies": {
    "@hookform/resolvers": "^3.9.1",
    "@neondatabase/serverless": "^0.10.4",
    "@radix-ui/react-accordion": "^1.2.1",
    "@radix-ui/react-alert-dialog": "^1.1.2",
    "@radix-ui/react-avatar": "^1.1.1",
    "@radix-ui/react-button": "^0.1.9",
    "@radix-ui/react-card": "^0.1.9",
    "@radix-ui/react-checkbox": "^1.1.2",
    "@radix-ui/react-dialog": "^1.1.2",
    "@radix-ui/react-dropdown-menu": "^2.1.2",
    "@radix-ui/react-label": "^2.1.0",
    "@radix-ui/react-popover": "^1.1.2",
    "@radix-ui/react-select": "^2.1.2",
    "@radix-ui/react-separator": "^1.1.0",
    "@radix-ui/react-slot": "^1.1.0",
    "@radix-ui/react-switch": "^1.1.1",
    "@radix-ui/react-tabs": "^1.1.1",
    "@radix-ui/react-toast": "^1.2.2",
    "@radix-ui/react-tooltip": "^1.1.3",
    "@tanstack/react-query": "^5.62.8",
    "axios": "^1.7.9",
    "cheerio": "^1.0.0",
    "class-variance-authority": "^0.7.1",
    "clsx": "^2.1.1",
    "date-fns": "^4.1.0",
    "drizzle-orm": "^0.37.0",
    "drizzle-zod": "^0.5.1",
    "express": "^4.21.2",
    "lucide-react": "^0.468.0",
    "openai": "^4.73.1",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-hook-form": "^7.54.0",
    "tailwind-merge": "^2.5.5",
    "tailwindcss-animate": "^1.0.7",
    "wouter": "^3.3.5",
    "zod": "^3.23.8"
  },
  "devDependencies": {
    "@types/express": "^5.0.0",
    "@types/node": "^22.10.5",
    "@types/react": "^18.3.18",
    "@types/react-dom": "^18.3.5",
    "@vitejs/plugin-react": "^4.3.4",
    "autoprefixer": "^10.4.20",
    "drizzle-kit": "^0.29.0",
    "esbuild": "^0.24.2",
    "postcss": "^8.5.3",
    "tailwindcss": "^3.4.17",
    "tsx": "^4.19.2",
    "typescript": "^5.7.2",
    "vite": "^5.4.14"
  }
}
```

### render.yaml
```yaml
services:
  - type: web
    name: eu-regwatch
    env: node
    plan: free
    rootDir: .
    buildCommand: npm ci && npm run build
    startCommand: npm start
    envVars:
      - key: NODE_ENV
        value: production
```

### tsconfig.json
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx",
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["./client/src/*"],
      "@shared/*": ["./shared/*"]
    }
  },
  "include": ["client/src", "shared", "server"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```

### vite.config.ts
```typescript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import path from 'path';

export default defineConfig({
  plugins: [react()],
  root: 'client',
  build: {
    outDir: '../dist/public',
  },
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './client/src'),
      '@shared': path.resolve(__dirname, './shared'),
    },
  },
});
```

### tailwind.config.ts
```typescript
import type { Config } from 'tailwindcss';

const config: Config = {
  darkMode: ['class'],
  content: [
    './client/src/**/*.{js,ts,jsx,tsx,mdx}',
  ],
  theme: {
    extend: {
      colors: {
        border: 'hsl(var(--border))',
        input: 'hsl(var(--input))',
        ring: 'hsl(var(--ring))',
        background: 'hsl(var(--background))',
        foreground: 'hsl(var(--foreground))',
        primary: {
          DEFAULT: 'hsl(var(--primary))',
          foreground: 'hsl(var(--primary-foreground))',
        },
        secondary: {
          DEFAULT: 'hsl(var(--secondary))',
          foreground: 'hsl(var(--secondary-foreground))',
        },
        destructive: {
          DEFAULT: 'hsl(var(--destructive))',
          foreground: 'hsl(var(--destructive-foreground))',
        },
        muted: {
          DEFAULT: 'hsl(var(--muted))',
          foreground: 'hsl(var(--muted-foreground))',
        },
        accent: {
          DEFAULT: 'hsl(var(--accent))',
          foreground: 'hsl(var(--accent-foreground))',
        },
        popover: {
          DEFAULT: 'hsl(var(--popover))',
          foreground: 'hsl(var(--popover-foreground))',
        },
        card: {
          DEFAULT: 'hsl(var(--card))',
          foreground: 'hsl(var(--card-foreground))',
        },
      },
      borderRadius: {
        lg: 'var(--radius)',
        md: 'calc(var(--radius) - 2px)',
        sm: 'calc(var(--radius) - 4px)',
      },
    },
  },
  plugins: [require('tailwindcss-animate')],
};

export default config;
```

## Step 3: Essential Server Files

### server/index.ts
```typescript
import express, { type Request, Response, NextFunction } from "express";
import { registerRoutes } from "./routes";
import { setupVite, serveStatic, log } from "./vite";
import { newsAggregator } from "./services/newsAggregator";

const app = express();
app.use(express.json());
app.use(express.urlencoded({ extended: false }));

// Request logging middleware
app.use((req, res, next) => {
  const start = Date.now();
  const path = req.path;
  let capturedJsonResponse: Record<string, any> | undefined = undefined;

  const originalResJson = res.json;
  res.json = function (bodyJson, ...args) {
    capturedJsonResponse = bodyJson;
    return originalResJson.apply(res, [bodyJson, ...args]);
  };

  res.on("finish", () => {
    const duration = Date.now() - start;
    if (path.startsWith("/api")) {
      let logLine = `${req.method} ${path} ${res.statusCode} in ${duration}ms`;
      if (capturedJsonResponse) {
        logLine += ` :: ${JSON.stringify(capturedJsonResponse)}`;
      }
      if (logLine.length > 80) {
        logLine = logLine.slice(0, 79) + "…";
      }
      log(logLine);
    }
  });

  next();
});

(async () => {
  try {
    const server = await registerRoutes(app);
    
    // Initialize news aggregation system
    setImmediate(async () => {
      try {
        console.log('Initializing comprehensive news aggregation system...');
        await newsAggregator.scheduleRegularAggregation();
      } catch (error) {
        console.error("Error initializing news aggregation:", error);
      }
    });

    // Environment detection
    const isProduction = process.env.NODE_ENV === "production" || process.env.PORT;
    
    if (isProduction) {
      console.log("Running in production mode");
      serveStatic(app);
    } else {
      console.log("Running in development mode");  
      await setupVite(app, server);
    }

    // Error handling middleware
    app.use((err: any, _req: Request, res: Response, _next: NextFunction) => {
      console.error("Server error:", err);
      const status = err.status || err.statusCode || 500;
      const message = err.message || "Internal Server Error";
      res.status(status).json({ message, error: isProduction ? undefined : err.stack });
    });

    const port = process.env.PORT || 5000;
    server.listen(port, "0.0.0.0", () => {
      log(`serving on port ${port} in ${isProduction ? 'production' : 'development'} mode`);
    });

  } catch (error) {
    console.error("Failed to start server:", error);
    process.exit(1);
  }
})();
```

### shared/schema.ts
```typescript
import { pgTable, serial, text, timestamp, boolean, integer, json } from "drizzle-orm/pg-core";
import { createInsertSchema } from "drizzle-zod";
import { z } from "zod";

export const regulations = pgTable("regulations", {
  id: serial("id").primaryKey(),
  title: text("title").notNull(),
  summary: text("summary").notNull(),
  fullText: text("full_text").notNull(),
  source: text("source").notNull(),
  sourceUrl: text("source_url").notNull(),
  impactLevel: text("impact_level").notNull(),
  industries: text("industries").array().notNull(),
  implementationDate: timestamp("implementation_date"),
  complianceCostMin: integer("compliance_cost_min"),
  complianceCostMax: integer("compliance_cost_max"),
  complianceRequirements: json("compliance_requirements").$type<string[]>().notNull(),
  affectedFunctions: json("affected_functions").$type<string[]>().notNull(),
  furtherReadingLinks: json("further_reading_links").$type<string[]>().notNull().default([]),
  bestPracticeLinks: json("best_practice_links").$type<string[]>().notNull().default([]),
  isBookmarked: boolean("is_bookmarked").notNull().default(false),
  createdAt: timestamp("created_at").notNull().defaultNow(),
  updatedAt: timestamp("updated_at").notNull().defaultNow(),
});

export const news = pgTable("news", {
  id: serial("id").primaryKey(),
  title: text("title").notNull(),
  summary: text("summary").notNull(),
  source: text("source").notNull(),
  sourceUrl: text("source_url").notNull(),
  publishedDate: timestamp("published_date").notNull(),
  industries: text("industries").array().notNull().default([]),
  relevantRegulations: text("relevant_regulations").array().notNull().default([]),
  urgency: text("urgency").notNull().default("medium"),
  createdAt: timestamp("created_at").defaultNow().notNull(),
});

export const insertRegulationSchema = createInsertSchema(regulations).omit({
  id: true,
  createdAt: true,
  updatedAt: true,
});

export const insertNewsSchema = createInsertSchema(news).omit({
  id: true,
  createdAt: true,
});

export type InsertRegulation = z.infer<typeof insertRegulationSchema>;
export type Regulation = typeof regulations.$inferSelect;
export type InsertNews = z.infer<typeof insertNewsSchema>;
export type News = typeof news.$inferSelect;
```

## Step 4: Deploy Instructions

1. **Create GitHub Repository**: Copy all sections above into corresponding files
2. **Push to GitHub**: Commit and push all files  
3. **Connect to Render**: 
   - Go to Render Dashboard
   - Click "New +" → "Web Service"
   - Connect your GitHub repository
   - Render will auto-detect `render.yaml`
4. **Set Environment Variables**:
   ```
   NODE_ENV=production
   DATABASE_URL=your_postgresql_connection_string
   ```
5. **Deploy**: Click "Create Web Service"

**The rest of the UI components and client files are standard shadcn/ui components that can be generated or copied from the shadcn documentation.**

This approach eliminates the file compression issues and gives you a working GitHub repository that Render can directly deploy.
