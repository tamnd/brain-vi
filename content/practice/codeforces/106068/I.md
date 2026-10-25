---
title: "CF 106068I - Vấn đề của thẩm phán"
description: "Mười giám khảo, mỗi người chọn một số từ 1 đến 10, đại diện cho vấn đề mà họ muốn thêm vào cuộc thi. Sau khi thu thập tất cả các phiếu bầu, bài toán được chọn là bài toán có số phiếu bầu cao nhất."
date: "2026-06-20T13:13:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106068
codeforces_index: "I"
codeforces_contest_name: "2025 Aleppo and Idlib Private Universities Collegiate Programming Contest (APUCPC 2025)"
rating: 0
weight: 106068
solve_time_s: 45
verified: true
draft: false
---

[CF 106068I - Vấn đề của thẩm phán](https://codeforces.com/problemset/problem/106068/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mười giám khảo, mỗi người chọn một số từ 1 đến 10, đại diện cho vấn đề mà họ muốn thêm vào cuộc thi. Sau khi thu thập tất cả các phiếu bầu, bài toán được chọn là bài toán có số phiếu bầu cao nhất. Nếu một số vấn đề có cùng số phiếu bầu cao nhất, quy tắc quyết định sẽ ưu tiên số vấn đề lớn nhất trong số đó. 

Vì vậy, đầu vào chỉ đơn giản là mười số nguyên, mỗi số đóng vai trò giống như một cuộc bỏ phiếu cho một trong mười ứng cử viên được gắn nhãn từ 1 đến 10. Đầu ra là một ứng cử viên duy nhất chiến thắng theo quy tắc đa số tần số với tỷ số hòa cho các nhãn lớn hơn. 

Mặc dù các ràng buộc rất nhỏ nhưng logic vẫn chứa đựng một chi tiết tinh tế trong quy tắc ràng buộc. Việc triển khai đơn giản chỉ theo dõi tần suất tối đa mà quên thực thi quy tắc "nhãn lớn nhất sẽ thắng" sẽ tạo ra kết quả không chính xác bất cứ khi nào nhiều số có tần suất như nhau. 

Ví dụ, hãy xem xét đầu vào`1 2 2 3 3 4 4 5 5 6`. Ở đây, các số 2, 3, 4 và 5 đều xuất hiện hai lần. Câu trả lời đúng là 5 vì nó lớn nhất trong số những câu trả lời có tần số tối đa. Giải pháp chỉ trả về số đầu tiên đạt tần số tối đa có thể trả về không chính xác 2 hoặc 3 tùy theo thứ tự lặp. 

Một trường hợp khác xuất hiện khi tất cả các phiếu bầu đều giống hệt nhau, chẳng hạn như`7 7 7 7 7 7 7 7 7 7`. Đầu ra phải là 7, điều này đơn giản nhưng xác nhận rằng thuật toán xử lý phân phối đồng đều một cách chính xác. 

Vì kích thước đầu vào được cố định ở mức 10 nên mọi cách tiếp cận hợp lý sẽ diễn ra ngay lập tức. Ngay cả mô phỏng O(n²) cũng có thể được chấp nhận, nhưng cấu trúc tự nhiên gợi ý một cách tiếp cận đếm tần số. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để giải quyết vấn đề này là mô phỏng việc đếm riêng cho từng ứng cử viên có thể có từ 1 đến 10. Đối với mỗi ứng cử viên, chúng tôi quét tất cả mười phiếu bầu và đếm xem nó xuất hiện bao nhiêu lần. Sau đó, chúng tôi theo dõi ứng viên tốt nhất dựa trên tần suất cao nhất, phá vỡ mối quan hệ bằng cách chọn số lớn hơn. 

Cách tiếp cận này hoạt động chính xác vì nó trực tiếp đánh giá từng ứng viên một cách độc lập. Tuy nhiên, nó quét lại cùng một danh sách cho từng ứng viên một cách dư thừa, dẫn đến công việc phải lặp đi lặp lại. Nói chung, nếu số lượng phiếu bầu lớn, chẳng hạn như n, thì giá trị này sẽ trở thành O(10n), điều này vẫn ổn ở đây nhưng về mặt khái niệm thì không hiệu quả và không thể mở rộng nếu không gian ứng cử viên cũng tăng lên. 

Điều quan trọng là chúng ta không cần phải quét nhiều lần. Một lần truyền là đủ để tính toán tất cả các tần số. Khi chúng ta biết tần số của mọi số, việc chọn ứng cử viên tốt nhất sẽ trở thành một bài toán so sánh đơn giản trên một tập hợp cố định có kích thước 10. 

Điều này làm giảm vấn đề duy trì một bảng tần số nhỏ và sau đó tìm mức tối đa theo quy tắc đặt hàng tùy chỉnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(100) | O(1) | Đã chấp nhận | 
| Đếm tần số tối ưu | O(10) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`freq`có kích thước 11 (chỉ số từ 1 đến 10) chứa đầy số không. Mảng này lưu trữ số lần mỗi số vấn đề được bình chọn. 
2. Đọc mười giá trị đầu vào và tăng tần số tương ứng cho mỗi giá trị. Mỗi phiếu bầu trực tiếp cập nhật số lượng của ứng cử viên đó. 
3. Khởi tạo hai biến:`best_value = 1`Và`best_count = 0`. Chúng lưu trữ ứng cử viên chiến thắng hiện tại và tần suất của nó. 
4. Lặp lại qua tất cả các số ứng cử viên từ 1 đến 10. Đối với mỗi ứng cử viên, hãy so sánh tần số của nó với tần số tốt nhất hiện tại. 
5. Nếu ứng cử viên hiện tại có tần suất cao hơn hẳn so với`best_count`, cập nhật`best_value`Và`best_count`đối với ứng viên này và tần suất của nó. 
6. Nếu tần số bằng`best_count`, cập nhật`best_value`chỉ khi số lượng ứng viên hiện tại lớn hơn. Điều này thực thi quy tắc ràng buộc. 
7. Sau khi quét tất cả các ứng viên, xuất ra`best_value`. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bản tóm tắt chính xác của tất cả các phiếu bầu trong`freq`, do đó không có thông tin nào bị mất sau lần chuyển đầu tiên. Giai đoạn thứ hai chỉ so sánh các ứng cử viên dựa trên tần số tổng hợp của họ, điều này là đủ vì quy tắc sắp xếp chỉ phụ thuộc vào tần suất và giá trị. Ở mỗi bước,`best_value`đại diện cho ứng cử viên có số lượng lớn nhất trong số những ứng cử viên được nhìn thấy cho đến nay với tần suất tối đa, vì vậy khi quá trình quét kết thúc, không có ứng cử viên nào chưa được nhìn thấy có thể cải thiện kết quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

freq = [0] * 11

votes = list(map(int, input().split()))
for v in votes:
    freq[v] += 1

best_value = 1
best_count = freq[1]

for i in range(2, 11):
    if freq[i] > best_count or (freq[i] == best_count and i > best_value):
        best_value = i
        best_count = freq[i]

print(best_value)
```Đầu tiên, mã này xây dựng bảng tần số theo một đường tuyến tính duy nhất trên mười phiếu bầu. Vòng lặp thứ hai thực hiện lựa chọn xác định ứng viên tối ưu. Điều kiện ràng buộc được xử lý rõ ràng bằng cách kiểm tra tính bằng nhau của số lượng và ưu tiên chỉ số lớn hơn. 

Một điểm tinh tế là khởi tạo`best_count`với`freq[1]`thay vì số không. Điều này tránh việc suy luận trong trường hợp đặc biệt không cần thiết và đảm bảo logic so sánh luôn hoạt động thống nhất ngay cả khi tất cả các tần số đều bằng 0 ngoại trừ một. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`1 2 2 3 3 3 4 4 4 4`Chúng tôi tính toán tần số đầu tiên. 

| Bước | Ứng viên | Tần số | Giá Trị Tốt Nhất | Số lượng tốt nhất | 
| --- | --- | --- | --- | --- | 
| Ban đầu | - | - | 1 | 0 | 
| 1 | 1 | 1 | 1 | 1 | 
| 2 | 2 | 2 | 2 | 2 | 
| 3 | 3 | 3 | 3 | 3 | 
| 4 | 4 | 4 | 4 | 4 | 

Câu trả lời cuối cùng là 4 vì nó có tần suất cao nhất. 

Dấu vết này cho thấy cách thuật toán cập nhật người chiến thắng một cách đơn điệu khi tần số cao hơn xuất hiện. 

### Ví dụ 2 

đầu vào:`1 1 2 2 3 3 4 4 5 5`Tần số đều bằng 2 cho mọi số từ 1 đến 5. 

| Bước | Ứng viên | Tần số | Giá Trị Tốt Nhất | Số lượng tốt nhất | 
| --- | --- | --- | --- | --- | 
| Ban đầu | - | - | 1 | 0 | 
| 1 | 1 | 2 | 1 | 2 | 
| 2 | 2 | 2 | 2 | 2 | 
| 3 | 3 | 2 | 3 | 2 | 
| 4 | 4 | 2 | 4 | 2 | 
| 5 | 5 | 2 | 5 | 2 | 

Câu trả lời cuối cùng là 5 vì tất cả các ứng cử viên đều hòa và chỉ số lớn nhất sẽ thắng. 

Điều này khẳng định rằng quy tắc ràng buộc được thực hiện chính xác thông qua việc so sánh chặt chẽ về chỉ số khi tần số bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(10) | Một lượt kiểm phiếu và một lượt vượt qua 10 ứng cử viên | 
| Không gian | O(1) | Mảng tần số có kích thước cố định không phụ thuộc vào kích thước đầu vào | 

Kích thước bài toán là không đổi, do đó thuật toán chạy trong thời gian không đổi và dễ dàng đáp ứng mọi ràng buộc cuộc thi điển hình. Ngay cả khi được mở rộng quy mô, cấu trúc tương tự sẽ vẫn hiệu quả do không gian ứng viên bị giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    freq = [0] * 11
    votes = list(map(int, input().split()))
    for v in votes:
        freq[v] += 1

    best_value = 1
    best_count = freq[1]

    for i in range(2, 11):
        if freq[i] > best_count or (freq[i] == best_count and i > best_value):
            best_value = i
            best_count = freq[i]

    return str(best_value)

# provided sample-style case
assert run("1 2 3 4 2 3 4 3 4 4") == "4"

# all equal
assert run("7 7 7 7 7 7 7 7 7 7") == "7"

# tie with largest wins
assert run("1 1 2 2 3 3 4 4 5 5") == "5"

# single dominant
assert run("10 10 10 1 2 3 4 5 6 7") == "10"

# no tie, clear winner
assert run("1 2 3 4 5 6 7 8 8 8") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bằng nhau 7 | 7 | xử lý phân phối thống nhất | 
| xen kẽ các cặp 1-5 | 5 | hòa theo hướng lớn nhất | 
| chiếm ưu thế 10 | 10 | trường hợp đa số rõ ràng | 
| ba cuối cùng bằng 8 | 8 | tính chính xác của tập hợp tần số | 

## Vỏ cạnh 

Kịch bản tất cả đều bình đẳng được xử lý một cách tự nhiên theo quy tắc hòa. Khi mọi tần số đều giống nhau, việc quét qua các ứng cử viên luôn ưu tiên các chỉ số lớn hơn vì sự bằng nhau sẽ kích hoạt`i > best_value`tình trạng. 

Ví dụ, với đầu vào`7 7 7 7 7 7 7 7 7 7`, mảng tần số có`freq[7] = 10`và tất cả những thứ khác bằng không. Thuật toán khởi tạo với ứng viên 1, sau đó đạt đến 7, nơi tần số tối đa được tìm thấy và lưu trữ. Không có ứng cử viên nào sau đó vượt quá nó, vì vậy 7 vẫn là câu trả lời cuối cùng. 

Trong một trường hợp cà vạt đầy đủ như`1 1 2 2 3 3 4 4 5 5`, mọi ứng cử viên từ 1 đến 5 đều có tần số 2. Thuật toán cập nhật liên tục giá trị tốt nhất ở mỗi bước đẳng thức, kết thúc ở 5. Điều này phản ánh trực tiếp quy tắc phá vỡ ràng buộc và xác nhận rằng đẳng thức được xử lý nhất quán trong quá trình quét thay vì sau đó.
