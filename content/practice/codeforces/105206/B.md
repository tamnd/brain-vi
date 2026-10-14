---
title: "CF 105206B - \u041f\u0443\u0442\u0435\u0448\u0435\u0441\u0442\u0432\u0438\u0435"
description: "Chúng ta được cho một dãy dài các số nguyên không âm. Chúng ta cũng được cho một giá trị mục tiêu x. Nhiệm vụ là nhìn vào bên trong dãy và tìm một đoạn liền kề có tổng các phần tử chính xác bằng x. Trong số tất cả các đoạn như vậy, chúng ta được yêu cầu chọn một đoạn có độ dài tối đa có thể."
date: "2026-06-27T02:40:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105206
codeforces_index: "B"
codeforces_contest_name: "VitebskOpen Junior"
rating: 0
weight: 105206
solve_time_s: 83
verified: false
draft: false
---

[CF 105206B - \u041f\u0443\u0442\u0435\u0448\u0435\u0441\u0442\u0432\u0438\u0435](https://codeforces.com/problemset/problem/105206/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy dài các số nguyên không âm. Chúng tôi cũng được cung cấp một giá trị mục tiêu`x`. Nhiệm vụ là nhìn vào bên trong chuỗi và tìm một đoạn liền kề có tổng các phần tử chính xác bằng`x`. Trong số tất cả các đoạn như vậy, chúng ta được yêu cầu chọn một đoạn có độ dài tối đa có thể. Nếu một số phân đoạn chia sẻ độ dài tối đa đó thì bất kỳ phân đoạn nào trong số chúng đều được chấp nhận. Nếu không có đoạn nào có tổng chính xác`x`, chúng tôi báo cáo thất bại. 

Đầu ra không phải là đoạn đó mà là hai giá trị mô tả nó: độ dài của nó và vị trí bắt đầu sử dụng chỉ mục dựa trên 1. 

Những hạn chế là những gì định hình giải pháp. Kích thước mảng có thể đạt tới năm triệu phần tử, vì vậy bất kỳ phương pháp nào kiểm tra tất cả các mảng con một cách rõ ràng sẽ không tồn tại được. Ngay cả việc lưu trữ tổng tiền tố trong một mảng đơn giản cũng là giới hạn trong bộ nhớ nhưng vẫn có thể thực hiện được nếu thực hiện cẩn thận. Giới hạn tổng lên tới 10^16, đảm bảo rằng số học 32 bit là không đủ để tích lũy, do đó, số nguyên 64 bit là bắt buộc trong suốt. 

Một thuộc tính cấu trúc quan trọng là tất cả các số đều không âm. Điều này hoàn toàn xác định hướng di chuyển của cửa sổ hợp lệ: khi tổng cửa sổ vượt quá`x`, mở rộng nó hơn nữa không bao giờ có thể khắc phục được tình hình, chỉ thu nhỏ nó mới có thể giúp ích. 

Có một vài trường hợp đặc biệt phá vỡ lý luận ngây thơ: 

Trường hợp cạnh đầu tiên là khi`x = 0`. Vì tất cả các số đều không âm nên các phân đoạn hợp lệ duy nhất là những phân đoạn được tạo thành hoàn toàn bằng số 0. Một cửa sổ trượt ngây thơ giả định mức tăng trưởng dương có thể vẫn hoạt động, nhưng việc triển khai chỉ di chuyển con trỏ khi tổng thay đổi có thể bị kẹt nếu chúng không xử lý việc mở rộng bằng 0 một cách cẩn thận. 

Ví dụ: 

đầu vào:```
5 0
1 0 0 0 2
```Câu trả lời đúng là`3 2`, phân đoạn của ba số không. 

Trường hợp cạnh thứ hai là khi tất cả các phần tử đều lớn và tổng vượt quá`x`ngay lập tức; ở đây chỉ có vấn đề thu hẹp và việc không thu nhỏ chính xác dẫn đến nỗ lực mở rộng vô hạn khi triển khai hai con trỏ. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ liệt kê mọi chỉ số bắt đầu`l`, và với mỗi`l`, mở rộng`r`trong khi duy trì tổng số hiện có cho đến khi nó vượt quá hoặc khớp`x`. Nếu nó khớp, chúng tôi sẽ cập nhật độ dài tốt nhất. Điều này đúng vì mọi mảng con đều được xem xét, nhưng trong trường hợp xấu nhất, nó kiểm tra các phân đoạn O(n^2) và tổng công việc sẽ suy biến thành các phép cộng O(n^2). Với`n = 5 * 10^6`, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng xuất phát từ thực tế là tất cả các số đều không âm. Điều này đảm bảo rằng nếu chúng ta duy trì một cửa sổ`[l, r]`và số tiền trở nên quá lớn, tăng lên`r`chỉ làm cho nó tồi tệ hơn. Hành vi đơn điệu này cho phép chúng ta duy trì một cửa sổ trượt trong đó cả hai điểm cuối chỉ di chuyển về phía trước. Mỗi phần tử vào và ra khỏi cửa sổ nhiều nhất một lần, vì vậy chúng ta có thể theo dõi các tổng hợp lệ theo thời gian tuyến tính. 

Thay vì kiểm tra tất cả các mảng con, chúng tôi duy trì một khoảng động luôn biểu thị tổng hiện tại. Chúng ta tham lam mở rộng đầu bên phải và khi số tiền trở nên quá lớn, chúng ta thu nhỏ đầu bên trái cho đến khi nó có giá trị trở lại. Mỗi khi tổng bằng`x`, chúng tôi cập nhật câu trả lời tốt nhất nếu cửa sổ dài hơn các cửa sổ trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Cửa Sổ Trượt | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai con trỏ`l = 0`,`r = 0`, và tổng chạy`s = 0`. Đồng thời lưu trữ câu trả lời hay nhất`(best_len, best_l)`được khởi tạo là không tìm thấy. Hai con trỏ xác định cửa sổ hiện tại đang được xem xét. 
2. Di chuyển`r`từ trái sang phải qua mảng, thêm`a[r]`ĐẾN`s`ở mỗi bước. Thao tác này sẽ mở rộng cửa sổ hiện tại để bao gồm nhiều phần tử hơn. 
3. Sau mỗi lần mở rộng, trong khi`s > x`Và`l <= r`, trừ`a[l]`từ`s`và tăng dần`l`. Điều này khôi phục tính khả thi của cửa sổ vì tất cả các giá trị đều không âm, nghĩa là thu nhỏ là cách duy nhất để giảm tổng. 
4. Nếu tại bất kỳ thời điểm nào`s == x`, so sánh độ dài cửa sổ hiện tại`r - l + 1`với độ dài tốt nhất được tìm thấy cho đến nay. Nếu nó lớn hơn, hãy cập nhật câu trả lời tốt nhất cho`(length, l + 1)`bởi vì đầu ra sử dụng lập chỉ mục dựa trên 1. 
5. Tiếp tục cho đến khi`r`đến cuối mảng. Câu trả lời được lưu trữ cuối cùng là phân đoạn hợp lệ dài nhất. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên tính chất đơn điệu: khi tổng vượt quá`x`, không có phần mở rộng nào trong tương lai của điểm cuối bên phải có thể khắc phục được sự cố đó nếu không loại bỏ các phần tử ở bên trái trước. Vì tất cả các giá trị đều không âm nên tổng chỉ tăng khi mở rộng`r`, và chỉ giảm khi di chuyển`l`. Điều này đảm bảo mọi mảng con có điểm cuối phù hợp`r`được biểu diễn duy nhất bằng chính xác một vị trí con trỏ trái trong quá trình xử lý. Kết quả là không có cửa sổ hợp lệ nào bị bỏ qua và mọi ứng cử viên đều được đánh giá chính xác khi nó trở nên tối thiểu theo ràng buộc`s <= x`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, x = map(int, input().split())
    a = list(map(int, input().split()))

    l = 0
    s = 0

    best_len = 0
    best_l = -1

    for r in range(n):
        s += a[r]

        while l <= r and s > x:
            s -= a[l]
            l += 1

        if s == x:
            length = r - l + 1
            if length > best_len:
                best_len = length
                best_l = l + 1

    if best_l == -1:
        print(-1)
    else:
        print(best_len, best_l)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì một cửa sổ trượt sử dụng hai chỉ mục. tổng`s`luôn đại diện cho tổng của phân khúc hiện tại`[l, r]`. Vòng lặp bên trong chịu trách nhiệm khôi phục tính hợp lệ bất cứ khi nào tổng vượt quá`x`. Vì mỗi phần tử được loại bỏ khỏi bên trái nhiều nhất một lần nên vòng lặp này không tạo ra hành vi bậc hai. 

Việc cập nhật câu trả lời chỉ xảy ra khi`s == x`và nó theo dõi độ dài cửa sổ tối đa được thấy cho đến nay. Việc chuyển đổi chỉ mục sang dạng dựa trên 1 được xử lý tại thời điểm chỉ định để tránh điều chỉnh lặp lại sau này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 5
1 1 1 1 2 1
```| r | tôi | cửa sổ | tổng hợp | hành động | tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | [1] | 1 | mở rộng | - | 
| 3 | 0 | [1,1,1,1] | 4 | mở rộng | - | 
| 4 | 0 | [1,1,1,1,2] | 6 → thu nhỏ | di chuyển tôi | - | 
| 4 | 4 | [2] | 2 | thu nhỏ xong | - | 
| 5 | 4 | [2,1] | 3 | mở rộng | - | 

Tiếp tục quá trình này, khi cửa sổ trở thành`[1,1,1,1]`bắt đầu từ chỉ số 3, chúng ta nhận được tổng 4, và các cửa sổ con hợp lệ sau đó sẽ được so sánh cho đến khi có kết quả tốt nhất`[1,1,1,1]`đoạn giống nhau được tìm thấy với độ dài 4 bắt đầu từ vị trí 3. 

Dấu vết này cho thấy cách thu nhỏ loại bỏ các phần tử trước đó để thiết lập lại tính hợp lệ và cách thuật toán không bao giờ quay lại ranh giới bên trái khi nó di chuyển qua nó. 

### Ví dụ 2 

đầu vào:```
5 11
4 2 9 0 1
```| r | tôi | cửa sổ | tổng hợp | hành động | tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | [4] | 4 | mở rộng | - | 
| 1 | 0 | [4,2] | 6 | mở rộng | - | 
| 2 | 0 | [4,2,9] | 15 → thu nhỏ | tôi di chuyển | - | 
| 2 | 1 | [2,9] | 11 | trận đấu | (2,2) | 
| 3 | 1 | [2,9,0] | 11 | trận đấu | (3,2) | 
| 4 | 1 | [2,9,0,1] | 12 → thu nhỏ | điều chỉnh | (3,2) | 

Ví dụ này cho thấy số 0 không ảnh hưởng đến các ràng buộc tổng như thế nào và nhiều cửa sổ hợp lệ kết thúc ở các vị trí khác nhau như thế nào.`r`có thể chia sẻ cùng một số tiền, yêu cầu theo dõi độ dài tối đa thay vì lần xuất hiện đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi phần tử được thêm vào và xóa tối đa một lần khỏi cửa sổ trượt | 
| Không gian | O(1) | chỉ có một số bộ đếm được duy trì bên cạnh mảng đầu vào | 

Hành vi tuyến tính là cần thiết cho`n`lên đến năm triệu. Bất kỳ phương pháp bậc hai nào cũng sẽ vượt quá giới hạn thời gian theo một số bậc độ lớn, trong khi bộ nhớ bổ sung không đổi đảm bảo khả năng mở rộng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, x = map(int, input().split(
```
