# JUnit Setup in Advance in IntelliJ  

---

## 🔹 Install JUnit Plugin
1. Open **IntelliJ IDEA**.  
2. Navigate to:  
   - **Windows/Linux**: `File -> Settings`  
   - **macOS**: `IntelliJ IDEA -> Preferences`  
3. In the left sidebar, select **Plugins**.  
4. In the **Marketplace** tab, search for **JUnit**.  
5. Install the **JUnit plugin** (if not already installed).  

---

## 🔹 Configure JUnit Library
1. Open the **Project Structure** dialog:  
   - Right-click your project in the **Project Explorer**  
   - Select **Open Module Settings**  
2. In the **Project Settings** section, click **Libraries**.  
3. If JUnit is not listed:  
   - Click the **+** button → Select **From Maven...**  
   - Search for **`junit:junit`** in the search bar.  
   - Choose the desired version, e.g.:  
     - `junit:junit:4.12`  
     - `org.junit.jupiter:junit-jupiter-api:5.8.0`  
   - Click **OK** to add the library.  

---

## 🔹 Configure JUnit for Tests
1. In the **Project Structure** dialog, go to **Modules** (left sidebar).  
2. Open the **Dependencies** tab.  
3. Verify that the **JUnit library** is listed in the **Module SDK** section.  
