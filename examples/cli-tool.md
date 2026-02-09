# 🖥️ Example: Building a CLI Tool

A real conversation building a useful CLI tool with vibe coding.

---

## The Goal

Build a CLI tool that helps manage environment variables across projects.

---

## Session 1: Initial Build

### User

```
Build a CLI tool called "envman" that helps manage .env files.

Features:
- List all env vars in current directory
- Compare .env with .env.example (show missing/extra)
- Sync .env with .env.example (interactive)
- Encrypt/decrypt sensitive values

Use Commander.js, TypeScript, make it pleasant to use with colors.
```

### Claude

I'll create a complete CLI tool for managing environment files.

```typescript
// src/index.ts
#!/usr/bin/env node
import { Command } from 'commander';
import chalk from 'chalk';
import { listEnvVars } from './commands/list';
import { compareEnvFiles } from './commands/compare';
import { syncEnvFiles } from './commands/sync';
import { encryptValue, decryptValue } from './commands/crypto';

const program = new Command();

program
  .name('envman')
  .description('Manage your .env files with ease')
  .version('1.0.0');

program
  .command('list')
  .description('List all environment variables')
  .option('-f, --file <path>', 'env file path', '.env')
  .option('--show-values', 'show actual values (careful!)')
  .action(listEnvVars);

program
  .command('compare')
  .description('Compare .env with .env.example')
  .option('--source <path>', 'source file', '.env')
  .option('--target <path>', 'target file', '.env.example')
  .action(compareEnvFiles);

program
  .command('sync')
  .description('Sync .env with .env.example interactively')
  .action(syncEnvFiles);

program
  .command('encrypt <value>')
  .description('Encrypt a sensitive value')
  .action(encryptValue);

program
  .command('decrypt <value>')
  .description('Decrypt an encrypted value')
  .action(decryptValue);

program.parse();
```

```typescript
// src/commands/list.ts
import chalk from 'chalk';
import { parseEnvFile } from '../utils/parser';

interface ListOptions {
  file: string;
  showValues?: boolean;
}

export async function listEnvVars(options: ListOptions) {
  const vars = await parseEnvFile(options.file);
  
  console.log(chalk.bold(`\n📋 Environment Variables (${options.file})\n`));
  
  for (const [key, value] of Object.entries(vars)) {
    const displayValue = options.showValues 
      ? chalk.gray(value) 
      : chalk.gray('••••••••');
    console.log(`  ${chalk.cyan(key)}=${displayValue}`);
  }
  
  console.log(chalk.dim(`\n  Total: ${Object.keys(vars).length} variables\n`));
}
```

```typescript
// src/commands/compare.ts
import chalk from 'chalk';
import { parseEnvFile } from '../utils/parser';

interface CompareOptions {
  source: string;
  target: string;
}

export async function compareEnvFiles(options: CompareOptions) {
  const source = await parseEnvFile(options.source);
  const target = await parseEnvFile(options.target);
  
  const sourceKeys = new Set(Object.keys(source));
  const targetKeys = new Set(Object.keys(target));
  
  const missing = [...targetKeys].filter(k => !sourceKeys.has(k));
  const extra = [...sourceKeys].filter(k => !targetKeys.has(k));
  const common = [...sourceKeys].filter(k => targetKeys.has(k));
  
  console.log(chalk.bold('\n🔍 Environment Comparison\n'));
  
  if (missing.length > 0) {
    console.log(chalk.red('❌ Missing from .env (defined in .env.example):'));
    missing.forEach(k => console.log(`   ${chalk.red(k)}`));
    console.log();
  }
  
  if (extra.length > 0) {
    console.log(chalk.yellow('⚠️  Extra in .env (not in .env.example):'));
    extra.forEach(k => console.log(`   ${chalk.yellow(k)}`));
    console.log();
  }
  
  if (missing.length === 0 && extra.length === 0) {
    console.log(chalk.green('✅ Perfect! All variables are in sync.\n'));
  } else {
    console.log(chalk.dim(`  Common: ${common.length} | Missing: ${missing.length} | Extra: ${extra.length}\n`));
  }
}
```

```typescript
// src/commands/sync.ts
import chalk from 'chalk';
import inquirer from 'inquirer';
import { parseEnvFile, writeEnvFile } from '../utils/parser';

export async function syncEnvFiles() {
  const env = await parseEnvFile('.env').catch(() => ({}));
  const example = await parseEnvFile('.env.example');
  
  const envKeys = new Set(Object.keys(env));
  const missing = Object.keys(example).filter(k => !envKeys.has(k));
  
  if (missing.length === 0) {
    console.log(chalk.green('\n✅ Already in sync!\n'));
    return;
  }
  
  console.log(chalk.bold(`\n🔄 ${missing.length} missing variables\n`));
  
  const answers: Record<string, string> = {};
  
  for (const key of missing) {
    const defaultValue = example[key] || '';
    const { value } = await inquirer.prompt([{
      type: 'input',
      name: 'value',
      message: `${chalk.cyan(key)}:`,
      default: defaultValue || undefined
    }]);
    answers[key] = value;
  }
  
  const updated = { ...env, ...answers };
  await writeEnvFile('.env', updated);
  
  console.log(chalk.green('\n✅ .env updated successfully!\n'));
}
```

```typescript
// src/utils/parser.ts
import fs from 'fs/promises';
import path from 'path';

export async function parseEnvFile(filePath: string): Promise<Record<string, string>> {
  const fullPath = path.resolve(process.cwd(), filePath);
  const content = await fs.readFile(fullPath, 'utf-8');
  
  const vars: Record<string, string> = {};
  
  for (const line of content.split('\n')) {
    const trimmed = line.trim();
    if (!trimmed || trimmed.startsWith('#')) continue;
    
    const [key, ...valueParts] = trimmed.split('=');
    if (key) {
      vars[key.trim()] = valueParts.join('=').trim().replace(/^["']|["']$/g, '');
    }
  }
  
  return vars;
}

export async function writeEnvFile(filePath: string, vars: Record<string, string>) {
  const content = Object.entries(vars)
    .map(([key, value]) => `${key}=${value}`)
    .join('\n');
  
  await fs.writeFile(filePath, content + '\n');
}
```

```json
// package.json
{
  "name": "envman",
  "version": "1.0.0",
  "description": "Manage your .env files with ease",
  "bin": {
    "envman": "./dist/index.js"
  },
  "scripts": {
    "build": "tsc",
    "dev": "ts-node src/index.ts"
  },
  "dependencies": {
    "chalk": "^4.1.2",
    "commander": "^11.0.0",
    "inquirer": "^8.2.5"
  },
  "devDependencies": {
    "@types/inquirer": "^8.2.5",
    "@types/node": "^20.0.0",
    "typescript": "^5.0.0"
  }
}
```

---

### User

```
Nice! Add the encryption feature using a master password.
Also add a "validate" command that checks for common mistakes:
- Empty values
- Values that look like placeholders (YOUR_API_KEY)
- Duplicate keys
```

### Claude

```typescript
// src/commands/crypto.ts
import chalk from 'chalk';
import crypto from 'crypto';
import inquirer from 'inquirer';

const ALGORITHM = 'aes-256-gcm';

function getKey(password: string): Buffer {
  return crypto.scryptSync(password, 'envman-salt', 32);
}

export async function encryptValue(value: string) {
  const { password } = await inquirer.prompt([{
    type: 'password',
    name: 'password',
    message: 'Master password:'
  }]);
  
  const key = getKey(password);
  const iv = crypto.randomBytes(16);
  const cipher = crypto.createCipheriv(ALGORITHM, key, iv);
  
  let encrypted = cipher.update(value, 'utf8', 'hex');
  encrypted += cipher.final('hex');
  
  const authTag = cipher.getAuthTag();
  const result = `ENC:${iv.toString('hex')}:${authTag.toString('hex')}:${encrypted}`;
  
  console.log(chalk.green('\n🔐 Encrypted value:\n'));
  console.log(`   ${chalk.cyan(result)}\n`);
}

export async function decryptValue(encrypted: string) {
  const { password } = await inquirer.prompt([{
    type: 'password',
    name: 'password',
    message: 'Master password:'
  }]);
  
  try {
    const [, iv, authTag, data] = encrypted.split(':');
    const key = getKey(password);
    const decipher = crypto.createDecipheriv(ALGORITHM, key, Buffer.from(iv, 'hex'));
    decipher.setAuthTag(Buffer.from(authTag, 'hex'));
    
    let decrypted = decipher.update(data, 'hex', 'utf8');
    decrypted += decipher.final('utf8');
    
    console.log(chalk.green('\n🔓 Decrypted value:\n'));
    console.log(`   ${chalk.cyan(decrypted)}\n`);
  } catch {
    console.log(chalk.red('\n❌ Decryption failed. Wrong password?\n'));
  }
}
```

```typescript
// src/commands/validate.ts
import chalk from 'chalk';
import { parseEnvFile } from '../utils/parser';

interface ValidateOptions {
  file: string;
}

const PLACEHOLDER_PATTERNS = [
  /^your_/i,
  /^xxx/i,
  /^changeme$/i,
  /^todo$/i,
  /^<.*>$/,
  /^\[.*\]$/,
];

export async function validateEnvFile(options: ValidateOptions) {
  const vars = await parseEnvFile(options.file);
  const issues: string[] = [];
  const seen = new Map<string, number>();
  
  console.log(chalk.bold(`\n🔍 Validating ${options.file}\n`));
  
  for (const [key, value] of Object.entries(vars)) {
    // Check for empty values
    if (!value || value.trim() === '') {
      issues.push(`${chalk.red('Empty')}: ${chalk.cyan(key)} has no value`);
    }
    
    // Check for placeholders
    if (PLACEHOLDER_PATTERNS.some(p => p.test(value))) {
      issues.push(`${chalk.yellow('Placeholder')}: ${chalk.cyan(key)} looks like a placeholder ("${value}")`);
    }
    
    // Track duplicates
    const count = (seen.get(key) || 0) + 1;
    seen.set(key, count);
    if (count > 1) {
      issues.push(`${chalk.red('Duplicate')}: ${chalk.cyan(key)} appears ${count} times`);
    }
  }
  
  if (issues.length === 0) {
    console.log(chalk.green('✅ No issues found!\n'));
  } else {
    console.log(chalk.yellow(`⚠️  Found ${issues.length} issue(s):\n`));
    issues.forEach(issue => console.log(`   ${issue}`));
    console.log();
  }
}
```

Add to main index.ts:
```typescript
program
  .command('validate')
  .description('Validate env file for common mistakes')
  .option('-f, --file <path>', 'env file path', '.env')
  .action(validateEnvFile);
```

---

### User

```
Perfect. Now add a README and make it publishable to npm.
```

### Claude

```markdown
// README.md
# 🔧 envman

> Manage your .env files with ease

[![npm](https://img.shields.io/npm/v/envman-cli)](https://www.npmjs.com/package/envman-cli)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Installation

\`\`\`bash
npm install -g envman-cli
\`\`\`

## Commands

### List variables
\`\`\`bash
envman list
envman list --show-values  # careful!
\`\`\`

### Compare with .env.example
\`\`\`bash
envman compare
\`\`\`

### Sync missing variables
\`\`\`bash
envman sync  # interactive prompts for missing vars
\`\`\`

### Validate
\`\`\`bash
envman validate  # check for empty values, placeholders, duplicates
\`\`\`

### Encrypt/Decrypt
\`\`\`bash
envman encrypt "my-secret-api-key"
envman decrypt "ENC:..."
\`\`\`

## License

MIT
```

---

## Key Takeaways

1. **Started with clear requirements** - Listed features upfront
2. **Iterated on additions** - Added validation and encryption after core
3. **Finished with polish** - README makes it publishable
4. **Total time** - ~15 minutes of conversation for a useful tool
