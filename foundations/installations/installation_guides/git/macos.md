### Hướng dẫn: Thiết lập Git trên macOS

#### Bước 1: Cài đặt Homebrew

Trước tiên, bạn cần cài đặt Homebrew. Để cài đặt, trước hết hãy đảm bảo bạn đáp ứng [các yêu cầu của Homebrew trên macOS](https://docs.brew.sh/Installation#macos-requirements). Sau khi đáp ứng các yêu cầu, hãy sao chép và dán nội dung sau vào terminal:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

<div class="lesson-note lesson-note--warning" markdown=1>

#### Có thể xuất hiện "Next steps" sau khi cài đặt Homebrew

Nếu xem phần xuất ra trong terminal sau khi cài đặt Homebrew, bạn sẽ thấy "Installation Successful!". Ở phía dưới trong terminal, bạn cũng có thể thấy một phần có tiêu đề "Next steps".

Đọc nội dung terminal có thể khiến bạn hơi e ngại, nhưng đây là cơ hội tốt để vượt qua cảm giác đó. Nếu thấy phần này, hãy làm theo các bước tiếp theo được nêu trong terminal (sao chép và dán các lệnh được cung cấp) để thêm Homebrew vào `PATH`, nhờ đó bạn có thể sử dụng tiền tố lệnh `brew`.

</div>

#### Bước 2: Cập nhật Git

macOS đã có sẵn một phiên bản Git, nhưng bạn nên cập nhật lên phiên bản mới nhất. Trong terminal, hãy nhập

```bash
brew install git
```

Lệnh này sẽ cài đặt phiên bản Git mới nhất. Đơn giản phải không?

#### Bước 3: Kiểm tra phiên bản

Nếu vừa cài đặt và/hoặc cập nhật Git ở bước trước, trước tiên hãy đóng cửa sổ terminal đó.

**Mở một cửa sổ terminal mới** rồi đảm bảo phiên bản Git của bạn **ít nhất** là 2.28 bằng cách chạy lệnh sau:

```bash
git --version
```

Nếu số phiên bản nhỏ hơn 2.28, hãy làm lại hướng dẫn. Nếu gặp lỗi `no formulae found in taps`:

1. Chạy `brew doctor`.
1. Bạn sẽ thấy nội dung xuất ra tương tự như bên dưới. LƯU Ý: Nội dung thực tế của `brew doctor` có thể thay đổi tùy theo phiên bản macOS bạn đang chạy và các vấn đề khác trong quá trình cài đặt của riêng bạn. Cuối cùng, bạn phải chạy từng đoạn lệnh mà Homebrew cung cấp sau khi chạy `brew doctor` để sửa chữa bản cài đặt Homebrew, bao gồm cả `brew cleanup` ở cuối.
![Nội dung mẫu của Brew Doctor](https://cdn.statically.io/gh/TheOdinProject/curriculum/284f0cdc998be7e4751e29e8458323ad5d320303/foundations/installations/setting_up_git/imgs/00.png)
1. Chạy `brew install git`, **mở một cửa sổ terminal mới**, rồi kiểm tra phiên bản Git; lúc này phiên bản này sẽ là phiên bản mới nhất.

[Quay lại bài học Thiết lập Git](https://www.theodinproject.com/lessons/foundations-setting-up-git#assignment)