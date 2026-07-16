---
title: "CF 103411A - \u0414\u0438\u0441\u0442\u0430\u043d\u0446\u0438\u043e\u043d\u043d\u043e\u0435 \u043e\u0431\u0443\u0447\u0435\u043d\u0438\u0435"
description: "Chúng tôi đang tổ chức các lớp học trực tuyến bằng cách sử dụng một số lượng hội nghị video cố định. Mỗi hội nghị đều có giới hạn cứng về tổng số người tham gia và trong mỗi hội nghị, một số ghế cố định phải được dành riêng cho giáo viên. Những chỗ còn lại, nếu có, có thể được lấp đầy bởi học sinh."
date: "2026-07-03T10:55:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103411
codeforces_index: "A"
codeforces_contest_name: "2020-2021, ICPC, East Siberian Regional Contest"
rating: 0
weight: 103411
solve_time_s: 44
verified: true
draft: false
---

[CF 103411A - \u0414\u0438\u0441\u0442\u0430\u043d\u0446\u0438\u043e\u043d\u043d\u043e\u0435 \u043e\u0431\u0443\u0447\u0435\u043d\u0438\u0435](https://codeforces.com/problemset/problem/103411/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang tổ chức các lớp học trực tuyến bằng cách sử dụng một số lượng hội nghị video cố định. Mỗi hội nghị đều có giới hạn cứng về tổng số người tham gia và trong mỗi hội nghị, một số ghế cố định phải được dành riêng cho giáo viên. Những chỗ còn lại, nếu có, có thể được lấp đầy bởi học sinh. Một sinh viên được phép tham dự tối đa một hội nghị, vì vậy chúng tôi không thể sử dụng lại sinh viên qua các phiên. 

Nhiệm vụ là xác định tổng số sinh viên có thể được tổ chức trong tất cả các hội nghị có sẵn trong những ràng buộc này. 

Đầu vào cung cấp số lượng hội nghị có sẵn, sức chứa tối đa của mỗi hội nghị và số lượng vị trí trong số đó đã được dành riêng cho giáo viên. Đầu ra là tổng số vị trí sinh viên trên tất cả các hội nghị. 

Các hạn chế rất nhỏ: lên tới 1000 hội nghị và mỗi hội nghị hỗ trợ tối đa 100 người tham gia. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào chạy trong thời gian tuyến tính trên các giá trị này là đủ, vì ngay cả một phép tính trực tiếp cũng bao gồm tối đa vài nghìn phép tính số học. 

Một sai lầm ngây thơ xuất phát từ việc hiểu sai liệu học sinh được phân bổ độc lập hay bị hạn chế trên toàn cầu. Hạn chế chính là mỗi hội nghị dành riêng các vị trí cho giáo viên một cách độc lập, do đó năng lực của học sinh là trên mỗi hội nghị chứ không phải chia sẻ. 

Một cạm bẫy tiềm ẩn khác là quên rằng thời gian dành cho giáo viên sẽ làm giảm năng lực của học sinh ngay cả khi hội nghị không được sử dụng hết. Ví dụ: nếu có 10 hội nghị, mỗi hội nghị có 10 giáo viên và 9 giáo viên thì chỉ có 1 sinh viên phù hợp cho mỗi hội nghị, tổng cộng là 10 sinh viên chứ không phải 100. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ mô phỏng việc phân công hội nghị học sinh theo hội nghị, lấp đầy các vị trí giáo viên trước rồi thêm học sinh cho đến khi hội nghị kín hoặc hết học sinh. Điều này sẽ yêu cầu theo dõi từng hội nghị và có khả năng lặp lại từng học sinh một. Mặc dù đúng nhưng đây là chi phí không cần thiết vì cấu trúc đồng nhất. 

Quan sát quan trọng là mọi hội nghị đều có cấu trúc giống hệt nhau: mỗi hội nghị luôn đóng góp chính xác`n - c`khe sinh viên, độc lập với những khe khác. Không có sự tương tác giữa các hội nghị ngoại trừ số lượng hội nghị có sẵn trên toàn cầu`k`. 

Điều này thu gọn bài toán thành một phép nhân đơn giản: mỗi hội nghị đóng góp một số lượng ghế sinh viên cố định và chúng ta có`k`những hội nghị như vậy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng tàn bạo | O(k · n) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Công thức trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số hội nghị`k`, công suất tối đa`n`và số lượng giáo viên`c`. Những điều này xác định có bao nhiêu suất sinh viên tồn tại trong mỗi hội nghị và có bao nhiêu hội nghị. 
2. Tính số lượng sinh viên trong một hội nghị như sau`n - c`. Điều này phản ánh thực tế rằng chỗ ngồi của giáo viên là bắt buộc và chiếm một phần sức chứa trước khi học sinh có thể được xếp chỗ. 
3. Nhân sức chứa sinh viên mỗi buổi hội thảo với`k`. Điều này tổng hợp các hội nghị độc lập giống hệt nhau thành một tổng thể duy nhất. 
4. Đưa ra kết quả là số lượng học sinh tối đa có thể chứa được. 

### Tại sao nó hoạt động 

Mỗi hội nghị hoạt động độc lập và có cấu trúc giống hệt nhau. Vì mỗi hội nghị luôn dự trữ chính xác`c`chỗ dành cho giáo viên, số lượng học sinh còn lại được cố định ở mức`n - c`. Không có sự kết nối giữa các hội nghị, do đó việc tối đa hóa số lượng sinh viên trên toàn cầu sẽ giảm xuống mức tối đa hóa số lượng sinh viên tại địa phương và tổng hợp trên tất cả các hội nghị. Bất kỳ sai lệch nào so với`n - c`mỗi hội nghị sẽ vi phạm yêu cầu của giáo viên hoặc hạn chế về năng lực, vì vậy giá trị này vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

k = int(input())
n = int(input())
c = int(input())

print(k * (n - c))
```Giải pháp đọc ba số nguyên và áp dụng trực tiếp công thức dẫn xuất. Phép nhân nắm bắt sự tổng hợp trên tất cả các hội nghị. Việc trừ đảm bảo rằng việc đặt chỗ của giáo viên được loại trừ khỏi năng lực của học sinh. 

Không có điều kiện biên nào ngoài việc đảm bảo`c < n`, điều này đảm bảo kết quả luôn dương. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào có 3 hội nghị, mỗi hội nghị cho phép 5 người tham gia, trong đó có 2 hội nghị dành riêng cho giáo viên. 

| k | n | c | sinh viên mỗi hội nghị | tổng số sinh viên | 
| --- | --- | --- | --- | --- | 
| 3 | 5 | 2 | 3 | 9 | 

Mỗi hội nghị đóng góp 3 suất sinh viên, vì vậy qua 3 hội nghị, chúng tôi có 9 sinh viên. 

Bây giờ hãy xem xét trường hợp thứ hai với một hội nghị lớn. 

| k | n | c | sinh viên mỗi hội nghị | tổng số sinh viên | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 1 | 9 | 9 | 

Ở đây kết quả trực tiếp là năng lực sẵn có của sinh viên trong một hội nghị. 

Những ví dụ này xác nhận rằng việc tính toán hoàn toàn mang tính cộng trên các đơn vị độc lập giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các ràng buộc cho phép tối đa 1000 hội nghị, nhưng giải pháp không lặp lại chúng. Thay vào đó, nó sử dụng số học theo thời gian không đổi, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    k = int(sys.stdin.readline())
    n = int(sys.stdin.readline())
    c = int(sys.stdin.readline())
    return str(k * (n - c))

# provided sample-style checks
assert run("3\n5\n2\n") == "9"
assert run("1\n10\n1\n") == "9"

# custom cases
assert run("1\n2\n1\n") == "1", "minimum student capacity"
assert run("1000\n100\n1\n") == str(1000 * 99), "maximum k and near-maximum n"
assert run("5\n10\n9\n") == "5", "only one student per conference"
assert run("10\n100\n0\n") == "1000", "no teachers edge case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1,2,1 | 1 | dung lượng tối thiểu khác 0 | 
| 1000,100,1 | 99000 | phép nhân quy mô lớn | 
| 5,10,9 | 5 | sự thống trị cực độ của giáo viên | 
| 10.100,0 | 1000 | vắng giáo viên | 

## Vỏ cạnh 

Trường hợp một bên là số lượng giáo viên chỉ ít hơn năng lực một giáo viên. Đối với đầu vào`k=5, n=10, c=9`, mỗi hội nghị chỉ cho phép một sinh viên. Thuật toán tính toán`10 - 9 = 1`, sau đó nhân với 5, thu được 5. Điều này phù hợp với hạn chế dự kiến ​​rằng vị trí giáo viên là bắt buộc và luôn tiêu tốn năng lực trước học sinh. 

Một trường hợp khác là khi không có giáo viên, về mặt khái niệm`c = 0`(mặc dù vấn đề nêu rõ`c ≥ 1`, điều này rất hữu ích để suy luận về tính đúng đắn). Vì`k=10, n=100, c=0`, mỗi hội nghị đóng góp đầy đủ 100 vị trí sinh viên và thuật toán trả về`1000`, xử lý đúng đắn mọi năng lực sẵn có của học sinh. 

Tình huống ranh giới cuối cùng là cấu hình nhỏ nhất`k=1, n=2, c=1`. Chỉ có thể xếp một học sinh và công thức cho`1`, phù hợp với kết quả mong đợi mà không yêu cầu bất kỳ xử lý đặc biệt nào.
