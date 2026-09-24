# Bank System Management (C++ OOP)

A console-based bank management system written in C++ with clean Object-Oriented design and class inheritance. Organized and fixed to build and run out of the box on VS Code with `g++`.

## Features

- **Clients:** list, add, delete, update, find
- **Transactions:** deposit, withdraw, total balances, transfer, transfer log
- **Users & permissions:** manage users, per-screen access rights (`pListClients`, `pAddNewClient`, `pDeleteClient`, `pUpdateClients`, `pFindClient`, `pTranactions`, `pManageUsers`, `pShowLogInRegister`)
- **Login system:** 3 login trials then lockout, login register log
- **File-based storage:** `Clients.txt`, `Users.txt` (passwords encrypted), `TransfersLog.txt`, `LoginRegister.txt`

## OOP Design (inheritance preserved)

```text
InterfaceCommunication
└── clsPerson
    ├── clsBankClient
    └── clsUser

clsScreen (base for all screens)
├── clsMainScreen
├── clsLoginScreen
├── clsClientListScreen
├── clsAddNewClientScreen / clsDeleteClientScreen / clsUpdateClientScreen / clsFindClientScreen
├── clsTransactionsScreen
│   ├── clsDepositScreen / clsWithdrawScreen / clsTotalBalancesScreen
│   └── clsTransferScreen / clsTransferLogScreen
├── clsManageUsersScreen
│   └── clsListUsersScreen / clsAddNewUserScreen / clsDeleteUserScreen / clsUpdateUserScreen / clsFindUserScreen
└── clsLoginRegisterScreen

Helpers: clsDate, clsString, clsUtil, clsInputValidate
Global:  Global.h (CurrentUser)
```

> Portability note: the original code used MSVC-only `__declspec(property)`. This version uses standard `Get/Set` methods with the same encapsulation, so it compiles on both MSVC and `g++` (MinGW). Minimal fixes were also applied: `Withdraw` return value, missing `clsDate.h` include, circular `MainScreen ↔ LoginScreen` include, and struct access levels.

## Project Structure

```text
BankSystem/
├── main.cpp                     # Login loop entry point
├── clsPerson.h / clsBankClient.h / clsUser.h
├── clsScreen.h / clsMainScreen.h / clsLoginScreen.h
├── Client screens (List/Add/Delete/Update/Find)
├── Transaction screens (Deposit/Withdraw/TotalBalances/Transfer/TransferLog/Transactions)
├── User screens (List/Add/Delete/Update/Find/Manage/LoginRegister)
├── clsDate.h / clsString.h / clsUtil.h / clsInputValidate.h
├── Global.h / InterfaceCommunication.h
├── Clients.txt / Users.txt      # demo data (passwords in Users.txt are encrypted, log in with plain ones below)
├── TransfersLog.txt / LoginRegister.txt   # runtime logs (start empty)
└── .vscode/tasks.json           # VS Code build task
```

## Requirements

- VS Code with C/C++ extension
- `g++` (tested with MinGW GCC 6.3, C++14)

## Build & Run (VS Code)

1. Open the `BankSystem` folder in VS Code.
2. Build: `Ctrl+Shift+B` (task: **build BankSystem**) — produces `BankSystem.exe`.
3. Run from the project folder (important, data files are loaded by relative path):

```bash
./BankSystem.exe
```

Or from terminal:

```bash
g++ -std=c++14 -Wall -o BankSystem.exe main.cpp
./BankSystem.exe
```

## Demo Login

| Username | Password | Permissions |
|----------|----------|-------------|
| User1    | 1234     | Full (-1)   |
| User2    | 1234     | Full (-1)   |
| User3    | 125      | Full (-1)   |
| User4    | 1234     | Full (-1)   |
| User5    | 5555     | Full (-1)   |
| User6    | 4323     | Full (-1)   |

After login use the main menu (1–9). Choosing **9 Logout** returns to the login screen; 3 failed logins lock you out and exit.

## Data Files

- `Clients.txt` — format: `First#//#Last#//#Email#//#Phone#//#AccNumber#//#PinCode#//#Balance`
- `Users.txt` — same + `UserName#//#EncryptedPassword#//#Permissions` (encrypted with key=2, decrypted on load)
- `TransfersLog.txt` — `DateTime#//#SrcAcc#//#DstAcc#//#Amount#//#SrcBalAfter#//#DstBalAfter#//#UserName`
- `LoginRegister.txt` — `DateTime#//#UserName#//#EncryptedPassword#//#Permissions`
