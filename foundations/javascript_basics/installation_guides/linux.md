### Hướng dẫn: Cài đặt nvm trên Linux

#### Bước 0: Điều kiện tiên quyết

Trước hết, hãy chạy lệnh sau trong terminal để bảo đảm danh sách package của Ubuntu được cập nhật:

```bash
sudo apt update && sudo apt upgrade
```

Để cài đặt nvm đúng cách, bạn cần `curl`. Chạy lệnh dưới đây để cài đặt `curl`:

```bash
sudo apt install curl
```

#### Bước 1: Tải xuống và cài đặt nvm

Chạy lệnh này để cài đặt `nvm`:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.4/install.sh | bash
```

#### Bước 2: Khởi tạo nvm

Trong terminal thường sẽ có hướng dẫn cách khởi tạo `nvm`. Nếu không có (hoặc bạn không muốn chép lại từ terminal), hãy chạy các lệnh sau:

```bash
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

Bạn có thể xác minh `nvm` đã được cài đặt bằng cách chạy lệnh:

```bash
command -v nvm
```

Nếu lệnh này trả về `nvm: command not found`, hãy đóng terminal rồi mở lại.

[Quay lại bài học Cài đặt Node.js](https://www.theodinproject.com/lessons/foundations-installing-node-js#assignment)