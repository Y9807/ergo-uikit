# Ergo UIKit: Smart Money at Your Fingertips

## Introduction

[Ergo platform](https://ergoplatform.org/en/) is open which is one of its core values. In
particular it is open for developers and community driven. The Ergo network should be
usable from any device and any platform to ensure wide reach. The requirement for hardware
must be as low as possible. The basic development framework needs to be flexible and
robust to allow a single codebase to run on mobile, web and desktop. In addition, security
of the user application is especially important to guarantee safety of user secrets
(private keys, mnemonic phrases etc).

Ergo smart contracts are based on the extended UTXO model, which allows complex
multi-stage dApp scenarios like DEX, ErgoMix, Crowd Funding, Notary, ICO, LETS, Ring
Signatures etc. All this scenarios require full
[ErgoTree](https://ergoplatform.org/docs/ErgoTree.pdf) interpreter to be available as part
of the Ergo UI framework to sign transaction locally, without sending any secrets out of
the user device. This is because all for each input the Ergo application need to execute
the guarding script to sign the input. This is requirement of Ergo's extended UTXO model.

Thus, any GUI application need a basic support to perform basic blockchain related
operations:
- user authentication
- secure wallet management 
- funds accounting, sending and receiving
- transaction signing etc.

This UIKit will help to extend your existing application with Ergo's Smart Money
capabilities, or you can use it to create a new Ergo app from scratch.

## Goals

- Simplify development of true cross-platform GUI Ergo applications which can run in the
following environments/platforms:
    - Android 5.x of newer (ARM)
    - iOS 8 or newer (iPhone 4S or newer)
    - Web (Firefox, Chrome, Safari, Egde)
    - Desktop (macOS, Windows, Linux)

- Extend existing Android, iOS, Web and Desktop application with Smart Money capabilities powered by Ergoplatform.

NOTE, development of any specific application is not a goal of UIKit, only as an example.
It doesn't mean that the example cannot be useful by its own.

## Use Cases

Ergo UIKit is a collection of non-opinionated GUI Widgets for
[Flutter](https://flutter.dev/) which support different Ergo application scenarios:

- Standalone, pure Flutter, Ergo GUI application can be implemented using UIKit Widgets.
The application can utilize UIKit Widgets to interact with Ergo blockchain, visualise
data, compose transactions, sign them and send to the blockchain (via REST API
endpoint). The UIKit Widgets have minimal assumptions about application architecture thus
not limiting applicability. 

- Existing native Android, iOS, Web application can be extended with Ergo blockchain
interactions. The UIKit Widgets can be seamlessly integrated into application navigation
and user facing interactions. The data model used by the UIKit Widgets is abstracted from the
concrete storage and can be adapted to any existing application design.

## Components

The library implements the following components (Widgets):

#### Login
- [ ] `Login` screen
  - [ ] PIN code authentication
  - [ ] password entry authentication
  - [ ] Fingerprint and FaceID authentication
  
#### Wallet Management screens
  - [ ] `Wallets` list view
  - [ ] `New Wallet` creation form
    - [ ] Mnemonic Phrase entry, master key and Secret Storage generation according to [BIP-32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki)
    - [ ] double password entry
  - [ ] `Edit Wallet` form
    - [ ] view and edit wallet properties
    - [ ] `Keys` list view
  - [ ] `Backup Mnemonic` form
  
#### Settings and Preferences
- [ ] `Settings` form 
  - [ ] `Ergo Node` Endpoint
  - [ ] `Known Peers` list view
 
#### Standard Wallet Operations
- [ ] `Wallet Details` screen (show a list of wallet addresses with with additional info)
- [ ] `Address Details` screen (show details of the address, ERG balance, list of tokens, etc)
- [ ] `Tokens Portfolio` screen (show tokens list of either a wallet or an address)
- [ ] `Send` form (to send ERGs and tokens to an address)
- [ ] `Transactions` list (shows transactions for an Address, Wallet)
- [ ] `TransactionDetails` form (shows details of  )

#### Utilities
- [ ] `QRCode Button` can be attached to a text field and open a `QRCode View`
- [ ] `QRCode Scan Button` can be attached to an input text field and open a `QRCode Scanner`
- [ ] `QRCode View` shows a QR-code for a string
- [ ] `Explorer Link` widget (when tapped opens Ergo Explorer page for a given Address,
Box, Transaction, Block)

## Why Flutter?

The motivation behind choosing Flutter can be found [here](docs/ui-platform.md).

