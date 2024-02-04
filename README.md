## 🌟 Community-Powered Repository 🌟

This repository is crafted with ❤️ by our talented community members. It's a space for everyone to use, contribute to, and share. While it aligns with the spirit of our community, please note that this repo is not directly endorsed or supported by Investec. Always exercise caution and discretion when using or contributing to community-driven projects.


# investecOpenAPI


## installation

make sure you are in your root of your project folder
and you have a go.mod file there if not you just need to
create it with

```bash
go mod init <project name>
```

```bash
go get -v github.com/t4ke0/investecOpenAPI
```


## Usage

before you start you might need to have the `clientID` and `secret` from the API provider.
then you are good to proceed.

```golang
package main

import (
    ***

	client "github.com/t4ke0/investecOpenAPI"

    ***
)


func main() {
    // need to have secret and clientID then fill in these vars.
    secret := ""
    clientID := ""
    apiKey := ""

    clt := client.NewBankingClient(secret, clientID, apiKey)

    // OAuth to the API
    if err := clt.GetAccessToken(); err != nil {
        log.Fatal(err)
    }

    // Get Accounts.
    accounts, err := clt.GetAccounts()
    if err != nil {
        log.Fatal(err)
    }

    // see the specification of the account type in `api/api.go`
    fmt.Println("Accounts", accounts) 

    // Get Account Balance
    var accountID string = accounts.Data.AccountArr[0].AccountID
    balance, err := clt.GetAccountBalance(accountID)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Println("balance", balance)

    // Get account transactions
    // you can use fromDate and toData params to filter the transactions.
    // see investecOpenAPI.go file the dates need to have ISO 8601 format.
    transactions, err := clt.GetAccountTransactions(accountID, "", "")
    if err != nil {
        log.Fatal(err)
    }
    
    fmt.Println("transactions", transactions)
}
```

to Run an example please build `example/cmd/main.go`

```bash
go build example/cmd/main.go
```
