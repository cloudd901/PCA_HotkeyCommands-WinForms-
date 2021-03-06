# HotkeyCommands
Resource to add Hotkey shortcuts to your WinForms.
________________________________________________________________

- Reference HotkeyCommands.dll in the Forms.cs.
- Set the Extension method next to your Form class.
- Create new instance of HotkeyCommands
- Set Action KeyActionCall (Returns Form and Key Pressed)
________________________________________________________________

Setup Example:

	using PCAFFINITY;
	using System;
	using System.Windows.Forms;

	namespace YourHotkeyProgram
	{
	    public partial class Form1 : HotkeysExtensionForm
	    {
		public Form1()
		{
		    InitializeComponent();

		    //Configure Hotkeys
		    HotkeyCommands hotkeyComm = new HotkeyCommands(this, new string[] { "Escape", "{CTRL}{Shift}A" });
		    hotkeyComm.StartHotkeys();
		    hotkeyComm.KeyActionCall += onKeyAction;
		}
	
		public static void onKeyAction(Form form, short id, string key)
        	{
            	//do something with "key" or 'id'
        	}
	}

Adding and Removing Hotkeys Example:

	----------
	HotkeyCommands hotkeyComm = new HotkeyCommands(this);
	hotkeyComm.HotkeyRegisterList(new string[] { "F1", "F2", "F3" });
	hotkeyComm.KeyActionCall += onKeyAction;
	hotkeyComm.StartHotkeys();
	
	hotkeyComm.HotkeyUnregisterAll(); //Removes and unregisters all keys in the Dictionary
	hotkeyComm.HotkeyAddKey("F4").HotkeyAddKey("{Shift}F4"); //Stacking is allowed
	hotkeyComm.HotkeyRemoveKey("F4");
	hotkeyComm.RestartHotkeys();
	
	//The example above leaves only Hotkey "{Shift}F4" active.
	
	----------
	HotkeyCommands hotkeyComm = new HotkeyCommands(this);
	hotkeyComm.HotkeyAddKeyList(new string[] { "F1", "F2", "F3" });
	hotkeyComm.KeyActionCall += onKeyAction;
	hotkeyComm.StartHotkeys();
	
	hotkeyComm.HotkeyUnregisterAll(false); //unregisters keys but leaves them in the dictionary.
	hotkeyComm.HotkeyAddKey("F4").HotkeyAddKey("{Shift}F4"); //Stacking is allowed
	hotkeyComm.HotkeyRemoveKey("F4");
	hotkeyComm.RestartHotkeys(); //All keys in the dictionary get reregistered on Start or Restart.
	
	//The example above leaves Hotkeys "F1", "F2", "F3", and "{Shift}F4" active.


Other Options:
	
	//True sets Hotkeys everywhere, False only catches Hotkeys if Form is active.
	hotkeyComm.SetHotkeysGlobally = true;
	
	//False will throw exceptions when something fails to work.
	hotkeyComm.SetSuppressExceptions = false;
	
	//Returns the dictionary used to store <string> Hotkeys and their <short> ID's.
	//Upon _StartHotkeys, all dicitonary keys will be registered/activated.
	hotkeyComm.HotkeyDictionary;
	
	//Unregister/deactivate all Hotkeys with the option of keeping the dictionary.
	hotkeyComm.HotkeyUnregisterAll();
	
	HotkeyAddKey and HotkeyAddKeyList gives the options to add custom ID's to the Dictionary.
	
	KeyRegisteredCall is an event fired when a key is registered. Returns true or false along with id and key.
	KeyUnRegisteredCall is an event fired when a key is unregistered. Returns id and key.
