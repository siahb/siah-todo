# SiahDo

Siahverse's personal task and routine manager, built with HTML, CSS, and vanilla JavaScript.

**App:** [todo.siahverse.cc](https://todo.siahverse.cc)  
**Portal:** [siahverse.cc](https://siahverse.cc)

## Features

- Add and edit tasks with tags, priorities, and due dates.
- Schedule daily or weekly repeats, including selected weekdays.
- Complete a repeating task to advance its next occurrence.
- Search tasks and filter by tags or priority.
- Sort by priority, due date, name, tags, or creation date.
- Drag to reorder tasks and save a custom order.
- Select multiple tasks, confirm deletion, and undo supported changes.
- Track completion progress and overdue tasks.
- Switch between light and dark themes.

## How to use

1. Open the app and use **Enter Password** to enable task changes.
2. Enter a task, set its properties, and press **Enter**.
3. Mark tasks done, or edit them inline.
4. Use **Reorder** with custom sorting to arrange the list.

Search shortcuts include `/` and `Ctrl+F`; press `Esc` to close search. Filters include `untagged`, `no-tags`, `p:high`, `p:medium`, `p:low`, and `p:none`.

## Repository layout

| Path | Purpose |
| --- | --- |
| `public/index.html` | Frontend used by the deployment workflow |
| `public/todo.css` | Deployed frontend styles |
| `public/todo.js` | Deployed task logic and API requests |
| `index.html`, `todo.css`, `todo.js` | Separate root-level development copies |
| `.github/workflows/deploy.yml` | Deploys `public/` to the homelab server |
| `favicon.svg`, `manifest.json`, `_headers` | Supporting root-level assets |

Root and `public/` files are separate copies. Changes intended for the existing deployment workflow must be made in `public/`.

## Local preview

No frontend build step is required:

```sh
git clone https://github.com/siahb/siah-todo.git
cd siah-todo
python3 -m http.server 8080 --directory public
```

Open `http://localhost:8080`. This previews the interface; loading and saving tasks requires a compatible API on the same origin or a configured development backend. This repository does not include the API server.

## Backend contract

The frontend uses a task API:

| Method | Route | Purpose |
| --- | --- | --- |
| GET | `/todos` | Load the task list |
| POST | `/todos` | Add a task |
| PATCH | `/todos/:index` | Update a task |
| DELETE | `/todos/:index` | Delete a task |
| POST | `/todos/reorder` | Save the reordered list |

Task data is saved by the backend, not as a standalone offline browser database. The frontend stores the entered admin password in browser local storage and sends it as a bearer credential for protected requests; logging out removes it.

The [Siahverse repository](https://github.com/siahb/siahverse) also contains a newer Cloudflare Pages Functions/D1 implementation and a separate copy of the frontend. That implementation is maintained there; the homelab deployment workflow in this repository remains distinct.

## Deployment

The existing GitHub Actions workflow runs on changes to `public/**` or the workflow file on `main`. It copies `public/` to the server using SSH and rsync. Configure the repository secrets `SSH_HOST`, `SSH_PORT`, `SSH_USER`, and `SSH_KEY` for that workflow.

README-only changes do not trigger this deployment.

Made with Siahverse by Josiah Borja.
