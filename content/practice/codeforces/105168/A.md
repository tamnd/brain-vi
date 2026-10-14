---
title: "CF 105168A - Ngày Hôm Qua Điên Rồ"
description: "Chúng ta được cung cấp một chuỗi truy vấn, trong đó mỗi truy vấn cho chúng ta biết “hôm nay” là ngày nào trong tuần, được mã hóa dưới dạng số nguyên từ 1 đến 7. Ánh xạ cố định: 1 tương ứng với Thứ Hai, 2 đến Thứ Ba, v.v. cho đến 7 tương ứng với Chủ Nhật."
date: "2026-06-27T08:34:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105168
codeforces_index: "A"
codeforces_contest_name: "2024 Fujian Normal University Programming Contest"
rating: 0
weight: 105168
solve_time_s: 36
verified: true
draft: false
---

[CF 105168A - Ngày hôm qua điên rồ](https://codeforces.com/problemset/problem/105168/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi truy vấn, trong đó mỗi truy vấn cho chúng ta biết “hôm nay” là ngày nào trong tuần, được mã hóa dưới dạng số nguyên từ 1 đến 7. Ánh xạ cố định: 1 tương ứng với Thứ Hai, 2 đến Thứ Ba, v.v. cho đến 7 tương ứng với Chủ Nhật. Đối với mỗi truy vấn, chúng ta phải xuất ra ngày trong tuần ngay trước nó trong chu kỳ hàng tuần. 

Cấu trúc có hình tròn, nghĩa là sau Chủ Nhật lại đến thứ Hai. Vì vậy, lùi một bước so với bất kỳ ngày nào sẽ làm giảm số đi một hoặc giảm từ 1 xuống còn 7. 

Kích thước đầu vào có thể đạt tới 200.000 trường hợp thử nghiệm, điều này ngay lập tức loại trừ mọi hoạt động không liên tục trên mỗi truy vấn. Chúng tôi cần tính toán O(1) cho mỗi trường hợp thử nghiệm, nếu không giải pháp có nguy cơ bị hết thời gian. Vì mỗi truy vấn là độc lập nên không cần chia sẻ trạng thái hoặc tiền xử lý ngoài việc hiểu ánh xạ. 

Trường hợp có cạnh tinh tế duy nhất là phần bọc xung quanh từ thứ Hai. Nếu hôm nay là 1 (Thứ Hai) thì hôm qua không phải là 0 mà là 7 (Chủ Nhật). Cách tiếp cận phép trừ ngây thơ mà không xử lý ranh giới này sẽ âm thầm tạo ra kết quả không hợp lệ. 

Ví dụ: nếu đầu vào là 1, việc triển khai bất cẩn có thể tính toán 1 - 1 = 0, nằm ngoài phạm vi hợp lệ. Câu trả lời đúng phải là 7. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ coi mỗi truy vấn như duyệt qua danh sách tuần cho đến khi tìm thấy phần tử trước đó. Chúng tôi có thể lưu trữ rõ ràng trình tự`[1, 2, 3, 4, 5, 6, 7]`và đối với mỗi truy vấn, hãy quét ngược từ vị trí đã cho cho đến khi chúng ta tìm thấy truy vấn trước đó. Điều này hiệu quả vì cấu trúc rất nhỏ và cố định, nên độ chính xác là không đáng kể. 

Tuy nhiên, mặc dù quy mô tuần không đổi, việc triển khai mạnh mẽ sử dụng vòng lặp cho mỗi truy vấn là chi phí không cần thiết. Quan trọng hơn, nó không khái quát hóa tốt theo mô hình đã định: tuần là một chu kỳ mô-đun. Quan sát quan trọng là bài toán chỉ đơn giản là số học theo chu trình modulo-7, trong đó chúng ta trừ đi một và bao quanh. 

Điều này làm giảm mỗi truy vấn thành một phép biến đổi theo thời gian không đổi: tính toán`(w - 2) % 7 + 1`. Công thức này chuyển chỉ số 1-7 thành hệ thống 0-6, áp dụng mức giảm an toàn và chuyển đổi lại. Ngoài ra, chúng ta có thể xử lý ranh giới một cách rõ ràng: nếu`w == 1`, đầu ra`7`, nếu không thì xuất ra`w - 1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(t) nhưng quét theo truy vấn không cần thiết | O(1) | Được chấp nhận nhưng quá mức cần thiết | 
| Tối ưu | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Mỗi trường hợp thử nghiệm đều độc lập nên chúng tôi xử lý chúng một cách tuần tự. 
2. Với mỗi giá trị đầu vào`w`, kiểm tra xem nó có bằng 1 hay không. Đây là trường hợp đặc biệt duy nhất vì tuần có chu kỳ lùi từ thứ Hai đến Chủ Nhật. 
3. Nếu`w == 1`, đầu ra 7. Điều này trực tiếp mã hóa hành vi bao trùm của chu kỳ lịch. 
4. Ngược lại, xuất ra`w - 1`. Điều này xử lý tất cả các chuyển đổi khác trong thời gian không đổi. 

### Tại sao nó hoạt động 

Các ngày tạo thành một chu kỳ khép kín có kích thước 7, trong đó mỗi ngày có đúng một ngày trước đó. Phép trừ 1 chính xác sẽ di chuyển ngược lại dọc theo chu trình này ngoại trừ tại ranh giới, nơi biểu diễn phá vỡ tính tuyến tính. Sự điều chỉnh rõ ràng cho`w = 1`khôi phục cấu trúc tuần hoàn. Vì mọi đầu vào hợp lệ đều ánh xạ tới chính xác một đầu vào trước đó và không có hai đầu vào nào chia sẻ các đầu ra xung đột nhau nên quá trình chuyển đổi vừa hoàn chỉnh vừa nhất quán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        w = int(input())
        if w == 1:
            out.append("7")
        else:
            out.append(str(w - 1))
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai đọc tất cả các truy vấn và lưu trữ kết quả đầu ra trong một danh sách để tránh chi phí I/O lặp lại. Mỗi truy vấn được xử lý bằng một lần kiểm tra có điều kiện duy nhất, đảm bảo xử lý liên tục trong thời gian. 

Chi tiết triển khai duy nhất quan trọng là tránh in trực tiếp bên trong vòng lặp, vì I/O Python có thể trở thành nút thắt cổ chai ở 200.000 thao tác. Tích lũy kết quả và ghi một lần đảm bảo hiệu suất có thể dự đoán được. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
5
1
7
```| Truy vấn | w | Kiểm tra (w == 1) | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | 5 | Không | 4 | 
| 2 | 1 | Có | 7 | 
| 3 | 7 | Không | 6 | 

Truy vấn đầu tiên thể hiện mức giảm bình thường. Chương trình thứ hai kéo dài từ thứ Hai đến Chủ nhật. Thứ ba xác nhận ranh giới trên hoạt động giống như mức giảm bình thường. 

### Ví dụ 2 

đầu vào:```
4
2
3
4
1
```| Truy vấn | w | Kiểm tra (w == 1) | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | 2 | Không | 1 | 
| 2 | 3 | Không | 2 | 
| 3 | 4 | Không | 3 | 
| 4 | 1 | Có | 7 | 

Trình tự này đi qua một quá trình truyền ngược hoàn toàn của chu trình và xác nhận tính nhất quán của các chuyển đổi trên tất cả các giá trị bên trong và ranh giới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm yêu cầu một phép so sánh và số học duy nhất | 
| Không gian | O(1) | Chỉ có bộ nhớ bổ sung liên tục được sử dụng ngoài bộ nhớ đầu ra | 

Các ràng buộc cho phép tối đa 200.000 truy vấn và mỗi truy vấn được xử lý trong thời gian không đổi. Giải pháp phù hợp một cách thoải mái trong cả giới hạn thời gian và bộ nhớ, với chi phí chủ yếu là I/O tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        w = int(input())
        if w == 1:
            output.append("7")
        else:
            output.append(str(w - 1))
    return "\n".join(output)

# provided samples
assert run("3\n5\n1\n7\n") == "4\n7\n6"

# custom cases
assert run("1\n1\n") == "7", "minimum boundary"
assert run("1\n7\n") == "6", "upper boundary"
assert run("7\n1\n2\n3\n4\n5\n6\n7\n") == "7\n1\n2\n3\n4\n5\n6", "full cycle"
assert run("3\n2\n2\n2\n") == "1\n1\n1", "repeated same day"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 → 1 | 7 | bao quanh ranh giới tối thiểu | 
| 7 → 7 | 6 | giảm ranh giới trên | 
| chu kỳ đầy đủ | tuần tự | tính chính xác trong suốt chu kỳ | 
| lặp lại 2 | 1 giây | ổn định trên các đầu vào giống hệt nhau lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp cạnh có ý nghĩa duy nhất là ranh giới chu kỳ vào thứ Hai. 

đầu vào:```
1
1
```Đây`w = 1`, do đó thuật toán lấy nhánh đặc biệt và đưa ra`7`. Séc`w == 1`kích hoạt ngay lập tức, do đó không thực hiện phép trừ số học. 

Để kiểm tra ranh giới thứ hai: 

đầu vào:```
1
2
```Từ`w != 1`, đầu ra của thuật toán`2 - 1 = 1`. Điều này xác nhận rằng các giá trị không biên hoạt động thống nhất và không yêu cầu xử lý đặc biệt ngoài quy tắc giảm. 

Hai trường hợp này cùng nhau xác nhận rằng hàm chuyển đổi mô hình chính xác mối quan hệ tiền nhiệm theo chu kỳ đầy đủ trong hệ thống 7 ngày.
