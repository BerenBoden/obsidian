##### ***Using `import.meta.url`***
```
import { fileURLToPath } from 'url';
import { dirname, join } from 'path';

// Get the file path of the current module
const __filename = fileURLToPath(import.meta.url);

// Get the directory name of the current module
const __dirname = dirname(__filename);

// Path to package.json
const packageJsonPath = join(__dirname, 'package.json');
```
