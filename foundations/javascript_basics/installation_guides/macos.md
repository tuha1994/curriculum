### Hướng dẫn: Cài đặt nvm trên macOS

Trên macOS 10.15 trở lên, shell mặc định hiện là zsh. Trong quá trình cài đặt, nvm sẽ tìm file `.zshrc` trong thư mục home của người dùng. Theo mặc định, file này không tồn tại, vì vậy chúng ta cần tạo nó bằng cách chạy lệnh sau trong terminal:

```bash
touch ~/.zshrc
```

Bây giờ chúng ta có thể chạy lệnh sau để cài đặt nvm:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.4/install.sh | bash
```

Hãy khởi động lại terminal, hoặc sao chép và dán nội dung sau vào terminal rồi nhấn <kbd>Enter</kbd>:

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" # This loads nvm bash_completion
```

Kiểm tra cài đặt nvm bằng cách chạy lệnh sau; lệnh này sẽ hiển thị số phiên bản đã cài đặt:

```bash
nvm --version
```

Để biết thêm thông tin, hãy đọc về [cách cài đặt và cập nhật từ GitHub của NVM](https://github.com/nvm-sh/nvm#installation-and-update).

[Quay lại bài học Cài đặt Node.js](https://www.theodinproject.com/lessons/foundations-installing-node-js#assignment)