# Bài 4: Mô phỏng quy trình Hotfix & Gitflow thực tế

## 1. Mục tiêu

Thực hành quy trình Hotfix theo mô hình Gitflow.

Mục tiêu là tạo nhánh hotfix trực tiếp từ `main`, sửa lỗi khẩn cấp, đưa bản sửa lỗi vào `main`, tạo tag phiên bản mới và đồng bộ bản sửa lỗi trở lại `develop`.

## 2. Các nhánh sử dụng

Repository sử dụng các nhánh:

```text
main
develop
hotfix/v1.0.1
```

Trong đó:

* `main`: chứa phiên bản production ổn định.
* `develop`: chứa các chức năng đang phát triển cho phiên bản tiếp theo.
* `hotfix/v1.0.1`: dùng để sửa lỗi nghiêm trọng trên phiên bản production.

## 3. Phiên bản ban đầu

Tạo commit ban đầu:

```text
release: version 1.0.0
```

Sau đó tạo tag:

```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
```

Nhánh `main` lúc này đại diện cho phiên bản stable `v1.0.0`.

## 4. Nhánh develop

Tạo nhánh `develop` từ `main`:

```bash
git checkout -b develop
```

Thêm một chức năng mới để mô phỏng quá trình phát triển:

```text
feat: add new book management
```

Nhánh `develop` lúc này chứa code mới nhưng chưa được deploy lên production.

## 5. Tạo nhánh Hotfix

Khi phát hiện lỗi nghiêm trọng liên quan đến việc lộ dữ liệu người dùng, chuyển về `main`:

```bash
git checkout main
```

Sau đó tạo nhánh hotfix trực tiếp từ `main`:

```bash
git checkout -b hotfix/v1.0.1
```

Điều này đảm bảo bản sửa lỗi được xây dựng dựa trên phiên bản production hiện tại.

## 6. Sửa lỗi

Thực hiện sửa lỗi bảo mật và commit:

```text
fix: prevent user data exposure
```

Bản sửa lỗi đảm bảo hệ thống không còn cho phép dữ liệu người dùng bị lộ.

## 7. Merge Hotfix vào main

Chuyển về `main`:

```bash
git checkout main
```

Merge nhánh hotfix:

```bash
git merge hotfix/v1.0.1
```

Sau đó tạo tag phiên bản mới:

```bash
git tag -a v1.0.1 -m "Release Hotfix 1.0.1"
```

Tag `v1.0.1` đánh dấu phiên bản production đã được sửa lỗi.

## 8. Đồng bộ Hotfix vào develop

Sau khi phát hành bản sửa lỗi, cần đưa bản sửa lỗi trở lại nhánh phát triển.

Chuyển sang `develop`:

```bash
git checkout develop
```

Merge hotfix:

```bash
git merge hotfix/v1.0.1
```

Như vậy `develop` cũng chứa bản sửa lỗi bảo mật.

Điều này giúp tránh trường hợp lỗi đã được sửa trên production nhưng xuất hiện trở lại trong phiên bản tiếp theo.

## 9. Xóa nhánh Hotfix

Sau khi đã merge vào cả `main` và `develop`, xóa nhánh hotfix:

```bash
git branch -d hotfix/v1.0.1
```

## 10. Sơ đồ Gitflow

```text
                         develop
                            |
                            | feat: add new book management
                            |
main ----------------------●-------------------------
  |                         \
  | v1.0.0                   \
  |                          hotfix/v1.0.1
  |                               |
  |                               | fix: prevent user data exposure
  |                               |
  |                               ●
  |                              / \
  |                             /   \
  |                            /     \
  ▼                           ▼       ▼
main                       v1.0.1   develop
                            tag       |
                                      |
                                      | hotfix được đồng bộ
                                      ▼
```

## 11. Kiểm tra

Kiểm tra các nhánh:

```bash
git branch -a
```

Kiểm tra tag:

```bash
git tag
```

Kết quả mong đợi:

```text
v1.0.0
v1.0.1
```

Kiểm tra lịch sử:

```bash
git log --graph --oneline --all
```

Lịch sử phải thể hiện được:

* `main` chứa bản sửa lỗi hotfix.
* Tag `v1.0.1` trỏ đến phiên bản đã sửa lỗi.
* `develop` cũng chứa commit `fix: prevent user data exposure`.
* Nhánh `hotfix/v1.0.1` đã được xóa sau khi hoàn thành.

## 12. Kết quả

Đã hoàn thành quy trình Hotfix theo mô hình Gitflow.

Bản sửa lỗi được tạo trực tiếp từ `main`, sau đó được merge vào `main` và tạo tag `v1.0.1`.

Cuối cùng, bản sửa lỗi được merge ngược vào `develop` để đảm bảo lỗi bảo mật không xuất hiện lại trong các phiên bản tương lai.
