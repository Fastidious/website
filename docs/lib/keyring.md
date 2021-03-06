# Keyring

The following example describes how to:

- Initialize and setup/unlock a Keyring
- Save a secret to the keyring
- Get a secret from the keyring
- List keyring items

```go
// Initialize Keyring.
// You can use keyring.System, keyring.SystemOrFS, keyring.FS, keyring.Mem or git.NewRepository.
kr, err := keyring.New(keyring.SystemOrFS("AppName"))
if err != nil {
    log.Fatal(err)
}

// Setup the keyring.
if _, err := kr.UnlockWithPassword("mypassword", true); err != nil {
    log.Fatal(err)
}

// Create item.
// Item IDs are NOT encrypted.
item := keyring.NewItem("id1", []byte("mysecret"), "", time.Now())
if err := kr.Create(item); err != nil {
    log.Fatal(err)
}

// Get item.
out, err := kr.Get("id1")
if err != nil {
    log.Fatal(err)
}
fmt.Printf("secret: %s\n", string(out.Data))

// List items.
items, err := kr.List(nil)
if err != nil {
    log.Fatal(err)
}
for _, item := range items {
    fmt.Printf("%s: %v\n", item.ID, string(item.Data))
}
```
