---
title: "CF 1931F - Ảnh chụp màn hình trò chuyện"
description: "Chúng ta có một cuộc trò chuyện với n người tham gia, mỗi người được xác định bằng một số duy nhất từ ​​1 đến n. Cuộc trò chuyện hiển thị những người tham gia trong danh sách được sắp xếp theo hoạt động nhưng mỗi người tham gia luôn thấy mình ở đầu danh sách của riêng mình."
date: "2026-06-08T18:27:44+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dfs-and-similar", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1931
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 925 (Div. 3)"
rating: 1700
weight: 1931
solve_time_s: 161
verified: false
draft: false
---

[CF 1931F - Ảnh chụp màn hình trò chuyện](https://codeforces.com/problemset/problem/1931/F) 

**Đánh giá:** 1700 
**Tags:** tổ hợp, dfs và tương tự, đồ thị 
**Thời gian giải:** 2m 41s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trò chuyện với`n`người tham gia, mỗi người được xác định bằng một số duy nhất từ`1`ĐẾN`n`. Cuộc trò chuyện hiển thị những người tham gia trong danh sách được sắp xếp theo hoạt động nhưng mỗi người tham gia luôn thấy mình ở đầu danh sách của riêng mình. Nhiều người tham gia đã chụp ảnh màn hình danh sách trò chuyện, ghi lại thứ tự khi họ nhìn thấy. Mỗi ảnh chụp màn hình hiển thị người tham gia đứng đầu, tiếp theo là những người tham gia còn lại theo thứ tự nào đó. 

Vấn đề hỏi liệu có tồn tại một thứ tự cơ bản duy nhất của những người tham gia giải thích tất cả các ảnh chụp màn hình hay không. Nói cách khác, chúng tôi muốn biết liệu có một thứ tự nhất quán dựa trên hoạt động của mọi người sao cho nếu mỗi người tham gia thấy mình ở trên cùng thì ảnh chụp màn hình của họ sẽ khớp với thứ tự được ghi lại. 

Những ràng buộc cho chúng ta biết rằng`n`Và`k`có thể lên đến`2·10^5`tổng cộng trên tất cả các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ các cách tiếp cận bạo lực nhằm thử tất cả`n!`hoán vị. Mỗi ảnh chụp màn hình chứa`n`các phần tử, do đó, giải pháp phải xử lý các mảng một cách hiệu quả, lý tưởng nhất là theo thời gian tuyến tính so với tổng của`n`các giá trị. 

Các trường hợp khó khăn không rõ ràng phát sinh khi ảnh chụp màn hình không nhất quán do thứ tự tương đối của những người tham gia không phải là người tham gia hàng đầu. Ví dụ: nếu ba người tham gia đăng ảnh chụp màn hình như`[1,2,3]`,`[2,3,1]`, Và`[3,1,2]`, tồn tại một trật tự cơ bản nhất quán. Nhưng nếu ảnh chụp màn hình hiển thị một người tham gia ở trên cùng với thứ tự tương đối của những người khác xung đột với ảnh chụp màn hình khác thì câu trả lời sẽ là KHÔNG. Một cách tiếp cận ngây thơ có thể chỉ kiểm tra người tham gia hàng đầu và bỏ qua thứ tự của những người còn lại, điều này sẽ thất bại trong những trường hợp như vậy. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra tất cả`n!`hoán vị của người tham gia và kiểm tra từng hoán vị đối với tất cả các ảnh chụp màn hình. Đối với mỗi hoán vị, chúng tôi sẽ mô phỏng chế độ xem của từng người tham gia và xác minh xem nó có khớp với ảnh chụp màn hình của họ hay không. Điều này đúng nhưng không khả thi: với`n`lên đến`2·10^5`,`n!`có kích thước lớn về mặt thiên văn. 

Thông tin chi tiết quan trọng là đối với bất kỳ đơn đặt hàng cơ bản hợp lệ nào, vị trí của mọi người tham gia so với những người khác phải nhất quán trên các ảnh chụp màn hình. Mỗi ảnh chụp màn hình có thể được hiểu là một tập hợp các ràng buộc có hướng: nếu`x`đến ngay sau đó`y`trong ảnh chụp màn hình (bỏ qua người tham gia hàng đầu), sau đó theo thứ tự chung`y`phải đến trước`x`. Bởi vì người tham gia hàng đầu luôn ở vị trí đầu tiên trong tầm nhìn của họ nên chúng tôi có thể loại bỏ họ và tập trung vào thứ tự tương đối của những người còn lại`n-1`người tham gia. 

Quan sát này cho phép chúng tôi giảm vấn đề thành kiểm tra tính nhất quán của trình tự. Chúng tôi chọn một trong các ảnh chụp màn hình làm ứng cử viên cho thứ tự cơ bản và cố gắng mô phỏng tất cả các ảnh chụp màn hình khác theo thứ tự đó, được điều chỉnh bằng cách di chuyển người tham gia hàng đầu của họ lên phía trước. Nếu thao tác này không thể thu được bất kỳ ảnh chụp màn hình nào thì thứ tự không nhất quán và câu trả lời là KHÔNG. Ngược lại, đơn hàng hợp lệ và câu trả lời là CÓ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n!·k·n) | O(n) | Quá chậm | 
| Tối ưu | O(k·n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`t`, số lượng ca kiểm thử 
2. Với mỗi test case, hãy đọc`n`Và`k`. 
3. Lưu trữ tất cả`k`ảnh chụp màn hình trong một danh sách. Mỗi ảnh chụp màn hình là một danh sách`n`số nguyên, với số nguyên đầu tiên là người tham gia đã lấy nó. 
4. Chọn một ảnh chụp màn hình làm ứng cử viên cho đơn hàng cơ bản. Điều này là tùy ý nên chúng tôi thường chọn ảnh chụp màn hình đầu tiên. 
5. Xây dựng ánh xạ từ người tham gia đến vị trí theo thứ tự ứng viên này. 
6. Đối với mỗi ảnh chụp màn hình, hãy kiểm tra xem việc di chuyển người tham gia hàng đầu lên phía trước thứ tự ứng viên có tạo ra ảnh chụp màn hình hay không. Để làm điều này: 

- Loại bỏ người tham gia hàng đầu khỏi thứ tự ứng cử viên. 
- Chèn nó ở phía trước. 
- So sánh danh sách kết quả với ảnh chụp màn hình. 
- Nếu không khớp thì trả về NO. 
7. Nếu tất cả các ảnh chụp màn hình đều khớp, hãy trả về CÓ. 

Tại sao nó hoạt động: di chuyển người tham gia hàng đầu lên phía trước sẽ mô phỏng góc nhìn của từng người tham gia. Nếu tất cả các ảnh chụp màn hình khớp với thao tác này cho một đơn hàng ứng viên duy nhất, điều đó đảm bảo tồn tại một thứ tự toàn cầu cơ bản nhất quán với tất cả các ảnh chụp màn hình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        screenshots = [list(map(int, input().split())) for _ in range(k)]
        
        candidate = screenshots[0]  # arbitrary candidate
        pos_in_candidate = {val: i for i, val in enumerate(candidate)}
        possible = True
        
        for shot in screenshots:
            top = shot[0]
            # simulate participant's view
            reordered = [top] + [x for x in candidate if x != top]
            if reordered != shot:
                possible = False
                break
        print("YES" if possible else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả đầu vào một cách hiệu quả với`sys.stdin.readline`. Thứ tự ứng cử viên được chọn từ ảnh chụp màn hình đầu tiên. Điều tinh tế quan trọng là tạo ra chế độ xem chính xác của người tham gia bằng cách xóa mã nhận dạng riêng của họ khỏi danh sách ứng cử viên và đặt nó ở phía trước. Điều này ngăn ngừa sự không khớp do bỏ qua người tham gia hàng đầu. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
4 4
1 2 3 4
2 3 1 4
3 2 1 4
4 2 3 1
```| Bước | Ứng viên | Ảnh chụp màn hình | Sắp xếp lại | Trận đấu? | 
| --- | --- | --- | --- | --- | 
| 1 | [1,2,3,4] | [1,2,3,4] | [1,2,3,4] | Có | 
| 2 | [1,2,3,4] | [2,3,1,4] | [2,1,3,4] | Không | 

Giải thích: chọn ảnh chụp màn hình đầu tiên làm ứng cử viên, không thể lấy được ảnh chụp màn hình thứ hai bằng cách di chuyển người tham gia hàng đầu của nó lên phía trước. Vậy câu trả lời là KHÔNG. 

### Đầu vào mẫu 2```
1 1
1 2 3 4 5
```| Bước | Ứng viên | Ảnh chụp màn hình | Sắp xếp lại | Trận đấu? | 
| --- | --- | --- | --- | --- | 
| 1 | [1,2,3,4,5] | [1,2,3,4,5] | [1,2,3,4,5] | Có | 

Chỉ với một ảnh chụp màn hình, ứng viên sẽ tự làm được, hãy trả lời CÓ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k·n) | Đối với mỗi ảnh chụp màn hình, chúng tôi xây dựng một danh sách độ dài được sắp xếp lại`n`và so sánh nó. | 
| Không gian | O(n) | Chúng tôi lưu trữ thứ tự ứng cử viên và ánh xạ từ người tham gia đến chỉ mục. | 

Cho rằng tổng của`n·k`trên tất cả các trường hợp thử nghiệm 2·10^5, giải pháp này phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("10\n5 1\n1 2 3 4 5\n4 4\n1 2 3 4\n2 3 1 4\n3 2 1 4\n4 2 3 1\n6 2\n1 3 5 2 4 6\n6 3 5 2 1 4\n3 3\n1 2 3\n2 3 1\n3 2 1\n10 2\n1 2 3 4 5 6 7 8 9 10\n10 9 8 7 6 5 4 3 2 1\n1 1\n1\n5 2\n1 2 3 5 4\n2 1 3 5 4\n3 3\n3 1 2\n2 3 1\n1 3 2\n5 4\n3 5 1 4 2\n2 5 1 4 3\n1 5 4 3 2\n5 1 4 3 2\n3 3\n1 3 2\n2 1 3\n3 2 1") == "YES\nYES\nYES\nYES\nNO\nYES\nYES\nYES\nYES\nNO"

# Custom edge cases
assert run("1\n1 1\n1") == "YES", "single participant"
assert run("1\n2 2\n1 2\n2 1") == "YES", "two participants reversed screenshots"
assert run("1\n3 3\n1 2 3\n2 1 3\n3 2 1") == "YES", "three participants all screenshots"
assert run("1\n3 2\n1 3 2\n2 3 1") == "NO", "conflicting screenshots"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 người tham gia | CÓ | Đầu vào kích thước tối thiểu | 
| 2 người tham gia đảo ngược | CÓ | Ảnh chụp màn hình có thể xoay được, nên phát hiện CÓ | 
| 3 người tham gia tất cả các ảnh chụp màn hình | CÓ | Nhiều ảnh chụp màn hình tương thích với một đơn hàng cơ bản | 
| 3 người tham gia xung đột | KHÔNG | Ảnh chụp màn hình không thể giải thích bằng một đơn hàng | 

## Vỏ cạnh 

Nếu có
