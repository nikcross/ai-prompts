# OpenForum Server Context

You are working with an OpenForum server, a browser-editable web application platform that combines wiki-style content management with powerful JavaScript capabilities.

**Key Feature:** OpenForum includes automatic two-way data binding between HTML and JavaScript, eliminating most manual DOM manipulation!

## Operational Reminders

- Treat `page.content` as the source of truth. Regenerate `page.html` via the Refresh action instead of editing or deleting the generated file.
- Keep `page.html.template.link` files pristine—each must contain only the absolute template path with no extra markup or commentary.

## Development Philosophy

When building features on OpenForum, follow these principles:

### 1. Incremental Development
Break tasks into small, testable steps:
- Create minimal working version first
- Add one feature at a time
- Test after each change
- Fix issues before continuing

### 2. Progressive Enhancement
Start simple, add complexity gradually:
1. **Minimal working example** - Hardcoded values
2. **Basic form** - User input for required fields
3. **Validation** - Check inputs
4. **Optional fields** - Add non-required options
5. **Advanced features** - Power user options
6. **UI polish** - Styling, animations, UX improvements

### 3. Quick Testing Cycle
- Make small change
- Use pageBuilder refresh to update page
- Test in browser immediately
- Fix any issues before continuing

## Working Permissions

You have permission to work with:
- **Local server**: Pages and files under `/web/content/default/`
- **Remote server**: Pages and files under these hierarchies:
  - `/Development/*` - Development pages
  - `/TheLab/Experiments/*` - Experimental pages
  - Use `mcp__open-forum-remote__pageBuilder` for remote operations
  - Use `mcp__open-forum-local__pageBuilder` for local operations

## Speech Synthesis

**IMPORTANT**: At the end of EVERY response, use `mcp__open-forum-local__enhancedSpeechTool` to provide a spoken summary of what you did or explained. This helps provide audio feedback for all interactions.

Example:
```javascript
{
  "action": "speak",
  "text": "Brief summary of what was accomplished in this response"
}
```

## Quick Start: Building a Simple App

### Basic Workflow

1. **Create a page** - Use pageBuilder MCP tool or web interface
2. **Edit page.content** - Write OpenForum markup + HTML
3. **Add page.js** - Client-side JavaScript (optional)
4. **Add get.sjs/post.sjs** - Server-side JavaScript (optional)
5. **Refresh page** - Changes take effect immediately

## OpenForum Markup Syntax

When creating or editing OpenForum pages, use OpenForum wiki markup:

### Basic Formatting
- **Headings**: `!`, `!!`, `!!!`, `!!!!` (more exclamations = smaller heading)
- **Bold**: `__text__`
- **Italic**: `''text''`
- **Strike through**: `~~text~~`
- **Superscript**: `^^text^^`
- **Center**: `==text==`

### Links & Images
- `[/OpenForum/PagePath]` - internal wiki page link
- `[Link Text|/path]` - link with custom text
- `[http://example.com]` - external link
- `[Link Text|http://example.com]` - external link with text
- `[/path/to/image.png]` - embed image

### Lists
- `* item` - bulleted list (use `**` for nested)
- `# item` - numbered list (use `##` for nested)

### Blocks & Special Formatting
- `((text))` - paragraph
- `[[text]]` - box
- `{{{code}}}` - code blocks (displays markup literally)
- `----` - horizontal rule
- `\\` - forced line break

### Tables
```
|column 1|column 2|column 3|
|cell 1|cell 2|cell 3|
|cell 1|cell 2|cell 3|
```

### Comments
- `/* invisible comment */` - won't appear in rendered output

### Extended Markup (Extensions)
- `[{ChildPagesList}]` - list child pages
- `[{InsertPage page="/path" section="1"}]` - insert content from another page
- `[{MarkDown}]...[{/MarkDown}]` - include Markdown content
- `[{Icon name="iconName"}]` - insert icon
- `[{Date}]` - insert current date
- `[{AttachmentsList}]` - list page attachments
- And many more extensions available in `/OpenForum/Extensions/`

## OpenForum Page Structure

Each OpenForum page is a directory containing:
- `page.content` - wiki markup content
- `page.html` - rendered HTML output
- `page.html.fragment` - HTML fragment (without template)
- `page.html.template` or `page.html.template.link` - template reference
- `page.js` - client-side JavaScript
- `data.json` - page metadata (title, description, keywords, author, etc.)
- `*.sjs` - server-side JavaScript files
- Other attachments (images, files, etc.)

## Working with MCP Tools

### pageBuilder Tool
Use the `mcp__open-forum-local__pageBuilder` or `mcp__open-forum-remote__pageBuilder` tool for page operations:

**Actions:**
- `create` - Create new page (requires: page_name, page_title, page_content)
- `read` - Read existing page (requires: page_name)
- `update` - Update page content (requires: page_name, page_content)
- `convert` - Convert Markdown to OpenForum markup (requires: markdown_content)
- `refresh` - Re-render page HTML (requires: page_name)
- `list_pages` - List all pages
- `delete_page` - Delete entire page (requires: page_name, confirm_delete: true)
- `delete_file` - Delete specific file (requires: page_name, file_name, confirm_delete: true)
- `copy_page` - Copy page to new location
- `move_page` - Move page to new location

**Common parameters:**
- `page_name` - Path like "HomeLab/MyProject/PageName"
- `page_content` - OpenForum markup content
- `page_data` - Metadata object with author, description, keywords, etc.
- `auto_refresh` - Auto-render after create/update (default: true)

### enhancedSpeechTool
Text-to-speech with notifications:
- `speak` - Speak text (params: text, voice, force_speak)
- `notify_completion` - Announce task completion (params: process_name, duration)
- `set_silent_mode` - Enable/disable silent mode (params: silent)
- `replay_last_response` - Replay last spoken text
- `list_voices` - List available voices
- `get_status` - Get current status

### MCPDebugTool
Debug MCP operations:
- `get_queue_messages` - Get recent queue messages
- `get_latest_error` - Get most recent error
- `get_error_history` - Get error history
- `get_recent_messages` - Get recent messages

## Key Directories

- `/web/content/default/` - Main content directory
- `/web/content/default/OpenForum/` - OpenForum system pages
- `/web/content/default/OpenForum/AddOn/` - AddOns/plugins
- `/web/content/default/OpenForum/Documentation/` - Documentation
- `/web/content/default/OpenForum/Extensions/` - Markup extensions

## Best Practices

1. **Use OpenForum markup by default** - Don't use Markdown unless explicitly asked
2. **Include HTML when needed** - OpenForum markup can be mixed with HTML
3. **Always specify page paths correctly** - Use format like "HomeLab/Project/Page"
4. **Set auto_refresh: true** - To immediately see rendered output
5. **Use convert action** - When you have Markdown that needs to be converted
6. **Include page metadata** - Add author, description, keywords in page_data
7. **Read before update** - Always read existing pages before modifying

## Common Workflows

### Creating a new page:
```javascript
{
  action: "create",
  page_name: "HomeLab/MyProject/NewPage",
  page_title: "My New Page",
  page_content: "!!Welcome\n\n((This is my new page))",
  page_data: {
    author: "Your Name",
    description: "Page description",
    keywords: "keyword1, keyword2"
  }
}
```

### Converting Markdown to OpenForum:
```javascript
{
  action: "convert",
  markdown_content: "# Heading\n\nSome **bold** text"
}
```

### Updating existing page:
```javascript
{
  action: "update",
  page_name: "HomeLab/MyProject/ExistingPage",
  page_content: "!!Updated Content\n\n((New paragraph))"
}
```

## OpenForum Architecture

### Core Concepts
- **Browser-editable web application server**: Can be edited while running, directly in the browser
- **Page-based architecture**: Each page is a URL with a directory structure
- **Wiki-like but more powerful**: Extends beyond content to enable building web services
- **Hierarchical organization**: Pages inherit templates and access controls from parents
- **Self-contained**: Includes integrated web server and user authentication

### Page Files Explained
- **page.content** - Source content mixing wiki syntax and HTML. When saved, generates page.html and page.html.fragment
- **page.html** - Fully rendered page with template wrapper (this is what's served)
- **page.html.fragment** - Content only, without template wrapper
- **page.html.template** - Optional template wrapper. Searches hierarchy, defaults to root template
- **page.js** - Client-side JavaScript, typically defines `OpenForum.init()` function
- **data.json** - Page metadata in JSON format (title, description, keywords, author)
- **get.sjs** - Server-side JavaScript for GET requests
- **post.sjs** - Server-side JavaScript for POST requests
- **access.json** - Page-level permissions controlling user/group actions
- **private** marker - Restricts access to administrators only
- **history/** - Child page maintaining version control for all file changes

### Server-Side JavaScript (.sjs files)
- Execute on the server before content is delivered
- Full access to Java APIs, databases, external services
- `get.sjs` handles GET requests, `post.sjs` handles POST requests
- Can integrate with databases, REST APIs, XML web services
- Example: `/OpenForum/Actions/` contains built-in server-side actions

## Web Service Builder Pattern

When creating REST services in OpenForum, developers often use the **Web Service Builder plugin** in the Editor. This generates boilerplate code and creates several interconnected files that must be kept in sync.

### Generated Files:
- **service-definition-config.json** - Overall service definition
- **get-service-definition-config.json** - GET method definitions
- **post-service-definition-config.json** - POST method definitions
- **{Library}.sjs** - Server-side implementation (e.g., Actions.sjs)
- **{Library}Client.js** - Client-side API wrapper
- **get.sjs** - GET request router/handler
- **post.sjs** - POST request router/handler

### IMPORTANT: When adding a new REST endpoint, you MUST update ALL relevant files:

1. **{Library}.sjs** - Add the implementation function
2. **{Library}Client.js** - Add the client-side API method
3. **get.sjs or post.sjs** - Add the action handler with parameter validation
4. **get-service-definition-config.json or post-service-definition-config.json** - Add the service definition

**Missing any of these files will cause the endpoint to fail!** The most commonly forgotten file is get.sjs/post.sjs.

### Example Pattern:

**Step 1: Actions.sjs (Implementation)**
```javascript
/* Used in service /Actions getProcessesByPort */
//Example use: Actions.getProcessesByPort(port)
self.getProcessesByPort = function(port) {
  var output = runSycnProcess( "lsof -i :" + port );
  return { processes: output.split("\n") };
};
```

**Step 2: ActionsClient.js (Client Wrapper)**
```javascript
//Example use: ActionsClient.getProcessesByPort( port, callBack );
self.getProcessesByPort = function( port, callBack ) {
  OFX.get( host+'/Actions' )
    .withAction( 'getProcessesByPort' )
    .withData( {port: port} )
    .onSuccess( callBack ).go();
};
```

**Step 3: get.sjs (Request Handler - DON'T FORGET THIS!)**
```javascript
if(action==="getProcessesByPort") {
    var port = transaction.getParameter("port");
    if(port===null) {
     throw "Request is missing required parameter port";
    } else {
     port = "" + port;
    }

    var returned = Actions.getProcessesByPort(port);

    if(returned) result = {result: "ok", message: "Performed action "+action, data: returned};
}
```

**Step 4: get-service-definition-config.json (Service Definition)**
```json
{
    "name": "getProcessesByPort",
    "call": "Actions.getProcessesByPort(port);",
    "parameters": [
        {
            "name": "port",
            "required": true,
            "type": "string"
        }
    ],
    "libraries": [],
    "method": "get",
    "description": "Get processes listening on a specific port using lsof",
    "example": {
        "request": "",
        "response": "",
        "errorResponse": ""
    },
    "action": "getProcessesByPort"
}
```

### Checklist for Adding a New Service Method:
- [ ] Add implementation to {Library}.sjs
- [ ] Add client method to {Library}Client.js
- [ ] **Add handler to get.sjs or post.sjs with parameter validation** ← Most commonly forgotten!
- [ ] Add definition to get-service-definition-config.json or post-service-definition-config.json
- [ ] Refresh the page to apply changes

### Common Mistake:
❌ **Don't forget to update get.sjs/post.sjs!** - Without the handler in get.sjs or post.sjs, the REST endpoint won't work even if all other files are correct. The client will make the request but the server won't know how to route it.

## Complete Example: Building a Simple Web App

Here's a complete step-by-step example showing all the pieces working together:

### Step 1: Create the Page

```javascript
// Using MCP pageBuilder tool
{
  "action": "create",
  "page_name": "MyApp/Dashboard",
  "page_title": "My Dashboard",
  "page_content": "!!My Dashboard\n\n((Welcome to my app))",
  "page_data": {
    "author": "Your Name",
    "description": "Dashboard page"
  }
}
```

### Step 2: Add Interactive UI (page.content)

```
!!My Dashboard

((Enter your data below:))

<div style="padding: 20px;">
  <form id="myForm">
    <label>Name:</label>
    <input type="text" id="userName" />

    <button onclick="saveData()">Save</button>
  </form>
</div>

<div id="output" style="display: none;">
  <pre id="result"></pre>
</div>
```

### Step 3: Add Client-Side Logic (page.js)

```javascript
async function saveData() {
  const name = document.getElementById('userName').value;

  if (!name) {
    alert('Please enter a name');
    return;
  }

  // Call server-side endpoint
  const url = '/MyApp/Dashboard?action=save&name=' + encodeURIComponent(name);
  const response = await fetch(url);
  const result = await response.json();

  // Display result
  document.getElementById('output').style.display = 'block';
  document.getElementById('result').textContent = JSON.stringify(result, null, 2);
}
```

### Step 4: Add Server-Side Logic (get.sjs)

```javascript
var action = transaction.getParameter("action");

if (action === "save") {
  var name = transaction.getParameter("name");

  if (!name) {
    transaction.sendJSON(JSON.stringify({
      success: false,
      error: "Name is required"
    }));
    return;
  }

  // Save to file
  var data = {
    name: name,
    timestamp: new Date().getTime()
  };

  OpenForum.saveJSON("/MyApp/Dashboard/data.json", JSON.stringify(data));

  transaction.sendJSON(JSON.stringify({
    success: true,
    data: data
  }));
  return;
}

// Default: show the page
transaction.setResult(transaction.SHOW_PAGE);
```

## Common Development Patterns

### Pattern 1: Form with Validation

**HTML (in page.content):**
```html
<form id="configForm">
  <label>Port *</label>
  <input type="text" id="port" required />

  <label>Host</label>
  <input type="text" id="host" value="localhost" />

  <button onclick="submitForm()">Submit</button>
</form>
```

**JavaScript (in page.js):**
```javascript
function submitForm() {
  const port = document.getElementById('port').value.trim();
  const host = document.getElementById('host').value.trim();

  // Validate
  const errors = [];
  if (!port) errors.push('Port is required');

  if (errors.length > 0) {
    alert('Errors:\n' + errors.join('\n'));
    return;
  }

  // Build config
  const config = { port, host };

  // Submit to server
  callServer(config);
}

async function callServer(config) {
  const url = '/MyApp/Page?action=process&config=' +
              encodeURIComponent(JSON.stringify(config));
  const response = await fetch(url);
  const result = await response.json();

  if (result.success) {
    alert('Success!');
  } else {
    alert('Error: ' + result.error);
  }
}
```

### Pattern 2: Data Binding with Auto-Update

**The Magic of `of-id`:**

OpenForum provides automatic two-way data binding between HTML elements and JavaScript variables using the `of-id` attribute. This eliminates the need for manual `getElementById()` calls and event handlers!

**HTML:**
```html
<input of-id="user.name" type="text" />
<span>Hello, {{user.name}}!</span>
```

**JavaScript:**
```javascript
// Initialize data object
var user = {
  name: "World"
};

// That's it! OpenForum automatically:
// 1. Populates the input with user.name value
// 2. Updates user.name when input changes
// 3. Updates {{user.name}} display when value changes
// 4. Scans every 500ms to keep everything in sync
```

**How it works:**
1. **Auto-scan** - OpenForum runs `scan()` every 500ms by default
2. **Bidirectional** - Changes in UI update JS, changes in JS update UI
3. **Nested objects** - Use dot notation: `config.database.host`
4. **No boilerplate** - No event listeners, no `getElementById()`, no manual syncing

**Real Example - Config Form:**

```html
<!-- HTML with of-id binding -->
<form>
  <input of-id="config.host" type="text" />
  <input of-id="config.port" type="text" />
  <input of-id="config.database" type="text" />

  <button onclick="saveConfig()">Save</button>
</form>

<!-- Display current values -->
<div>
  Connecting to: {{config.host}}:{{config.port}}
  Database: {{config.database}}
</div>
```

```javascript
// Initialize with defaults
var config = {
  host: "localhost",
  port: "5432",
  database: "mydb"
};

function saveConfig() {
  // config object is already in sync with form!
  // Just validate and submit
  if (!config.host || !config.port) {
    alert('Host and port are required');
    return;
  }

  // Submit to server
  JSON.post('/MyApp/Page', 'saveConfig', JSON.stringify(config))
    .onSuccess(function(result) {
      alert('Saved!');
    })
    .go();
}
```

**Compare with manual approach:**

```javascript
// ❌ WITHOUT of-id (manual, verbose)
function saveConfig() {
  const config = {
    host: document.getElementById('host').value,
    port: document.getElementById('port').value,
    database: document.getElementById('database').value
  };
  // ... validate and submit
}

// ✅ WITH of-id (automatic, clean)
function saveConfig() {
  // config is already up-to-date!
  // ... validate and submit
}
```

### Pattern 3: Dynamic Tables with of-repeatFor

**HTML:**
```html
<table>
  <thead>
    <tr><th>Name</th><th>Value</th></tr>
  </thead>
  <tbody>
    <tr of-repeatFor="item in items">
      <td>{{item.name}}</td>
      <td>{{item.value}}</td>
    </tr>
  </tbody>
</table>
```

**JavaScript:**
```javascript
var items = [
  { name: "Item 1", value: 100 },
  { name: "Item 2", value: 200 }
];

// Table auto-updates when items array changes
function addItem() {
  items.push({ name: "New Item", value: 300 });
}
```

### Pattern 4: File Operations

**Load JSON:**
```javascript
// Client-side
OpenForum.loadJSON('/MyApp/data.json', function(data) {
  console.log('Loaded:', data);
});

// Or synchronous
var data = OpenForum.loadJSON('/MyApp/data.json');
```

**Save JSON:**
```javascript
var data = { name: "John", age: 30 };
OpenForum.saveJSON('/MyApp/data.json', data, function(result) {
  if (result.saved) {
    console.log('Saved successfully');
  }
});
```

**Check if file exists:**
```javascript
if (OpenForum.fileExists('/MyApp/config.json')) {
  // Load existing config
} else {
  // Create default config
}
```

### Pattern 5: Calling Java APIs from Server-Side JS

**Server-side (get.sjs):**
```javascript
var action = transaction.getParameter("action");

if (action === "callJavaApi") {
  try {
    // Get registered Java API
    var myApi = js.getApi("MyCustomAPI");

    if (!myApi) {
      transaction.sendJSON(JSON.stringify({
        success: false,
        error: "API not available"
      }));
      return;
    }

    // Call Java method
    var result = myApi.doSomething(param1, param2);

    transaction.sendJSON(JSON.stringify({
      success: true,
      result: result
    }));

  } catch (e) {
    transaction.sendJSON(JSON.stringify({
      success: false,
      error: "Exception: " + e.toString()
    }));
  }
  return;
}
```

## Client-Side JavaScript (OpenForum.js API)

### Core Functions
- `OpenForum.getVersion()` - Get OpenForum version
- `OpenForum.getBuildDate()` - Get build date/time
- `OpenForum.includeScript(scriptFileName)` - Dynamically load scripts
- `OpenForum.init()` - Called after dependencies load (define in page.js)
- `OpenForum.scan()` - Scan page for data-bound elements
- `OpenForum.onHash(hash)` - Handle URL hash changes

### Data Binding
- **{{variable}}** syntax in HTML for automatic UI binding
- **of-id attribute**: Bind element to JavaScript variable
  ```html
  <input of-id="user.name" type="text"/>
  <span>{{user.name}}</span>
  ```
- **OpenForum.getObject(id)** - Get data-bound object by ID
- **Two-way binding**: Changes in UI update variables, variable changes update UI
- **Auto-scanning**: Runs every 500ms by default to sync UI and data

### Tables and Repeating Data
- **of-repeatFor attribute**: Data-driven table rendering
  ```html
  <tr of-repeatFor="item in items">
    <td>{{item.name}}</td>
    <td>{{item.value}}</td>
  </tr>
  ```
- Automatically updates when collection changes

### File Operations
- `OpenForum.loadFile(fileName, callBack, noCache)` - Load file via AJAX
- `OpenForum.saveFile(fileName, data, callBack)` - Save file
- `OpenForum.loadJSON(fileName, callBack, noCache)` - Load and parse JSON
- `OpenForum.saveJSON(fileName, data, callBack)` - Save as JSON
- `OpenForum.deleteFile(pageName, fileName, callBack)` - Delete file
- `OpenForum.copyFile(fileName, toFileName, callBack)` - Copy file
- `OpenForum.moveFile(fileName, toFileName, callBack)` - Move file
- `OpenForum.fileExists(fileName)` - Check if file exists
- `OpenForum.getAttachments(pageName, callBack, matching, withMetaData)` - Get page attachments

### DOM Manipulation
- `OpenForum.setElement(id, content)` - Set element innerHTML
- `OpenForum.appendToElement(id, content)` - Append to element
- `OpenForum.showElement(id)` - Show element (display: block)
- `OpenForum.hideElement(id)` - Hide element (display: none)
- `OpenForum.toggleElement(id)` - Toggle element visibility
- `OpenForum.copyElement(id)` - Copy element text to clipboard
- `OpenForum.copyData(data)` - Copy data to clipboard

### Utilities
- `OpenForum.getParameter(name)` - Get URL parameter value
- `OpenForum.getCookie(name)` - Get cookie value
- `OpenForum.evaluate(script)` - Safely evaluate JavaScript
- `OpenForum.clone(object)` - Deep clone object
- `OpenForum.isEqual(a, b)` - Deep equality check
- `OpenForum.waitFor(test, callback, pause, timeout)` - Wait for condition
- `OpenForum.queue(process, supplyState)` - Queue async operations
- `OpenForum.setInterval(fn, timePeriod, immediate, blocking, doesCallback)` - Enhanced setInterval

### AJAX/JSON Helpers
- `JSON.get(page, action, parameters)` - GET request returning JSON
- `JSON.post(page, action, parameters)` - POST request with JSON
- `OFX.get(url).withAction(action).withData(data).onSuccess(callback).go()` - Fluent AJAX API

### Browser Utilities
- `OpenForum.Browser.download(fileName, data)` - Trigger file download
- `OpenForum.Browser.upload(callback, onError)` - Trigger file upload dialog
- `OpenForum.Browser.overrideSave(fn)` - Override Ctrl+S to call custom function

### Storage
- `OpenForum.Storage.get(key)` - Get from localStorage
- `OpenForum.Storage.set(key, value)` - Set in localStorage
- `OpenForum.Storage.find(regex)` - Find keys matching regex

## Data Binding Deep Dive

OpenForum's data binding system is powered by `/OpenForum/Javascript/open-forum.js` which is automatically loaded on every page.

### The `of-id` Attribute

**Syntax:** `of-id="objectName"` or `of-id="object.property"`

Links an HTML element to a JavaScript variable. Changes in either direction are automatically synchronized.

**Supported Elements:**
- `<input type="text">` - Binds to string value
- `<input type="checkbox">` - Binds to boolean value
- `<select>` - Binds to selected value
- `<select multiple>` - Binds to array of selected values
- `<textarea>` - Binds to text content
- Any element with `innerHTML` - For display only (use with `{{}}`)

**Example:**
```html
<input of-id="username" type="text" />
<input of-id="agree" type="checkbox" />
<select of-id="color">
  <option value="red">Red</option>
  <option value="blue">Blue</option>
</select>
```

```javascript
var username = "John";
var agree = false;
var color = "red";

// These variables stay in sync with the form automatically!
```

### The `{{variable}}` Syntax

**Syntax:** `{{variableName}}` or `{{object.property}}`

Embeds JavaScript variable values into HTML. Updates automatically when the variable changes.

**Example:**
```html
<p>Welcome, {{username}}!</p>
<p>Your selected color is: {{color}}</p>
<p>Terms agreed: {{agree}}</p>
```

### The `of-repeatFor` Attribute

**Syntax:** `of-repeatFor="item in arrayName"`

Creates dynamic lists or tables that automatically update when the array changes.

**Example:**
```html
<table>
  <tr of-repeatFor="user in users">
    <td>{{user.name}}</td>
    <td>{{user.email}}</td>
    <td>{{user.index}}</td>
  </tr>
</table>
```

```javascript
var users = [
  { name: "Alice", email: "alice@example.com" },
  { name: "Bob", email: "bob@example.com" }
];

// Add a user - table auto-updates!
users.push({ name: "Charlie", email: "charlie@example.com" });

// Note: Each item gets an 'index' property automatically
```

### Auto-Scanning

OpenForum automatically scans the page every 500ms (default) to keep everything in sync:

```javascript
// Runs every 500ms automatically
OpenForum.scan();

// Change scan interval (in milliseconds)
OpenForum.startAutoScan(1000); // Scan every 1 second

// Stop auto-scanning
OpenForum.stopAutoScan();

// Manual scan
OpenForum.scan();
```

### How Scanning Works

1. **Page Load:**
   - `OpenForum.crawl()` finds all `of-id`, `{{}}`, and `of-repeatFor` elements
   - Creates `OpenForumObject` instances for each variable
   - Links DOM elements to JavaScript variables

2. **Every Scan (500ms):**
   - Checks if form values changed → updates JavaScript
   - Checks if JavaScript values changed → updates DOM
   - Refreshes `{{}}` displays
   - Re-renders `of-repeatFor` tables if array changed

3. **Result:**
   - Bidirectional sync with no manual code!

### Nested Objects

You can use dot notation for complex data structures:

```html
<input of-id="config.database.host" />
<input of-id="config.database.port" />
<input of-id="config.database.name" />
```

```javascript
var config = {
  database: {
    host: "localhost",
    port: "5432",
    name: "mydb"
  }
};
```

### Listeners for Custom Logic

You can listen for changes to data-bound variables:

```javascript
// Add listener
OpenForum.addListener('username', function(obj) {
  console.log('Username changed to:', obj.getValue());
});

// Remove listener
OpenForum.removeListener('username', listenerFunction);
```

### Best Practices for Data Binding

1. **Initialize variables before page load** - Define defaults in page.js
2. **Use meaningful names** - `config.host` not just `h`
3. **Group related fields** - Use objects like `config.database.*`
4. **Validate before submission** - Data binding doesn't validate
5. **Clean up before sending** - Remove empty optional fields

## Special Pages and Built-In Services

### Core Services
- **Authentication** (`/OpenForum/Authentication`) - User sign-in
- **Authorization** (`/OpenForum/Authorization`) - Permission management
- **Actions** (`/OpenForum/Actions/*`) - Built-in web services
  - `/OpenForum/Actions/Save` - Save files
  - `/OpenForum/Actions/Delete` - Delete files
  - `/OpenForum/Actions/Copy` - Copy files/pages
  - `/OpenForum/Actions/Move` - Move files/pages
  - `/OpenForum/Actions/Attachments` - List attachments
- **Extensions** - Reusable markup elements (shortcuts to complex page elements)
- **Templates** - Page templates with inheritance
- **Error Pages** - Template-based error display

### Content Management
- **Deleted Pages** (`/OpenForum/DeletedPages`) - Archive for recovery
- **Page History** - Automatic version control in history/ subdirectories
- **Site Explorer** - Tree-view navigation and editing
- **Page Editor** - Resource management with plugin support

### Advanced Features
- **System Monitor** - Server state tracking with live logs
- **Message Queue** - Transient data service (2-minute persistence)
- **Spider** - Asynchronous page processing
- **Triggers** - Four event types:
  1. Startup triggers (run when server starts)
  2. Timer triggers (execute every 10 seconds)
  3. Page change triggers (fire when pages are modified)
  4. Rebuild triggers (run during rebuild operations)
- **SQL Integration** - Database connectivity support
- **Giraffe** - Graphics engine for animations and visualizations
- **Users and Groups** - Profile and credential management

## Giraffe Graphics Library

Create visual elements with objects like:
- Line, Rectangle, Circle, Polygon, Text
- Properties for styling, colors, shadows
- Event handlers: `onClick`, `onMouseOver`

## Foundation Framework

OpenForum uses Foundation v5.5.3 for responsive design:
- 12-column grid system
- Form elements and UI components
- Buttons, alerts, and style variants
- Initialize with: `$(document).foundation()`

## Error Handling Best Practices

### Server-Side Error Handling

**Always include error handling in .sjs files:**

```javascript
// Server-side (get.sjs or post.sjs)
try {
  var result = doSomething();
  transaction.sendJSON(JSON.stringify({ success: true, result: result }));
} catch (e) {
  transaction.sendJSON(JSON.stringify({
    success: false,
    error: e.toString()
  }));
}
```

**Validate parameters:**
```javascript
var action = transaction.getParameter("action");
var param = transaction.getParameter("param");

if (!param) {
  transaction.sendJSON(JSON.stringify({
    success: false,
    error: "Parameter 'param' is required"
  }));
  return;
}
```

### Client-Side Error Handling

**Handle network errors:**
```javascript
try {
  const response = await fetch(url);
  const result = await response.json();

  if (result.success) {
    handleSuccess(result);
  } else {
    handleError(result.error);
  }
} catch (error) {
  alert('Network error: ' + error.message);
}
```

**Check raw responses when debugging:**
```javascript
const response = await fetch(url);
const text = await response.text();
console.log('Raw response:', text);

// Then try to parse
try {
  const json = JSON.parse(text);
} catch (e) {
  console.error('Not valid JSON:', e);
}
```

## Common Pitfalls

### ❌ Don't: Mix concerns

```javascript
// Bad: Everything in one giant function
function doEverything() {
  var data = getFormData();
  validateData(data);
  callServer(data);
  updateUI();
  saveToLocalStorage();
  // ... 100 more lines
}
```

### ✅ Do: Separate concerns

```javascript
// Good: Small focused functions
function buildConfig() { ... }
function validateConfig(config) { ... }
function submitConfig(config) { ... }
function displayResult(result) { ... }
```

### ❌ Don't: Forget to refresh

After editing files, always refresh the page:
```javascript
// MCP tool
{ "action": "refresh", "page_name": "MyApp/Page" }
```

### ❌ Don't: Use hardcoded URLs

```javascript
// Bad
fetch('http://localhost/MyApp/Page?action=save');

// Good - relative URL
fetch('/MyApp/Page?action=save');
```

### ❌ Don't: Trust user input

```javascript
// Bad - no validation
var value = transaction.getParameter("value");
doSomethingDangerous(value);

// Good - validate first
var value = transaction.getParameter("value");
if (!value || value.length > 100) {
  transaction.sendJSON(JSON.stringify({ error: "Invalid input" }));
  return;
}
```

### ❌ Don't: Skip validation on server

```javascript
// Bad - only client-side validation
// Client can be bypassed!

// Good - validate on both client AND server
// Client-side for UX, server-side for security
```

### ❌ Don't: Forget the get.sjs/post.sjs handler when using Web Service Builder

```javascript
// Bad - Only updating Actions.sjs and ActionsClient.js
// The endpoint won't work without the handler in get.sjs!

// Good - Update all four files when adding a REST endpoint:
// 1. Actions.sjs - implementation
// 2. ActionsClient.js - client wrapper
// 3. get.sjs or post.sjs - request handler (MOST COMMONLY FORGOTTEN!)
// 4. get-service-definition-config.json or post-service-definition-config.json - service definition
```

## Debugging Tips

### 1. Server-Side Debugging

**Add logging to .sjs files:**
```javascript
// In .sjs file
js.println("DEBUG: action=" + action);
js.println("DEBUG: param=" + transaction.getParameter("param"));
```

**Check server logs/console** for output

### 2. Client-Side Debugging

**Use console.log:**
```javascript
console.log('Config:', config);
console.log('Response:', result);
```

**Use browser DevTools:**
- **Network tab**: See API calls and responses
- **Console tab**: See errors and logs
- **Elements tab**: Inspect DOM and data binding

### 3. Check Data Binding

**Inspect OpenForum objects:**
```javascript
// Get data-bound object
var obj = OpenForum.getObject('config');
console.log('Current value:', obj.getValue());

// List all data-bound objects
console.log(OpenForum.objects);
```

### 4. Test API Availability

**Check if Java API is registered:**
```javascript
// In .sjs file
var api = js.getApi("MyAPI");
if (!api) {
  js.println("API not available!");
  // List available APIs
  js.println("Available APIs: " + js.getApiNames());
}
```

### 5. Validate JSON Responses

**Check response format:**
```javascript
const response = await fetch(url);
console.log('Status:', response.status);
console.log('Content-Type:', response.headers.get('content-type'));

const text = await response.text();
console.log('Raw:', text);
```

## Development Best Practices

1. **Use OpenForum markup by default** - Don't use Markdown unless explicitly asked
2. **Include HTML when needed** - OpenForum markup can be mixed with HTML
3. **Always specify page paths correctly** - Use format like "HomeLab/Project/Page"
4. **Set auto_refresh: true** - To immediately see rendered output
5. **Use convert action** - When you have Markdown that needs to be converted
6. **Include page metadata** - Add author, description, keywords in page_data
7. **Read before update** - Always read existing pages before modifying
8. **Server-side for security** - Use .sjs files for sensitive operations, not client JavaScript
9. **Leverage data binding** - Use `of-id` and `{{}}` for reactive UIs
10. **Use Actions for common tasks** - Built-in Actions handle save/delete/copy/move operations
11. **Follow the Web Service Builder pattern** - When adding REST endpoints, update all four files: .sjs, Client.js, get.sjs/post.sjs, and service-definition-config.json

## Important Notes

- OpenForum is a live system - changes take effect immediately
- The system maintains history of all changes in `history/` subdirectories
- You can mix OpenForum markup with raw HTML when needed
- Always use forward slashes in page paths, never backslashes
- Pages auto-render when created/updated (unless auto_refresh: false)
- Access controls inherit from parent pages unless overridden
- Foundation JavaScript requires jQuery

## Summary: Key Principles for Success

### 1. Start Simple, Iterate Quickly
- Break tasks into small, testable steps
- Create minimal working version first
- Test after every change
- Fix issues before continuing

### 2. Leverage Data Binding
- Use `of-id` for automatic two-way binding
- Use `{{}}` for display values
- Use `of-repeatFor` for dynamic tables
- Eliminates most manual DOM manipulation

### 3. Separate Concerns
- Small, focused functions
- Client-side for UX, server-side for security
- Validate on both client AND server
- Keep business logic in .sjs files

### 4. Error Handling is Essential
- Always use try-catch in .sjs files
- Validate all parameters
- Handle network errors gracefully
- Provide meaningful error messages

### 5. Test Thoroughly
- Test after each change
- Test edge cases (empty inputs, invalid data)
- Use browser DevTools and console logging
- Check API availability before using

### 6. Documentation Matters
- Use meaningful variable names
- Comment complex logic
- Keep page metadata up to date
- Track development progress

## Quick Reference

### Creating a New Page
```javascript
{
  "action": "create",
  "page_name": "MyApp/Page",
  "page_title": "Page Title",
  "page_content": "!!Heading\n\n((Content))",
  "page_data": { "author": "Your Name" }
}
```

### Data Binding Pattern
```html
<!-- HTML -->
<input of-id="config.host" type="text" />
<span>{{config.host}}</span>

<!-- JavaScript -->
var config = { host: "localhost" };
```

### Server-Side Handler
```javascript
// get.sjs
var action = transaction.getParameter("action");
if (action === "save") {
  try {
    // Process request
    transaction.sendJSON(JSON.stringify({ success: true }));
  } catch (e) {
    transaction.sendJSON(JSON.stringify({ success: false, error: e.toString() }));
  }
  return;
}
transaction.setResult(transaction.SHOW_PAGE);
```

### Client-Side Fetch
```javascript
try {
  const response = await fetch('/MyApp/Page?action=save');
  const result = await response.json();
  if (result.success) {
    // Handle success
  } else {
    alert('Error: ' + result.error);
  }
} catch (error) {
  alert('Network error: ' + error.message);
}
```

## Resources

### Example Projects
- `/OpenForum/AddOn/Mailer/` - Java API integration example
- `/OpenForum/Actions/` - Server-side actions
- `/OpenForum/Extensions/` - Markup extensions

### Documentation
- Local: `/web/content/default/OpenForum/Documentation/`
- Remote: https://open-forum.onestonesoup.org/OpenForum/Documentation

### Tools
- **Site Explorer** - Browse and edit pages
- **Page Editor** - Edit page content
- **System Monitor** - View server state and logs

---

**Remember:** OpenForum's killer feature is automatic two-way data binding. Use it to eliminate boilerplate and build reactive UIs with minimal code!
