<h1 align="center">CS-360 Mobile Architecture and Programming</h1>

<h2 align="center">Inventory Tracker Mobile Application</h2>

### Table of Contents
<!-- toc-start -->
- [.](.)
<!-- toc-end -->




### Project Summary

For CS 360, I designed and developed Inventory Tracker, an Android application for warehouse employees, inventory managers, and small business owners. The app provides a simple way to monitor inventory quantities, update stock records, and identify items that need to be replenished.

The application allows a new user to create an account and an existing user to log in with saved credentials. After logging in, the user can add inventory items and view them on the Inventory Dashboard. Each record includes the item name, SKU, quantity, warehouse location, and low-stock threshold. The user can increase or decrease an item’s quantity and delete records that are no longer needed. The app also offers optional SMS alerts when an item reaches zero stock.

The primary user need was dependable inventory tracking without requiring a complicated warehouse-management system. Warehouse employees need to update quantities quickly. Inventory managers need an understandable view of current stock levels. Small business owners may need a lightweight tool that does not require expensive software or extensive training. I designed the app around these practical needs.

### User-Centered UI Design

The application required the following interface components:

- A Login and Create Account screen
- An Inventory Dashboard
- An Add Inventory Item screen
- A reusable inventory-row layout
- A collapsible SMS notification settings section

The Login screen contains fields for a username and password, along with separate controls for logging in and creating an account. The password field obscures the entered text to protect the user’s credentials.

The Inventory Dashboard displays saved records through a scrolling `RecyclerView`. Each inventory card groups related information and places the quantity where it can be identified quickly. The Increase and Decrease controls appear near the quantity information, while the destructive Delete Item action is visually separated. Stock conditions are communicated through visible status text as well as color, so users do not have to rely on color alone.

The Add Inventory Item screen uses a clear top-to-bottom form. It collects the item name, SKU, quantity, warehouse location, and low-stock threshold. Labels, examples, predictable focus order, scrolling support, and large touch targets make the form easier to complete on different screen sizes.

One important design challenge appeared when the SMS settings took up too much space on the Inventory Dashboard. The inventory list technically scrolled, but the visible area was too small to show a complete inventory card comfortably. I solved this by making the SMS settings collapsible. The SMS heading remains available, but users can collapse the optional settings when they want more room for inventory records.

This experience reminded me that a feature can work correctly and still need refinement before it becomes useful.

The UI was successful because it received full credit and positive instructor feedback. More importantly, the final design gives each screen a clear purpose. The project uses a consistent Material 3 color system, readable typography, related content groups, and predictable action placement.

### Development Approach

I approached development in stages rather than trying to make the complete application functional at once. I created the visual interface first and used it as a blueprint for the Java implementation. This helped me identify the required data before writing the database code.

I used Java for the application logic and SQLite for persistent storage. Separate database tables store user accounts and inventory records. The inventory table stores the following values:

- Item name
- SKU
- Quantity
- Warehouse location
- Low-stock threshold

The completed database supports creating, reading, updating, and deleting inventory records.

I divided the code into focused classes instead of placing everything inside one activity:

- `MainActivity` handles login and account creation.
- `InventoryActivity` manages the dashboard and inventory interactions.
- `AddItemActivity` validates and saves new records.
- `InventoryDatabaseHelper` controls SQLite operations.
- `InventoryAdapter` connects database records to the `RecyclerView`.
- `InventoryItem` represents a single inventory record.
- `SmsNotificationHelper` contains SMS-specific behavior.

I also used descriptive method names and comments for behavior that might not be immediately obvious. Examples include methods for loading inventory records, updating SMS controls, validating permission, and sending a zero-stock alert. This structure made troubleshooting easier because each class had a defined responsibility.

I can apply this staged strategy to future projects by separating interface design, data modeling, implementation, and testing. Building in smaller sections makes it easier to locate errors and reduces the chance that one unfinished feature will interfere with every other part of the application.

### Testing and Validation

I tested the application frequently with Android Studio rather than waiting until every feature was complete. My testing included:

- New account creation
- Valid login
- Invalid login
- Blank credentials
- Persistent user records
- Inventory item creation
- Quantity increases and decreases
- Inventory record deletion
- Database persistence after restarting the application
- SMS permission approval
- SMS permission denial
- Application-level SMS enable and disable controls
- Light and dark themes
- Scrolling and responsive layouts

I tested both SMS permission outcomes. When permission is granted and alerts are enabled, the app can attempt to send a message when an item reaches zero stock. When permission is denied, Inventory Tracker continues to support login and inventory management without sending an SMS. The user can also disable alerts at the application level without affecting the rest of the app.

Visual testing was just as important as functional testing. At one point, the Add Inventory Item form appeared compressed because the scrollable layout and constraints did not work together correctly. I reorganized the layout by using a constrained `NestedScrollView` with a vertical form container. This preserved scrolling while keeping the fields in the intended order.

Testing also revealed several configuration problems. The `RecyclerView` and `CardView` dependencies initially appeared unused or unresolved in the Gradle version catalog. I compared the aliases in `libs.versions.toml` with the references in the app-level Gradle file and corrected the naming mismatch.

I also found that the filled Material 3 button style had been referenced with an invalid resource name. Replacing it with the supported Material 3 button style restored the build.

This process was important because a successful layout preview did not always mean the complete project would compile or behave correctly. Testing exposed visual, configuration, persistence, and permission issues that were not obvious during initial development.

### Innovation and Problem Solving

The area where I had to innovate most was balancing the optional SMS controls with the inventory list. Both features were required, but keeping the entire SMS section fixed left too little room for inventory records. Removing the SMS controls was not acceptable, and wrapping everything in another `ScrollView` could interfere with `RecyclerView` behavior.

I addressed the conflict by creating a collapsible SMS settings card. The application remembers whether the section was expanded or collapsed through `SharedPreferences`. The saved SMS phone number and alert setting are also preserved separately. This means collapsing the card changes only its presentation. It does not silently enable or disable notifications.

Another discovery involved the difference between Android’s system permission and the app’s own alert preference. Granting `SEND_SMS` permission does not mean the user should lose the ability to turn alerts off. I added an application-level Enable or Disable control. Disabling the feature stops Inventory Tracker from sending alerts without attempting to revoke the Android permission itself.

These challenges taught me that mobile development often involves improving the relationship between features rather than treating every requirement as an isolated task.

### Demonstration of Knowledge and Skills

I was particularly successful in developing the Inventory Dashboard and connecting it to persistent data. This component brings together:

- The `RecyclerView`
- The SQLite database
- The reusable inventory-row layout
- Material 3 interface components
- Quantity controls
- Stock-status logic
- SMS notification settings

The dashboard demonstrates my ability to translate a visual design into a functional mobile workflow. Inventory records are loaded from SQLite and displayed through the adapter. Quantity controls update individual database records, and the Delete Item control removes the selected record. The list refreshes after changes, while the status text identifies whether an item is in stock, low in stock, or out of stock.

This project also demonstrates my growth in debugging Android layouts and build configuration. I learned to distinguish an XML design problem from a Gradle dependency problem or a Java behavior problem. That distinction helped me correct issues without repeatedly changing unrelated files.

Looking back, the most valuable result is not only the finished application. I now have a clearer understanding of how user-centered design, persistent data, runtime permissions, testing, and maintainable Java code work together in an Android project.

Inventory Tracker represents both the completed product and the development process I can carry into future software projects.

### Technologies and Development Tools

- Java
- Android Studio
- Android SDK
- SQLite
- Material Design 3
- `ConstraintLayout`
- `NestedScrollView`
- `RecyclerView`
- `SharedPreferences`
- Android runtime permissions
- Gradle version catalogs
- Android Emulator
- Git and GitHub
- Markdown

### Portfolio Artifacts

The portfolio artifacts for this course are the completed Project Three Android Studio project and the finalized App Launch Plan. The Android Studio project contains the finalized user interface developed during Project Two and the functional Java application completed during Project Three. The App Launch Plan documents the proposed store listing, Android compatibility, required permissions, monetization approach, testing process, and release strategy.

Together, these artifacts demonstrate the development of Inventory Tracker from its initial user interface design to a functional mobile application with a practical launch strategy.


- [Jacob_Garrett_Inventory_Tracker_App_Code.zip](./Jacob_Garrett_Inventory_Tracker_App_Code.zip)
- [Jacob_Garrett_Inventory_Tracker_Launch_Plan.docx](./Jacob_Garrett_Inventory_Tracker_Launch_Plan.docx)


Together, these artifacts demonstrate experience with:

- Java-based Android development
- Material 3 interface design
- ConstraintLayout and scrollable layouts
- SQLite database persistence
- Database-backed account creation and login
- RecyclerView and adapter implementation
- Inventory CRUD operations
- Runtime SMS permissions
- Application-level notification preferences
- Emulator testing
- Gradle troubleshooting
- User-centered mobile design

### Repository Contents

```text
Inventory-Tracker/
├── ./README.md
├── ./Jacob_Garrett_Inventory_Tracker_Project_Three.zip
└── ./Jacob_Garrett_Inventory_Tracker_Launch_Plan.docx
```
<br>
<br>
<br>
<h4 align="right">This assignment is dedicated to Professor DiMarizo.<br>Thank you sir,<h4>
<h4 align="right">Jacob Garrett<h4>
