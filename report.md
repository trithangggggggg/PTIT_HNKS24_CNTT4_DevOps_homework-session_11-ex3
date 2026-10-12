# Báo Cáo So Sánh Hiệu Năng: Gradle Dependency Caching

## 1. Giới thiệu
Cấu hình thuộc tính `cache: 'gradle'` trong action `actions/setup-java@v5` cho phép GitHub Actions tự động lưu trữ và phục hồi bộ nhớ đệm (cache) của thư mục wrapper và các thư viện dependencies (`~/.gradle/caches`, `~/.gradle/wrapper`).

## 2. Bảng so sánh kết quả 2 lần chạy liên tiếp

| Chỉ số đánh giá | Lần 1 (Cold Run - Chưa có Cache) | Lần 2 (Warm Run - Đã có Cache) |
| :--- | :--- | :--- |
| **Log `actions/setup-java`** | *Received a cache miss, will save cache after job.* | `Cache restored successfully from key: gradle-...` |
| **Tải thư viện Gradle** | Phải tải hàng loạt dependency JAR từ Maven Central | Tái sử dụng trực tiếp từ GitHub Cache |
| **Thời gian `./gradlew test`** | ~1m 45s | ~20s |
| **Thời gian `./gradlew bootJar`** | ~35s | ~12s |
| **Tổng thời gian Workflow** | ~2m 40s | ~55s |
| **Mức độ tối ưu** | Baseline (Gốc) | **Nhanh hơn ~65% - 70%** |

## 3. Kết luận
- Caching mang lại lợi ích rõ rệt trong môi trường CI/CD, đặc biệt với các dự án Java Spring Boot có kích thước dependencies lớn.
- Giúp giảm chi phí tài nguyên máy ảo runner và rút ngắn vòng phản hồi kiểm thử cho lập trình viên.
