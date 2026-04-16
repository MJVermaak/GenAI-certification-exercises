README and User Guide Documentation Submission
1. Prompt 1: Project README (README.md)
Markdown
# JavaScript Task Manager 

A lightweight, modular Node.js utility for advanced task management. This project provides core algorithms for parsing natural language into tasks, sorting tasks by dynamic priority scores, and performing two-way synchronization between local and remote task lists.

## Features
* **Natural Language Parsing**: Create structured tasks from strings using markers like `!urgent`, `@frontend`, and `#tomorrow`.
* **Smart Priority Sorting**: Automatically sort tasks based on a weighted algorithm factoring in base priority, due dates, tags, and completion status.
* **Two-Way Syncing**: Merge local and remote task lists seamlessly with built-in conflict resolution (timestamp-based and status-override rules).

## Installation
**Prerequisites:** Node.js (v14+)
1. Clone the repository: `git clone <repository-url>`
2. Navigate to the javascript directory: `cd use-cases/code-comprehension-001/javascript`
3. Install dependencies: `npm install`

## Basic Usage
```javascript
const { parseTaskFromText } = require('./src/algorithms/textParser');
const task = parseTaskFromText("Fix login bug !urgent @frontend #today");
console.log(task);
Code Structure
src/models/: Contains the core domain models (Task.js, TaskStatus.js).

src/algorithms/: Contains the standalone pure functions for logic (textParser.js, prioritySorter.js, listMerger.js).

Troubleshooting
Tasks not sorting correctly? Ensure your task dates are valid JavaScript Date objects. String dates will break the math in the priority sorter.

Sync conflicts overwriting data? Remember that a DONE status always wins during a merge, regardless of which task was updated most recently.

Contributing & License
Contributions are welcome! Please open a Pull Request with updated unit tests for any algorithmic changes. This project is licensed under the MIT License.


### 2. Prompt 2: Step-by-Step Guide
**Document:** How to Merge Local and Remote Task Lists
**User Level:** Intermediate

**Prerequisites:** * Both local and remote tasks must be formatted as JavaScript Objects (Dictionaries) where the key is the `taskId` and the value is the `Task` object.

**Step 1: Import the Merger Utility**
At the top of your file, require the list merger algorithm.
```javascript
const { mergeTaskLists } = require('./src/algorithms/listMerger');
Step 2: Execute the Merge
Pass your local task object and your remote task object into the function.

JavaScript
const result = mergeTaskLists(localTasks, remoteTasks);
Step 3: Handle the Output
The function returns a categorized object. You must handle each bucket appropriately to keep your databases in sync:

Save result.mergedTasks to update your UI.

Send result.toCreateRemote and result.toUpdateRemote to your backend API via a POST/PUT request.

Save result.toCreateLocal and result.toUpdateLocal to your local offline cache (e.g., LocalStorage or IndexedDB).

Common Mistakes:

Passing arrays instead of objects. The algorithm specifically requires objects structured as { "id_123": TaskObject } to easily look up missing IDs.

3. Prompt 3: FAQ Document
Task Manager FAQ

Q: Does this Task Manager include a database or frontend UI?
A: No. This project is strictly a collection of backend utility algorithms. You can plug these JavaScript algorithms into any Express.js backend or React frontend.

Q: How does the priority sorter calculate what is most important?
A: It uses a weighted scoring system. It starts with a base score (e.g., URGENT = 40 points), adds massive bonuses for overdue tasks (+30 points), adds minor bonuses for tags like "blocker" (+8 points), and heavily penalizes completed tasks (-50 points) to push them to the bottom.

Q: What happens if I type an invalid date format into the text parser?
A: The parser handles basic keywords (#tomorrow, #monday) and standard formats (#YYYY-MM-DD). If it doesn't recognize the format, it will silently fail, ignore the date string, and set the task's dueDate to null.

Q: When syncing lists, what happens if both the local and remote user changed the task title?
A: The algorithm looks at the updatedAt timestamp on both tasks. The task with the most recent timestamp wins, and its title will overwrite the older one.