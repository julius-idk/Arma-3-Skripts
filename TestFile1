if (missionNamespace getVariable ["QuadAirdropScriptRunning", false]) exitWith {
	systemChat "[Quad Airdrop] Simple Quad Airdrop Script is already running";
};

QuadAirdropScriptRunning = true;
publicVariable "QuadAirdropScriptRunning";



{
	player createDiarySubject ["SQAS_Diary", "Quadbike Airdrop"];

	private _SQAS_diaryRecord = player createDiaryRecord ["SQAS_Diary", 
	[
		"Info",
		"<br/>" +
		"Simple Quadbike Airdrop Script<br/><br/><br/>" +
		"- Keybind: CTRL + O (letter)<br/><br/>" +
		"After you pressed the key, scroll with your mousewheel and click<br/>" +
		"'Request Quadbike Aidrop'.<br/>" +
		"Then click the 'Yes Confirm' option and wait for the drop.<br/><br/>" +
		"You can request a quad every 5 minutes.<br/><br/><br/>" +
		"- script by julius"
	]];
} remoteExec ["call", 0, true];


["[Quad Airdrop] Simple Quadbike Airdrop enabled. Keybind: CTRL + O (letter)"] remoteExec ["systemChat", 0, true];




AirdropScript_CreateTempMarker = {
	
	_markerName = format ["QuadAirdrop_%1_%2", netId _quad, round(time)];	
	_playerSide = side _caller;



	[[_playerSide, _markerName, _pos, _caller], {
		_playerSide = _this select 0;
		_markerName = _this select 1;
		_pos = _this select 2;
		_caller = _this select 3;
		
		if (side player == _playerSide) then {		
			
			private _marker = createMarkerLocal [_markerName, _pos];
			_marker setMarkerTypeLocal "mil_end";
			_marker setMarkerTextLocal format ["Quad Airdrop (%1)", name _caller];
			_marker setMarkerSizeLocal [0.5, 0.5];
			_marker setMarkerColorLocal "ColorWhite";
			_marker setMarkerAlphaLocal 0.8;
		};
	}] remoteExec ["spawn", 0];

	
	_quad setVariable ["AirdropMarkerName", _markerName, true];

	[_quad, ["GetIn", {
		params ["_vehicle", "_role", "_unit", "_turret"];
		private _markerName = _vehicle getVariable "AirdropMarkerName";	
		if (!isNil "_markerName") then {
			deleteMarker _markerName;
			_vehicle setVariable ["AirdropMarkerName", nil, true];
		};					
	}]] remoteExec ["addEventHandler", 0, true];
	
	
	[_quad, ["Killed", {
		params ["_unit", "_killer", "_instigator", "_useEffects"];
		private _markerName = _unit getVariable "AirdropMarkerName";	
		if (!isNil "_markerName") then {
			deleteMarker _markerName;
			_unit setVariable ["AirdropMarkerName", nil, true];
		};			
	}]] remoteExec ["addEventHandler", 0, true];	

	[_quad, ["Deleted", {
		params ["_entity"];
		private _markerName = _entity getVariable "AirdropMarkerName";	
		if (!isNil "_markerName") then {
			deleteMarker _markerName;
			_entity setVariable ["AirdropMarkerName", nil, true];
		};		
	}]] remoteExec ["addEventHandler", 0, true];
	
	[_quad] spawn {
		_quad = _this select 0;
		sleep 300;
		private _markerName = _quad getVariable "AirdropMarkerName";	
		if (!isNil "_markerName") then {
			deleteMarker _markerName;
			_quad setVariable ["AirdropMarkerName", nil, true];
		};		
	};
	
};
publicVariable "AirdropScript_CreateTempMarker";



AirdropScript_Action = {
	player setVariable ["YesNoOptionsVisible", false, true];
	
	_MainAction_ID = player addAction ["<t size='1.4'>Request Quadbike Airdrop", { 
		params ["_target", "_caller", "_actionId", "_arguments"]; 
		_target setVariable ["YesNoOptionsVisible", true, true];	
	}, nil, 10, true, false, "", "(_this distance _target) < 0.01 && (vehicle _target == _target)"];
	
	_YesAction_ID = player addAction ["<t size='1.2'>Yes, confirm", {	
		params ["_target", "_caller", "_actionId", "_arguments"];
		if (_caller getVariable ["AirdropOnCooldown", false]) exitWith {
			[["<t color='#FF0000' size='1.7'>Quadbike Airdrop on cooldown (5min after last drop)", "PLAIN DOWN", 0.5, true, true]] remoteExec ["titleText", _caller];
		};
		private _pos = getPos _caller;
		private _dropPos = [_pos select 0, _pos select 1, (_pos select 2) + 100];
		private _quad = createVehicle ["B_Quadbike_01_F", _dropPos, [], 0, "NONE"];
						
					
		[_quad, _caller, _pos] call AirdropScript_CreateTempMarker;					
						
		[_quad, "<t color='#FF0000'>Delete Quad", 
			"\a3\ui_f\data\IGUI\Cfg\holdactions\holdAction_hack_ca.paa", 
			"\a3\ui_f\data\IGUI\Cfg\holdactions\holdAction_hack_ca.paa",
			"(_this distance _target) < 3 && (alive _target) && ((count crew vehicle _target) == 0) && (vehicle _this == _this) && (unitIsUAV _this == false)",	
			"(_this distance _target) < 3 && (alive _target) && ((count crew vehicle _target) == 0) && (vehicle _caller == _caller)",
			{}, 																
			{
				params ["_target", "_caller"];
				[["<t color='#FF0000' size='1.8'>Deleting...", "PLAIN DOWN", 0.01, true, true]] remoteExec ["titleText", _caller]; 							
			},									
			{ 
				params ["_target", "_caller"];			
				[_target] remoteExec ["deleteVehicle", 0];
				
			}, {}, [], 3, -1, true, false, false
		] remoteExec ["BIS_fnc_holdActionAdd", 0, true];			
						
		private _parachute = createVehicle ["B_Parachute_02_F", _dropPos, [], 0, "NONE"];
		_quad attachTo [_parachute, [0,0,0]];
		[_quad] spawn {
			_quad = _this select 0;
			waitUntil {(getPos _quad select 2) < 3};
			[_quad] remoteExec ["detach"];
		};
		[_parachute] spawn {
			_parachute = _this select 0;
			while {alive _parachute} do {
				_parachute setVelocity [0,0,-3];
				sleep 0.1;
			};
		};			
		[_caller] spawn {
			params ["_caller"];
			sleep 300;
			_caller setVariable ["AirdropOnCooldown", false, true];
		};
		[_quad, ["Killed", { 
			params ["_unit", "_killer"];  
			[_unit] spawn { 
				params ["_unit"]; 
				sleep 180;  
				[_unit] remoteExec ["deleteVehicle", 0]; 
			};         
		}]] remoteExec ["addEventHandler", 0, true]; 		
		[["<t color='#00FF0C' size='1.7'>Quadbike Airdrop confirmed. Standby", "PLAIN DOWN", 0.5, true, true]] remoteExec ["titleText", _caller];
		
		_caller setVariable ["AirdropOnCooldown", true, true];
		[_caller] call AirdropScript_RemoveAction;			
		
	}, nil, 9.9, true, false, "", "(_this distance _target) < 0.01 && (_target getVariable ['YesNoOptionsVisible', false]) && (vehicle _target == _target)"];	
	
	_No_Action_ID = player addAction ["<t size='1.2'>No, cancel", {	
		params ["_target", "_caller", "_actionId", "_arguments"];	
		[_caller] call AirdropScript_RemoveAction;	
	}, nil, 9.9, true, false, "", "(_this distance _target) < 0.01 && (_target getVariable ['YesNoOptionsVisible', false]) && (vehicle _target == _target)"];	

	player setVariable ["MainAction_ID", _MainAction_ID, true];
	player setVariable ["YesAction_ID", _YesAction_ID, true];
	player setVariable ["No_Action_ID", _No_Action_ID, true];
	
};
publicVariable "AirdropScript_Action";


AirdropScript_RemoveAction = {
	_MainAction_ID = player getVariable ["MainAction_ID", objNull];
	_YesAction_ID = player getVariable ["YesAction_ID", objNull];
	_No_Action_ID = player getVariable ["No_Action_ID", objNull];
	
	player removeAction _MainAction_ID;
	player removeAction _YesAction_ID;
	player removeAction _No_Action_ID;
	
	player setVariable ["YesNoOptionsVisible", false, true];
	player setVariable ["CanSeeAirDropOption", false, true];	
};
publicVariable "AirdropScript_RemoveAction";





AirdropScript_AddKeybindLocal = {
	waitUntil { 
		uisleep 0.1;
		!isNull (findDisplay 46) && alive player;
	};
	sleep 0.1;
	  
	if(!isNil "AirdropScript_DEH_KeyDown_Airdrop") then {
		(findDisplay 46) displayRemoveEventHandler ["KeyDown", AirdropScript_DEH_KeyDown_Airdrop];
	};

	AirdropScript_DEH_KeyDown_Airdrop = (findDisplay 46) displayAddEventHandler ["KeyDown", {
		params ["_display","_key","_shift","_ctrl","_alt"];
		
		if (_ctrl && {_key == 24}) then {
			if (isNull player) exitWith {};
			if (!alive player) exitWith {};
			if (vehicle player != player) exitWith {
				titleText ["<t color='#FF0000' size='1.7'>You can't be in a vehicle when requesting an airdrop", "PLAIN DOWN", 0.5, true, true];
			};

			_visible = !(player getVariable ["CanSeeAirDropOption", false]);
			player setVariable ["CanSeeAirDropOption", _visible, true];
			
			if (player getVariable ["CanSeeAirDropOption", false]) then {						
				[player] call AirdropScript_Action;						
				titleText ["<t color='#00FF0C' size='1.7'>Keybind pressed. Use scroll wheel to confirm request", "PLAIN DOWN", 0.5, true, true];
			} else {
				[player] call AirdropScript_RemoveAction;	
			};			
		};
	
	}];
	
};
publicVariable "AirdropScript_AddKeybindLocal";


comment "Just using   [AirdropScript_AddKeybindLocal] remoteExec ['spawn', 0, '...']   caused problems, so I went with his";
AirdropScript_Call_KeybindLocal = { 
	[] spawn AirdropScript_AddKeybindLocal; 
}; 
publicVariable "AirdropScript_Call_KeybindLocal";

[AirdropScript_Call_KeybindLocal] remoteExec ["call", 0, "AirdropScript_Call_KeybindLocal_JIPid"];



AirdropScript_RespawnEH = {
	player addEventHandler ["Respawn", {
		params ["_unit", "_corpse"];
		_unit setVariable ["CanSeeQuadAirDropOptions", false, true];
		[AirdropScript_Call_KeybindLocal] remoteExec ["call", _unit];
		if (_corpse getVariable ["AirdropOnCooldown", false]) then {
			[_unit, _corpse] spawn {
				params ["_unit", "_corpse"];
				waitUntil { !(_corpse getVariable ["AirdropOnCooldown", false]) };
				_unit setVariable ["AirdropOnCooldown", false, true];			
			};	
		};
	}];
};
[AirdropScript_RespawnEH] remoteExec ["call", 0, "AirdropScript_RespawnEH_JIPid"];
publicVariable "AirdropScript_RespawnEH";



deleteVehicle this;
