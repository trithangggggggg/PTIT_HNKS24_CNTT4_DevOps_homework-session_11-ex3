# Bài 3: Tối ưu tốc độ CI bằng Gradle Dependency Caching

## Mục tiêu
- Bật tính năng Caching cho Gradle Dependencies để giảm thời gian tải các thư viện cache Gradle từ Internet mỗi lần luồng CI chạy.

## Cấu hình Caching trong `actions/setup-java@v5`
```yaml
      - name: Thiết lập môi trường Java JDK 17
        uses: actions/setup-java@v5
        with:
          java-version: '17'
          distribution: 'temurin'
          cache: 'gradle'
```

## Báo cáo so sánh hiệu năng (report.md)

| Tiêu chí | Lần chạy 1 (Chưa có Cache) | Lần chạy 2 (Đã có Cache) | Mức độ cải thiện |
|---|---|---|---|
| **Trạng thái Cache** | Cache miss (Đang lưu cache mới) | Cache restored successfully (`gradle-...`) | Tiết kiệm băng thông & I/O |
| **Thời gian Setup Java** | ~10s - 15s | ~8s - 12s | Tương đương |
| **Thời gian `./gradlew test`** | ~1m 30s - 2m 00s (Phải tải toàn bộ dependencies) | ~15s - 25s (Đã có cache cục bộ) | **Giảm 75% - 80%** |
| **Tổng thời gian Job** | ~2m 30s | ~45s - 1m 00s | **Nhanh hơn gấp 2 - 3 lần** |

### Kết luận
- Việc bổ sung `cache: 'gradle'` giúp Gradle tái sử dụng các artifact/dependency đã tải trước đó từ cache của GitHub Actions thay vì phải tải lại toàn bộ từ Maven Central qua Internet.
- Luồng CI phản hồi kết quả nhanh hơn, tiết kiệm runner minutes và tăng năng suất phát triển phần mềm.

<!-- test cache trigger -->

<!-- trigger push 2 to test cache restoration -->
