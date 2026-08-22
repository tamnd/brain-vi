---
title: "CF 104157B - Hãy chú ý đến lượng đường của bạn!"
description: "Chúng tôi được tặng một bộ sưu tập sôcôla, mỗi loại có một lượng đường xác định. Thomas có giới hạn lượng đường hàng ngày và muốn ăn càng nhiều sôcôla nguyên chất càng tốt mà tổng lượng đường không vượt quá giới hạn đó."
date: "2026-07-02T01:14:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104157
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 01-27-23 Div. 2 (Beginner)"
rating: 0
weight: 104157
solve_time_s: 44
verified: true
draft: false
---

[CF 104157B - Hãy chú ý đến lượng đường của bạn!](https://codeforces.com/problemset/problem/104157/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một bộ sưu tập sôcôla, mỗi loại có một lượng đường xác định. Thomas có giới hạn lượng đường hàng ngày và muốn ăn càng nhiều sôcôla nguyên chất càng tốt mà tổng lượng đường không vượt quá giới hạn đó. Mỗi viên sôcôla được ăn hết hoặc không được ăn chút nào và mục tiêu là tối đa hóa số lượng sôcôla được chọn trong giới hạn tổng số tiền. 

Đầu vào bao gồm một danh sách các số nguyên biểu thị lượng đường trên mỗi sô cô la và một số nguyên duy nhất biểu thị tổng lượng đường tối đa được phép. Đầu ra là một số duy nhất: số lượng sôcôla lớn nhất có lượng đường kết hợp không vượt quá giới hạn. 

Các ràng buộc rất nhỏ: tối đa 100 sôcôla và giá trị đường lên tới tổng giới hạn 10.000. Điều này ngay lập tức gợi ý rằng ngay cả hành vi bậc hai cũng có thể được chấp nhận, vì các phép tính 100² là không đáng kể. Bất cứ điều gì theo cấp số nhân đều không cần thiết nhưng vẫn khả thi về mặt kỹ thuật ở quy mô này. 

Trường hợp then chốt là khi tất cả sôcôla đều vượt quá giới hạn riêng lẻ. Ví dụ, nếu`s = 3`và sôcôla là`[5, 6, 7]`, thì đáp án đúng là`0`. Cách tiếp cận tham lam hoặc dựa trên sắp xếp phải xử lý chính xác thực tế là không cho phép lựa chọn một phần và bỏ qua tất cả các mục là hợp lệ. 

Một trường hợp khó khăn khác là khi tất cả sôcôla đều rất nhỏ và có thể lấy hết được. Ví dụ,`s = 100`và tất cả các giá trị đều`1`, câu trả lời nên ở đâu`n`. 

Một trường hợp thất bại khó phát hiện nếu ai đó cố gắng lấy sôcôla theo thứ tự đầu vào mà không xem xét kích cỡ của chúng. Ví dụ,`s = 10`Và`[6, 6, 1, 1, 1]`. Lấy theo thứ tự sản lượng`6 + 6`ngay lập tức vượt quá giới hạn sau hai lần chọn đầu tiên, tạo ra số lượng dưới mức tối ưu, trong khi chiến lược tốt hơn sẽ là lấy sôcôla nhỏ hơn trước. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử tất cả các tập hợp con sôcôla, tính tổng lượng đường của chúng và theo dõi kích thước tối đa của bất kỳ tập hợp con nào có tổng nằm trong giới hạn. Điều này đúng vì nó trực tiếp khám phá mọi lựa chọn có thể. Tuy nhiên, số lượng tập hợp con là`2^n`, cái nào cho`n = 100`có kích thước lớn về mặt thiên văn, theo thứ tự`10^30`, điều đó làm cho nó hoàn toàn không khả thi. 

Cấu trúc của vấn đề cho thấy một chiến lược đơn giản hơn. Vì chúng ta chỉ quan tâm đến việc tối đa hóa số lượng mục trong một ràng buộc về tổng, nên trước tiên chúng ta nên ưu tiên các mục nhỏ hơn. Theo trực giác, mỗi viên sôcôla có "giá trị" bằng nhau về mặt đóng góp vào số lượng, do đó giảm thiểu chi phí cho mỗi mặt hàng là tối ưu. Việc phân loại sôcôla theo hàm lượng đường và sau đó tham lam lấy những viên nhỏ nhất đảm bảo rằng chúng ta đóng gói càng nhiều đồ càng tốt trước khi đạt đến giới hạn. 

Điều này chuyển đổi vấn đề thành một sự tích lũy tham lam đơn giản trên một mảng được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (liệt kê tập hợp con) | O(2^n · n) | O(n) | Quá chậm | 
| Sắp xếp + Tham lam | O(n log n) | O(1) bổ sung (không bao gồm sắp xếp) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số sôcôla`n`, giới hạn đường`s`và danh sách các giá trị đường. Việc sắp xếp là cần thiết vì thứ tự quyết định mức độ hiệu quả mà chúng ta có thể đóng gói các mặt hàng nhỏ trước tiên. 
2. Sắp xếp danh sách theo thứ tự không giảm. Điều này đảm bảo rằng mọi tiền tố của danh sách đều thể hiện cách rẻ nhất có thể để lấy được nhiều sôcôla như vậy. 
3. Khởi tạo tổng hiện có`total = 0`và một quầy`count = 0`. 
4. Lặp lại các sôcôla được sắp xếp từ nhỏ nhất đến lớn nhất. 
5. Đối với mỗi loại sôcôla, hãy kiểm tra xem có thêm đường vào`total`sẽ vượt quá`s`. Nếu không, hãy đưa nó vào bằng cách cập nhật`total += value`và tăng dần`count += 1`. 
6. Nếu việc thêm sô cô la hiện tại vượt quá giới hạn, hãy dừng lại ngay lập tức. Bất kỳ sôcôla nào sau này đều bằng hoặc lớn hơn, vì vậy không thể thêm sôcôla nào mà không vi phạm ràng buộc. 
7. Đầu ra`count`. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là việc thay thế bất kỳ sô-cô-la nào đã chọn bằng một sô-cô-la nhỏ hơn chỉ có thể cải thiện tính khả thi mà không làm giảm số lượng. Vì tất cả sôcôla đều đóng góp như nhau cho mục tiêu (mỗi sôcôla một cái), nên chiến lược tối ưu luôn ưu tiên các mặt hàng có chi phí thấp hơn. Việc sắp xếp đảm bảo rằng ở mỗi bước, chúng tôi sẽ đưa ra lựa chọn tối ưu cục bộ nhằm duy trì dung lượng còn lại tối đa có thể cho các lượt chọn trong tương lai. Điều này tạo ra một quy trình đơn điệu trong đó số lượng mục được chọn được tối đa hóa một cách tham lam mà không cần quay lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
s = int(input().strip())
a = list(map(int, input().split()))

a.sort()

total = 0
count = 0

for x in a:
    if total + x <= s:
        total += x
        count += 1
    else:
        break

print(count)
```Giải pháp bắt đầu bằng cách sắp xếp mảng, đây là bước không hề tầm thường. Điều này đảm bảo rằng chúng tôi luôn xem xét những viên sôcôla nhỏ hơn trước, điều này rất cần thiết để tối đa hóa số lượng mặt hàng. Vòng lặp duy trì tổng số hiện có và đảm bảo chúng tôi không bao giờ vượt quá giới hạn. Việc nghỉ sớm rất quan trọng vì một khi chúng ta thất bại ở một miếng sôcôla nhất định, tất cả những miếng sôcôla sau đó chắc chắn cũng sẽ quá lớn. 

Một lỗi phổ biến là quên sắp xếp, dẫn đến hành vi tham lam không chính xác dựa trên thứ tự đầu vào. Một vấn đề tế nhị khác là không phá vỡ sớm, điều này không sai nhưng kém hiệu quả hơn một chút. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, s = 10
chocolates = [2, 3, 5, 4, 1]
```Mảng được sắp xếp là`[1, 2, 3, 4, 5]`. 

| Bước | Sô cô la | Tổng số chạy | Đếm | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 2 | 3 | 2 | 
| 3 | 3 | 6 | 3 | 
| 4 | 4 | 10 | 4 | 
| 5 | 5 | dừng lại | 4 | 

Thuật toán chọn bốn viên sôcôla trước khi đạt đến giới hạn một cách chính xác. Điều này cho thấy việc đóng gói tham lam sẽ đạt được sự vừa vặn khi có thể. 

### Ví dụ 2 

đầu vào:```
n = 4, s = 6
chocolates = [4, 2, 3, 5]
```Mảng được sắp xếp là`[2, 3, 4, 5]`. 

| Bước | Sô cô la | Tổng số chạy | Đếm | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | 1 | 
| 2 | 3 | 5 | 2 | 
| 3 | 4 | dừng lại | 2 | 

Ở đây, thuật toán tránh chính xác những viên sôcôla lớn hơn vượt quá giới hạn. Câu trả lời tối ưu là 2, đạt được bằng cách chọn 2 và 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, lặp lại là tuyến tính | 
| Không gian | O(1) thêm | Chỉ các bộ đếm được sử dụng ngoài bộ nhớ đầu vào | 

Các ràng buộc cho phép tối đa 100 mục, do đó việc sắp xếp và vượt qua một lần là không đáng kể trong giới hạn thời gian. Ngay cả việc triển khai kém tối ưu hơn cũng có thể vượt qua, nhưng giải pháp này rõ ràng và có thể mở rộng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    s = int(input().strip())
    a = list(map(int, input().split()))
    a.sort()

    total = 0
    count = 0

    for x in a:
        if total + x <= s:
            total += x
            count += 1
        else:
            break

    return str(count).strip()

# provided samples (conceptual, since not explicitly given)
assert run("5\n10\n2 3 5 4 1\n") == "4"
assert run("4\n6\n4 2 3 5\n") == "2"

# custom cases
assert run("1\n0\n5\n") == "0", "cannot take anything"
assert run("3\n100\n1 1 1\n") == "3", "all fit"
assert run("3\n3\n4 5 6\n") == "0", "all too large"
assert run("5\n7\n6 1 1 1 1\n") == "4", "greedy after sorting"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1, 0, 5`| 0 | trường hợp cạnh ngân sách bằng không | 
|`3, 100, all 1s`| 3 | bao gồm tất cả các mục | 
|`3, 3, large values`| 0 | không có lựa chọn hợp lệ | 
|`5, 7, mixed values`| 4 | sự cần thiết sắp xếp và sự đúng đắn tham lam | 

## Vỏ cạnh 

Đối với trường hợp không thể lấy sô cô la, chẳng hạn như`s = 3`Và`[5, 6, 7]`, sắp xếp cho cùng một mảng. Thuật toán kiểm tra`5`đầu tiên và ngay lập tức dừng lại kể từ`0 + 5 > 3`. Đầu ra vẫn còn`0`, điều này đúng vì không có tập hợp con nào khả thi. 

Đối với trường hợp tất cả sôcôla vừa khít hoặc bị chùng, chẳng hạn như`s = 10`Và`[1, 2, 3, 4]`, sắp xếp tạo ra`[1, 2, 3, 4]`. Thuật toán tích lũy`1 → 3 → 6 → 10`, lấy thành công tất cả các vật phẩm. Điều kiện dừng không bao giờ được kích hoạt sớm, cho thấy thuật toán không kết thúc sớm khi tập hợp đầy đủ hợp lệ.
