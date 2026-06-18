**rclone + Garage S3 — Pathless Payload Storage**

---

**Configure rclone** (once, in `~/.config/rclone/rclone.conf`):

```ini
[garage]
type = s3
provider = Other
access_key_id = <key>
secret_access_key = <secret>
region = garage
endpoint = <endpoint> # ex tailscaleIP:3900
```

---

**Upload** (directory or single file):

```sh
go run build/main.go ./slides > slides.bin
rclone copyto slides.bin garage:zero/slides
rm slides.bin
```

```sh
go run build/main.go ./content/notes.md > notes.bin
rclone copyto notes.bin garage:zero/notes
rm notes.bin
```

---

**Verify:**

```sh
curl -s https://zero.s3.timefactory.io/slides | wc -c
```

**Delete:**

```sh
rclone delete garage:zero/slides
```

**List bucket:**

```sh
rclone ls garage:zero
```

---

**In main.go** — use the S3 URL as the path:

```go
p.Slides("https://zero.s3.timefactory.io/slides")
p.Text("https://zero.s3.timefactory.io/notes")
```

`pathless.Load` fetches the raw wire bytes and compresses them at startup.