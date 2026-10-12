# EXERCISE 3
## Gradle Dependency Caching trong GitHub Actions

### 1. Mục tiêu

Tối ưu tốc độ chạy CI của ứng dụng Java Spring Boot bằng cơ chế Gradle Dependency Caching trong GitHub Actions.

### 2. Cấu hình

Bổ sung `cache: 'gradle'` trong bước `actions/setup-java@v5`:

```yaml
- name: Set up Java JDK 17
  uses: actions/setup-java@v5
  with:
    java-version: '17'
    distribution: 'temurin'
    cache: 'gradle'
```

### 3. Kết quả thực nghiệm

| Lần chạy | Cấu hình | Thời gian | Kết quả |
|---|---|---|---|
| #1 | Không sử dụng cache | 41 giây | Success |
| #2 | Kích hoạt Gradle cache | 56 giây | Success |
| #3 | Kiểm tra Gradle cache | 29 giây | Success |

### 4. Phân tích

Lần chạy đầu tiên không sử dụng cache, hoàn thành trong 41 giây.

Lần chạy thứ hai đã bật Gradle Dependency Caching nhưng mất 56 giây. Nguyên nhân có thể liên quan đến việc tải dependency và tạo cache lần đầu.

Lần chạy thứ ba hoàn thành trong 29 giây, nhanh hơn lần chạy đầu tiên 12 giây, tương đương khoảng 29,27%.

Kết quả cho thấy thời gian chạy CI giảm trong lần thực nghiệm thứ ba. Tuy nhiên, cần kiểm tra log `setup-java` để xác nhận cache được khôi phục thành công và phân biệt tác động của cache với sự dao động hiệu năng runner.

### 5. Kết luận

Cả ba lần chạy GitHub Actions đều thành công.

Thời gian thực thi giảm từ 41 giây xuống 29 giây khi so sánh lần đầu với lần thứ ba. Việc sử dụng Gradle Dependency Caching có thể giúp giảm thời gian tải lại thư viện trong những lần CI tiếp theo.