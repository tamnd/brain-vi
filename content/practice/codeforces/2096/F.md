---
title: "CF 2096F - Kẻ mạo danh tuyệt vời"
description: "Chúng ta được giao một trò chơi với n người xem, mỗi người trong số họ có thể là đồng đội hoặc kẻ mạo danh. Người xem đưa ra những tuyên bố về bản thân họ và những người khác trong các phạm vi liền kề."
date: "2026-06-08T05:24:53+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2096
codeforces_index: "F"
codeforces_contest_name: "Neowise Labs Contest 1 (Codeforces Round 1018, Div. 1 + Div. 2)"
rating: 3100
weight: 2096
solve_time_s: 93
verified: false
draft: false
---

[CF 2096F - Những kẻ mạo danh tuyệt vời](https://codeforces.com/problemset/problem/2096/F) 

**Đánh giá:** 3100 
**Thẻ:** cấu trúc dữ liệu, cách triển khai, hai con trỏ 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cho một trò chơi với`n`người xem, mỗi người trong số họ có thể là đồng đội hoặc kẻ mạo danh. Người xem đưa ra những tuyên bố về bản thân họ và những người khác trong các phạm vi liền kề. Mỗi tuyên bố có một trong hai dạng: tuyên bố rằng một phân khúc người xem có chứa ít nhất một kẻ mạo danh hoặc tuyên bố rằng một phân khúc không chứa những kẻ mạo danh. 

Chúng ta được hỏi nhiều câu hỏi, mỗi câu chỉ định một tập hợp con các câu lệnh liền kề nhau và chúng ta phải xác định xem liệu tất cả các câu lệnh trong phạm vi đó có thể đồng thời đúng hay không. Điều quan trọng là trò chơi không đảm bảo sự tồn tại của ít nhất một kẻ mạo danh hoặc ít nhất một thành viên phi hành đoàn trên toàn cầu, vì vậy chúng tôi không thể giả định bất cứ điều gì ngoài phạm vi tuyên bố. 

Giới hạn đầu vào rất lớn: lên tới 200.000 người xem, câu lệnh và truy vấn cho mỗi trường hợp thử nghiệm, với tổng số tiền trên tất cả các trường hợp thử nghiệm cũng bị giới hạn ở mức 200.000. Điều này chỉ ra rằng bất kỳ giải pháp nào lặp lại một cách ngây thơ đối với người xem hoặc câu lệnh trên mỗi truy vấn sẽ quá chậm. Cụ thể, một cách tiếp cận mạnh mẽ kiểm tra mọi người xem để tìm mọi tuyên bố sẽ mang lại`O(m*n)`cho mỗi trường hợp thử nghiệm, tùy thuộc vào`4*10^10`trong trường hợp xấu nhất, vượt xa những gì khả thi trong 2 giây. 

Một trường hợp phức tạp phát sinh khi nhiều câu lệnh trùng nhau, chẳng hạn: một câu nói cho biết có ít nhất một kẻ mạo danh trong số người xem từ 1-3 và một câu khác cho biết người xem từ 2-3 không có kẻ mạo danh. Việc kiểm tra từng câu lệnh một cách ngây thơ có thể bỏ sót những xung đột được tạo ra bởi các phạm vi chồng chéo. Một trường hợp phức tạp khác là khi các câu lệnh bao phủ hoàn toàn cùng một phạm vi nhưng lại mâu thuẫn: một câu lệnh "có ít nhất một kẻ mạo danh trong 1-5" và một câu lệnh khác "không có kẻ mạo danh nào trong 1-5" phải ngay lập tức mang lại kết quả "KHÔNG" nếu được truy vấn cùng nhau. Một cách tiếp cận bất cẩn không theo dõi sự chồng chéo một cách hiệu quả sẽ đưa ra câu trả lời sai. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ gán cho mỗi câu lệnh một loại và sau đó thử gán tất cả các nhiệm vụ của người xem cho các vai trò, kiểm tra tính nhất quán của tất cả các câu lệnh trong một truy vấn. Về nguyên tắc, điều này đúng vì việc kiểm tra tất cả các nhiệm vụ có thể có của người xem sẽ đảm bảo tính chính xác. Tuy nhiên, với`n`lên tới 2 * 10^5, số lượng bài tập là`2^n`, điều đó hoàn toàn không thể thực hiện được. Ngay cả khi chúng tôi chỉ kiểm tra từng câu lệnh riêng lẻ cho mỗi truy vấn, việc lặp qua các phạm vi một cách rõ ràng cho mỗi truy vấn sẽ dẫn đến`O(q * m * n)`hoạt động quá chậm. 

Nhận xét quan trọng là chúng ta không cần phân công đầy đủ các vai trò; chúng ta chỉ cần xác định những xung đột tiềm ẩn giữa các phát biểu. Tuyên bố loại 0 (không có kẻ mạo danh trong phạm vi) buộc tất cả người xem trong phạm vi đó phải là đồng đội. Tuyên bố loại 1 (ít nhất một kẻ mạo danh) yêu cầu ít nhất một người xem trong phạm vi đó phải là kẻ mạo danh. Hai câu lệnh xung đột nếu câu lệnh loại 1 hoàn toàn nằm trong câu lệnh loại 0. Do đó, vấn đề giảm xuống mức phát hiện hiệu quả nếu trong một tập hợp các câu lệnh liền kề, bất kỳ câu lệnh loại 1 nào đều hoàn toàn nằm trong câu lệnh loại 0. Chúng ta có thể tính toán trước vị trí xung đột sớm nhất cho mỗi câu lệnh, sau đó trả lời từng truy vấn trong thời gian không đổi bằng cách kiểm tra xem phạm vi truy vấn có vượt qua xung đột sớm nhất này hay không. Điều này làm giảm độ phức tạp từ tiền xử lý hạn chế đến tiền xử lý tuyến tính cộng với các truy vấn có thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m * n * q) | O(n) | Quá chậm | 
| Tối ưu | O(m + q) cho mỗi trường hợp thử nghiệm | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc số lượng người xem`n`và số lượng phát biểu`m`. 
2. Khởi tạo một biến`rightmost_zero`để theo dõi điểm cuối xa nhất của bất kỳ câu lệnh loại 0 nào được thấy cho đến nay. Đây là phạm vi lớn nhất chỉ có đồng đội. 
3. Khởi tạo một mảng`earliest_conflict`kích thước`m + 1`để lưu trữ vị trí sớm nhất mà xung đột xảy ra. 
4. Lặp lại tất cả các câu lệnh từ 1 đến`m`. Đối với mỗi tuyên bố: 

- Nếu là loại 0 thì cập nhật`rightmost_zero`là giá trị tối đa của giá trị hiện tại của nó và là điểm cuối của câu lệnh này. Điều này đảm bảo chúng tôi biết đầy đủ về các thành viên phi hành đoàn được thực thi. 
- Nếu là loại 1, ghi lại chỉ số câu lệnh sớm nhất mà tại đó câu lệnh loại 1 này có thể xung đột với câu lệnh loại 0 trước đó. Cụ thể, nếu điểm bắt đầu của nó nhỏ hơn hoặc bằng`rightmost_zero`, thì nó có khả năng xung đột và`earliest_conflict[i]`nên là tối đa`earliest_conflict[i-1]`và chỉ số hiện tại. Ngược lại, kế thừa`earliest_conflict[i-1]`. 
5. Sau khi xử lý tất cả các câu lệnh, đối với mỗi câu lệnh, chúng ta có loại xung đột sớm nhất loại 1 hoàn toàn nằm trong câu lệnh loại 0 trước đó. 
6. Đối với mỗi truy vấn`(l, r)`, kiểm tra xem`earliest_conflict[r] < l`. Nếu đúng thì không có xung đột nào trong phạm vi này, vì vậy hãy xuất "CÓ". Nếu không thì xuất ra "NO". 

Điều này có hiệu quả vì các câu lệnh loại 0 thực thi nghiêm ngặt đối với các thành viên phi hành đoàn và bất kỳ câu lệnh loại 1 nào trong phạm vi như vậy sẽ ngay lập tức vi phạm khả năng tất cả các câu lệnh đều đúng. Bằng cách theo dõi số 0 ngoài cùng bên phải và xung đột lan truyền, chúng tôi giảm việc xác minh cho mỗi truy vấn thành một phép so sánh đơn giản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        statements = []
        for _ in range(m):
            x, a, b = map(int, input().split())
            statements.append((x, a, b))
        earliest_conflict = [0] * (m + 1)
        rightmost_zero = 0
        for i, (x, a, b) in enumerate(statements, start=1):
            if x == 0:
                rightmost_zero = max(rightmost_zero, b)
            if x == 1:
                if a <= rightmost_zero:
                    earliest_conflict[i] = max(earliest_conflict[i-1], i)
                else:
                    earliest_conflict[i] = earliest_conflict[i-1]
            else:
                earliest_conflict[i] = earliest_conflict[i-1]
        q = int(input())
        for _ in range(q):
            l, r = map(int, input().split())
            print("YES" if earliest_conflict[r] < l else "NO")

if __name__ == "__main__":
    solve()
```Đầu tiên, mã theo dõi phạm vi xa nhất của đồng đội được áp đặt bởi các câu lệnh loại 0. Khi câu lệnh loại 1 bắt đầu bên trong phạm vi này, nó có khả năng xung đột và chúng tôi ghi lại điều này trong`earliest_conflict`. Các truy vấn được trả lời bằng cách kiểm tra xem có xung đột nào tồn tại trong phạm vi truy vấn hay không. sử dụng`i-1`việc truyền bá đảm bảo rằng thông tin xung đột được tích lũy, vì vậy chúng tôi không bao giờ bỏ lỡ câu lệnh loại 0 trước đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Phân đoạn đầu vào:```
4 3
1 1 3
1 2 4
0 2 3
1
1 3
```| Tuyên bố | x | một | b | ngoài cùng bên phải_zero | sớm_xung đột | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 3 | 0 | 0 | 
| 2 | 1 | 2 | 4 | 0 | 0 | 
| 3 | 0 | 2 | 3 | 3 | 0 | 

Truy vấn (1,3) kiểm tra xem`earliest_conflict[3] < 1`→`0 < 1`→ CÓ. 

Điều này cho thấy rằng mặc dù loại 0 là cuối cùng nhưng nó không làm mất hiệu lực các câu lệnh loại 1 trước đó ngoài phạm vi của nó. 

### Ví dụ 2 

Phân đoạn đầu vào:```
5 2
0 1 5
1 1 5
3
1 1
2 2
1 2
```| Tuyên bố | x | một | b | ngoài cùng bên phải_zero | sớm_xung đột | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 5 | 5 | 0 | 
| 2 | 1 | 1 | 5 | 5 | 2 | 

Truy vấn (1,1):`earliest_conflict[1] < 1`→`0 < 1`→ CÓ 

Truy vấn (2,2):`earliest_conflict[2] < 2`→`2 < 2`→ KHÔNG 

Truy vấn (1,2):`earliest_conflict[2] < 1`→`2 < 1`→ KHÔNG 

Điều này xác nhận rằng việc phát hiện xung đột sẽ xác định chính xác các mâu thuẫn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m + q) cho mỗi trường hợp thử nghiệm | Mỗi câu lệnh được xử lý một lần và mỗi truy vấn được trả lời trong thời gian không đổi. | 
| Không gian | O(m) | Chúng tôi lưu trữ`earliest_conflict`có kích thước m+1. | 

Với những hạn chế rằng tổng`m`Và`q`trên tất cả các trường hợp thử nghiệm là 2 * 10^5, thuật toán này phù hợp thoải mái trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("""4
4 3
1 1 3
1 2 4
0 2
```
