# Hands-on Tutorial: Build Lightning Components with Salesforce DX

## Prerequisites setup:

1. **Github:** (Online) Create a free Github account if you don't have one yet. 
    1. Github account: https://github.com/join
2. **Dev Hub:** (Online) Create a free 30-day Salesforce trial org with Dev Hub feature enabled. You will need a Dev Hub enabled org to be able to some Salesforce DX features which we will discuss in a later section. If you already have a Salesforce org with Dev Hub enabled, you may use it.
    1. Salesforce Dev Hub trial org: https://developer.salesforce.com/promotions/orgs/dx-signup
3. **Git:** (Local Machine) Install the Git version control software for source code control and management.
    1. Git (includes command line tool and Git Bash terminal): https://git-scm.com/downloads
4. **Salesforce CLI:** (Local Machine) Install the Salesforce CLI tool to access Salesforce DX features and functionality directly from your local machine's command line.
    1. Salesforce CLI tool: https://developer.salesforce.com/tools/sfdxcli
5.  **VS Code:** (Local Machine) Install Visual Studio Code (VS Code) text editor, if you don't have it yet, which we will use for writing code and use as an IDE. VS Code is NOT the same as Visual Studio. 
    1. VS Code text editor: https://code.visualstudio.com/download
6. **Salesforce Extensions for VS Code:** (Local Machine) Once VS Code is installed, install the Salesforce Extensions for VS Code to access some Salesforce DX features and functionality directly from VS Code.
    1. Salesforce Extensions for VS Code: https://marketplace.visualstudio.com/items?itemName=salesforce.salesforcedx-vscode
7. **Command Line:** (Local Machine) Use Terminal if you are a Mac user. If you are a Windows user, for simplicity, use Git Bash. Git Bash is similar to the Terminal on Mac, and it usually comes installed along with Git. Note: For ease, wherever it says "Terminal" in the rest of the tutorial, it means "Git Bash" if you are using Windows.
    1. Terminal for Mac, Git Bash (usually comes with git installation) for Windows

## Hands-on Steps:

### Step 1:

Open Terminal and explore the Salesforce CLI by typing below commands and press enter, one by one, to get an idea of usage guidelines. Optionally, run the "sfdx update" command to ensure you are using the latest version of the CLI. The "--help" flag can be used for respective commands to get usage guidelines like examples and what flags (inputs) the command accepts.

```bash
sfdx update
sfdx --help
sfdx force
sfdx force --help
```

![1](tutorial/1.png)
### Step 2:

Create a new Salesforce DX project directory (folder) with the Salesforce CLI using the below commands, one by one. This will create a scaffolded (template) directory with necessary config files and sub-folders using the Salesforce DX format. Use terminal commands like "cd" and "ls" to switch into the directory and see the contents.

```
sfdx force:project:create --help
sfdx force:project:create -n LCwithDX
cd LCwithDX
ls
```

![2](tutorial/2.png)
### Step 3:

Initiate the project directory as a Git repository for source code management and version control.

```
git init
```

![3](tutorial/3.png)
### Step 4:

Open the VS Code text editor, and open our new folder "LCwithDX" in it. Check out the folder structure and default configuration files like "sfdx-project.json" (project configuration) and "config/project-scratch-def.json" (scratch org configuration).
![4](tutorial/4.png)![4a](tutorial/4a.png)![4b](tutorial/4b.png)
### Step 5:

Create a new Lightning Component named "quickContactView" using the below Salesforce CLI commands, one by one, including help commands for better understanding. Check out the newly created lightning component bundle under the "force-app/main/default/aura" directory  in VS Code.

```
sfdx force:lightning --help
sfdx force:lightning:component:create --help
sfdx force:lightning:component:create -n quickContactView -d force-app/main/default/aura
```

![5](tutorial/5.png)![5a](tutorial/5a.png)![5b](tutorial/5b.png)![5c](tutorial/5c.png)
### Step 6:

Remove the default markup in the "quickContactView.cmp" file, and paste the below code and save using ctrl+S or the Save button in VS Code's file menu.

```HTML
<aura:component implements="flexipage:availableForAllPageTypes,flexipage:availableForRecordHome,force:hasRecordId" access="global" >
    
    <lightning:card title="Quick view" iconName="standard:contact" >
        
        <lightning:recordViewForm recordId="{!v.recordId}" objectApiName="Contact">
            <p class="slds-p-horizontal_small">
                <lightning:outputField fieldName="FirstName" />
                <lightning:outputField fieldName="LastName" />
                <lightning:outputField fieldName="Email" />
                <lightning:outputField fieldName="Phone" />
            </p>
        </lightning:recordViewForm>
        
    </lightning:card>
    
</aura:component>
```

![6](tutorial/6.png)
### Step 7:

Create two more lightning components "quickContactEdit" and "quickContactViewEdit" using below commands and code and save them. The "quickContactViewEdit" component uses the "quickContactView" and "quickContactEdit" components in its markup.

```
sfdx force:lightning:component:create -n quickContactEdit -d force-app/main/default/aura/
```

![7](tutorial/7.png)
```html
<aura:component implements="flexipage:availableForAllPageTypes,flexipage:availableForRecordHome,force:hasRecordId" access="global" >
    
    <lightning:card title="Quick edit" iconName="utility:edit_form">
        
        <lightning:recordEditForm recordId="{!v.recordId}" objectApiName="Contact" >
            <lightning:messages />
            <p class="slds-p-horizontal_small">
                <lightning:inputField fieldName="FirstName" />
                <lightning:inputField fieldName="LastName" />
                <lightning:inputField fieldName="Email" />
                <lightning:inputField fieldName="Phone" />
                <lightning:button type="submit" label="Save" name="save" variant="brand" class="slds-m-top_small"/>
            </p>
        </lightning:recordEditForm>
        
    </lightning:card>
    
</aura:component>
```

![7a](tutorial/7a.png)
```
sfdx force:lightning:component:create -n quickContactViewEdit -d force-app/main/default/aura/
```

![7b](tutorial/7b.png)
```html
<aura:component implements="flexipage:availableForAllPageTypes,flexipage:availableForRecordHome,force:hasRecordId" access="global" >
    
    <aura:attribute name="editMode" type="Boolean" default="false" />
    
    <lightning:card title="Quick View/Edit">        
        
        <aura:if isTrue="{! !v.editMode }">
            <c:quickContactView recordId="{! v.recordId }"/>
        </aura:if>
        
        <aura:if isTrue="{! v.editMode }">
            <c:quickContactEdit recordId="{! v.recordId }"/>
        </aura:if>
        
        <aura:set attribute="actions">
            <lightning:buttonStateful state="{! v.editMode }" 
                                    iconNameWhenOff="utility:edit_form" 
                                    labelWhenOff="Click to Edit" 
                                    iconNameWhenOn="utility:check" 
                                    labelWhenOn="Done Editing" 
                                    onclick="{! c.toggleMode }"/>
        </aura:set>
        
    </lightning:card>   
    
</aura:component>
```

![7c](tutorial/7c.png)
### Step 8:

Add a Javascript method to the "quickContactViewEdit" component bundle by pasting below code in it's javascript controller file "quickContactViewEditController.js". This function gives view/edit mode toggling functionality to the "lightning:buttonStateful" button we use in the component markup.

```js
({
    toggleMode : function(component, event, helper) {
        component.set("v.editMode", !component.get("v.editMode"));
    }
})
```

![8](tutorial/8.png)
### Step 9:

Authenticate the Salesforce CLI with a Dev Hub enabled org to give the CLI the ability to create and manage scratch orgs. Make it the default Dev Hub for the CLI using the "-d" flag and give it an alias (short name) "devhub" using the "-a" flag in the command. When you enter the command, you will be prompted with a browser asking you for your Salesforce login, provide the Dev Hub org credentials, click "Allow" on the next screen and come back to the Terminal. You will see a message "Successfully authorized *some username* with org ID *some orgid*. You may now close the browser". At this point, you can close the Dev Hub org window in the browser and the Salesforce CLI now has the ability to create and manage scratch orgs.

```
sfdx force:auth --help
sfdx force:auth:web:login --help
```

![9](tutorial/9.png)
```
sfdx force:auth:web:login -d -a devhub
```

![9a](tutorial/9a.png)![9b](tutorial/9b.png)![9c](tutorial/9c.png)![9d](tutorial/9d.png)
### Step 10:

Create a new Scratch Org using the Salesforce CLI by providing a mandatory config file. We can just use the "config/project-scratch-def.json" file, which is a default scratch org configuration file that gets created when we create a Salesforce DX project. Make it the default scratch org using the "-s" flag and give it an alias (short name) "newscratch" using the "-a" flag.

```
sfdx force:org --help
```

![10](tutorial/10.png)
```
sfdx force:org:create --help
```

![10a](tutorial/10a.png)
```
sfdx force:org:create -s -a newscratch -f config/project-scratch-def.json
```

![10b](tutorial/10b.png)
### Step 11:

You can check the status of source code and metadata differences between the local machine and the scratch org using the "force:source:status" command. At this point, you will see all the files in the 3 new component bundles on the local machine that don't yet exist in the scratch. Push your local source code into the scratch org with the Salesforce CLI using the "force:source:push" command. Once the changes are pushed and then you check the status, we will see there are no more differences between the local machine and the scratch org.

```
sfdx force:source --help
```

![11](tutorial/11.png)
```
sfdx force:source:status
```

![11a](tutorial/11a.png)
```
sfdx force:source:push
sfdx force:source:status
```

![11b](tutorial/11b.png)
### Step 12:

Open the scratch org automatically with the Salesforce CLI using below command. You don't have to manually login to the scratch org as the CLI takes care of the authentication and launching it in the browser. At this point, you can find the 3 new components in the scratch org under "Setup | Custom Code | Lightning Components".

```
sfdx force:org:open
```

![12](tutorial/12.png)![12a](tutorial/12a.png)![12b](tutorial/12b.png)
### Step 13:

Add the "quickContactViewEdit" component to the contact detail page using Lightning App Builder and test the component. Refer to below Steps and the screenshots gif.

1. Click on the App Launcher icon in the left top corner
2. Click on the "Sales" app.
3. Go to the contacts tab in the Sales app and click the "New" button to create a new contact record with any name and contact details.
4. On the new Contact Record Home page (Contact detail page), click on the Setup icon in right top corner and click on "Edit Page" to launch the Lightning App builder.
5. Drag the "quickContactViewEdit" component from the custom components section on the left and drop it above the "Activity/Chatter" component towards the right in the canvas.
6. Click "Save" in the right top corner and on the next popup, click "Activate".
7. Click "Assign as Org Default" on next screen.
8. Click "Save" on next screen.
9. Click the "Back" button in the right top corner of Lightning App Builder to return to the Contact Record Home page.
10. Check out the component's functionality. It's in View mode by default, click on "Click to Edit" for Edit Mode.
11. Make changes, click "Save", and then click "Done Editing" to return to View Mode.

![13](tutorial/13.gif)
### Step 14:

At this point, there is new metadata in the scratch org, the new "Contact Record Page" lightning page (flexipage), which doesn't yet exist on your local machine. When you do "force:source:status" on local machine, we will see changes available to be synced in the "Remote" (scratch org). Pull the new metadata (which is also code) into the local source code (Salesforce DX project) using the Salesforce CLI command "force:source:pull". There is now a "flexipages" sub-directory (folder) with a file "Contact_Record_Page.flexipage-meta.xml" in the Salesforce DX project directory which shows in VS Code.

```
sfdx force:source:status
```

![14](tutorial/14.png)
```
sfdx force:source:pull
sfdx force:source:status
```

![14a](tutorial/14a.png)![14b](tutorial/14b.png)
### Step 15:
Use Git version control to safely commit the changes to source code and maintain this version permanently.

1. Check the status of the git repository. We see files in red which are not yet staged to be committed.

```
git status
```

![15](tutorial/15.png)
2. Add a new file in the project in VS Code names ".gitignore" with below content to avoid adding unnecessary files/folders to git.

```
.sfdx
.vscode
.forceignore
```

![15a](tutorial/15a.png)
3. Add all the necessary source code files to git staging using "git add ." and check status again to ensure all necessary files are green (tracked for committing changes).

```
git add .
git status
```

![15b](tutorial/15b.png)
4. Commit changes with a mandatory commit message using "git commit". Check status to ensure all necessary changes are committed. A version of your source code is now safely and permanently maintained by Git version control.

```
git commit -m "first commit"
git status
```

![15c](tutorial/15c.png)
### Step 16:

Create a Github repository with your github account to add our Salesforce DX project and git commit for sharing and collaboration. You can give it the same name as the Salesforce DX project "LCwithDX". Copy the URL of the new github repository for the next step.
![16](tutorial/16.png)![16a](tutorial/16a.png)![16b](tutorial/16b.png)
### Step 17:

Add the URL as a "remote" location to git on the local machine so that the local git commit can directly be pushed to Github with a single git command. Let's follow standard naming conventions and call the remote "origin". Remember to replace "<yourGithubRepoURL>" in the below comamnd with your actual Github repo URL that you copied in the previous step. We can now push the git commit to origin using the command "git push origin master". At this point, your github repository is now populated with your latest committed Salesforce DX project! You can now share this with anyone to collaborate while maintaining version control and source code management.

```
git remote add origin <yourGithubRepoURL>
git push origin master
```

![17](tutorial/17.png)![17a](tutorial/17a.png)