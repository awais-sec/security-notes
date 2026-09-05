# Autopsy UI Basics

Short reference notes on the Autopsy user interface.

- **Basic UI flow**: Tree (left) → Table (upper right) → Content Viewer (bottom).

## Tree (Left Pane)

Five top-level nodes:

- **Data Sources**: file system structure, user view. Good for specific locations. Collapsed by default.
- **Views**: same files, organized by metadata (file type, allocation status, size).
  - **File Type**: by extension (fast, can be changed) or MIME type/signature (accurate, requires analysis).
  - **Size**: look for large files (encrypted volumes, etc.).
- **Results**: output from background analysis modules (extracted content, keyword hits, hash set hits, email, etc.).
- **Tags**: user and system tags on files.
- **Reports**: generated reports.

**Tree organization:**

- Default: merged data sources.
- Ungrouped (by data source): see content per device.
- Change grouping in Display Settings (gear icon), or you're prompted for it with more than 5 data sources.

## Table View (Upper Right Pane)

Lists items from the selected tree node.

- Default view. Columns can be moved and sorted.
- **Paging**: default 10,000 items for performance.
- **Search within table**: select an item, start typing (a magnifying glass appears).
- **S (Score) column**: red icon = notable (hash hit, notable tag); yellow icon = suspicious (interesting module result, normal tag).
- **C (Comments) column**: yellow notepad icon if comments exist (from tagging or the central repository).
- **O (Occurrences) column**: how often the item has been seen in past cases (requires the central repository).
- **Machine translation**: for non-English file/folder names (if configured).
- **Thumbnail view**: for pictures and videos, organized in pages.

## Content Viewer (Lower Right Pane)

Displays the selected item from the table.

- **Hex**: raw file content (offsets, hex, ASCII). Launch HxD for advanced editing.
- **Text**:
  - **Strings**: potential text in various encodings (like the Unix `strings` command; may have false positives).
  - **Indexed Text**: text extracted for keyword search (from known file types). Defaults to Strings for unknown types.
  - **Translation**: translated text (if configured).
- **Application**: viewer based on file type (image, video, SQLite, HTML, registry, etc.). Basic functionality provided. HTML rendering strips external links by default (can enable downloading).
- **Message**: dedicated viewer for email/text messages (headers, attachments).
- **File Metadata**: details about the selected file (timestamps, size, MIME type) and output of Sleuth Kit (file-system-specific info).
- **Results**: analysis results associated with the file (hash hits, keyword hits, etc.).
- **Annotations**: examiner-related data (tags, comments) from the current case and past cases (if the central repository is enabled).
- **Other Occurrences**: locations of the selected item in the current case and past cases (if the central repository is enabled).
- Default viewer selection is made by Autopsy (can be set to stay on the same viewer via Display Settings).

## Other UI Concepts

- **History buttons**: back and forward navigation.
- **Progress bars**: lower right, for background tasks (can be cancelled).
- **Ingest Inbox**: envelope icon for notifications from analysis modules.
- **File Search by Attributes**: Tools menu, for searching by metadata (name, size, date, etc.).
- **Right-click options**:
  - **Extract Files**: save a copy outside of Autopsy (defaults to the case folder).
  - **View in External Viewer**: opens with the associated program.
  - **View in New Window**: undocked content viewer that stays on the selected file.

## Additional Interfaces (Toolbar)

Specialized views for certain data types:

- **Timeline**: events sorted by time.
- **Image Gallery**: pictures and videos grouped by folder.
- **Communications**: accounts, messages, call logs.
