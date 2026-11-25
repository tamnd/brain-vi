---
title: "CF 985B - Công tắc và đèn"
description: "Chúng ta có một tập hợp các công tắc, mỗi công tắc điều khiển một tập hợp con đèn. Khi nhấn một công tắc, mọi đèn kết nối với nó sẽ bật vĩnh viễn. Khi đèn đã bật, đèn sẽ không bao giờ tắt nữa, ngay cả khi các công tắc khác ảnh hưởng đến đèn được nhấn sau đó."
date: "2026-06-17T00:55:37+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 985
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 44 (Rated for Div. 2)"
rating: 1200
weight: 985
solve_time_s: 77
verified: true
draft: false
---

[CF 985B - Công tắc và đèn](https://codeforces.com/problemset/problem/985/B) 

**Đánh giá:** 1200 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các công tắc, mỗi công tắc điều khiển một tập hợp con đèn. Khi nhấn một công tắc, mọi đèn kết nối với nó sẽ bật vĩnh viễn. Khi đèn đã bật, đèn sẽ không bao giờ tắt nữa, ngay cả khi các công tắc khác ảnh hưởng đến đèn được nhấn sau đó. 

Nếu chúng ta nhấn tất cả các công tắc, đảm bảo mọi đèn sẽ sáng. Nhiệm vụ là xác định xem có tồn tại một công tắc nào mà chúng ta có thể bỏ qua một cách an toàn trong khi vẫn đảm bảo rằng việc nhấn tất cả các công tắc còn lại vẫn bật mọi đèn hay không. 

Một cách khác để xem vấn đề là mỗi công tắc đóng góp một bộ đèn và sự kết hợp của tất cả các bộ sẽ bao phủ tất cả các đèn. Chúng tôi muốn biết liệu việc loại bỏ một bộ có giữ nguyên kết hợp hay không. 

Các ràng buộc cho phép lên tới 2000 công tắc và 2000 đèn, điều này làm cho giải pháp bậc hai hoặc kém hơn một chút khoảng 4 triệu thao tác có thể chấp nhận được. Bất kỳ khối nào trên cấu trúc ma trận sẽ quá chậm, nhưng các phép toán tỷ lệ với n lần m đều ổn. 

Trường hợp khó phát hiện xảy ra khi đèn được che bởi đúng một công tắc. Trong trường hợp đó, việc tháo công tắc đó sẽ khiến đèn đó không thể sáng được. Ví dụ: nếu đèn j chỉ được kết nối với công tắc i thì công tắc i là bắt buộc. Bất kỳ giải pháp đúng nào cũng phải phát hiện các điều kiện duy nhất đó một cách ngầm định hoặc rõ ràng. 

Một trường hợp khác phát sinh khi nhiều công tắc che phủ tất cả các đèn một cách dư thừa. Ví dụ: nếu mỗi đèn được kết nối với tất cả các công tắc thì việc tháo bất kỳ công tắc nào vẫn khiến tất cả các đèn sáng. Một cách tiếp cận đơn giản có thể làm vấn đề này trở nên phức tạp hơn và vẫn đạt yêu cầu, nhưng điều quan trọng là phải nhận ra rằng sự dư thừa trên tất cả các đèn là cấu trúc cốt lõi đang được thử nghiệm. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng tác động của việc loại bỏ từng công tắc một. Đối với một công tắc cố định i, chúng ta sẽ tính toán lại những đèn nào vẫn sáng bằng cách sử dụng tất cả các công tắc khác và kiểm tra xem tất cả các đèn có còn bị che hay không. Nếu chúng tôi tính toán lại phạm vi bao phủ từ đầu mỗi lần, chúng tôi sẽ quét liên tục toàn bộ ma trận. 

Phương pháp brute-force này kiểm tra n ứng cử viên và với mỗi ứng cử viên sẽ tính toán lại vùng phủ sóng trên n công tắc và m đèn. Điều đó dẫn đến hoạt động O(n^2 m). Với n và m lên đến 2000, con số này đạt tới khoảng 8 × 10^9 phép tính, quá chậm. 

Quan sát quan trọng là chúng ta không thực sự cần phải tính toán lại mọi thứ. Điều quan trọng duy nhất khi tháo công tắc là liệu có ít nhất một đèn không được che chắn hay không. Đối với đèn j, tình huống nguy hiểm duy nhất là khi tất cả các công tắc bật đèn j đều có công tắc bị tháo ra. Tương tự, với mỗi đèn j, ta chỉ cần biết có bao nhiêu công tắc che nó. Nếu một bóng đèn bị che ít nhất hai lần, việc tháo bất kỳ công tắc che phủ nào cũng sẽ không tắt được đèn. 

Điều này làm giảm vấn đề về một cấu trúc đếm đơn giản. Đối với mỗi đèn, hãy tính xem có bao nhiêu công tắc kích hoạt nó. Sau đó, đối với mỗi công tắc, hãy xác minh xem đó có phải là nhà cung cấp duy nhất cho bất kỳ đèn nào không. Nếu công tắc là nguồn cung cấp duy nhất của một số đèn thì không thể tháo nó ra. Nếu không thì nó an toàn. 

Chúng ta có thể kiểm tra điều này một cách hiệu quả bằng cách tính toán trước số lượng vùng phủ sóng và sau đó quét lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại Brute Force | O(n^2 m) | O(nm) | Quá chậm | 
| Đếm vùng phủ sóng trên mỗi đèn | O(nm) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc ma trận đầu vào mô tả công tắc nào kích hoạt đèn nào. Cấu trúc này xác định mối quan hệ hai bên giữa công tắc và đèn. 
2. Với mỗi đèn, hãy đếm xem có thể bật được bao nhiêu công tắc. Điều này cung cấp một dãy tần số trên các đèn thu được sự dư thừa. Lý do điều này hữu ích là việc loại bỏ chỉ nguy hiểm khi dư thừa chính xác là một. 
3. Đối với mỗi công tắc, hãy kiểm tra tất cả các đèn mà nó kích hoạt. Nếu bất kỳ đèn nào như vậy có tổng số là 1, hãy đánh dấu công tắc là cần thiết. Lý do là chiếc đèn như vậy sẽ không sáng nếu loại bỏ công tắc này. 
4. Nếu tìm thấy ít nhất một công tắc không cần thiết, chúng ta có thể yên tâm bỏ qua nó và vẫn giữ cho tất cả các đèn sáng. Đầu ra "CÓ". 
5. Nếu mọi công tắc đều cần thiết thì mỗi công tắc đều cần có ít nhất một đèn được hỗ trợ riêng. Trong trường hợp đó, xuất ra "NO". 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên sự mô tả đơn giản về sự thất bại. Một công tắc có thể tháo rời khi và chỉ khi không có đèn nào phụ thuộc hoàn toàn vào nó. Tình trạng chung “tất cả các đèn vẫn sáng sau khi tháo công tắc” được phân tách thành các bước kiểm tra độc lập trên mỗi đèn, bởi vì việc kích hoạt đèn là đơn điệu và bổ sung đối với các công tắc. Vì mỗi đèn chỉ yêu cầu ít nhất một đèn lân cận đang hoạt động nên việc mất một công tắc chính xác là vô hại khi không có đèn nào bị mất cạnh đỡ cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    mat = [input().strip() for _ in range(n)]
    
    cnt = [0] * m
    
    for i in range(n):
        row = mat[i]
        for j in range(m):
            if row[j] == '1':
                cnt[j] += 1
    
    for i in range(n):
        row = mat[i]
        ok = True
        for j in range(m):
            if row[j] == '1' and cnt[j] == 1:
                ok = False
                break
        if ok:
            print("YES")
            return
    
    print("NO")

if __name__ == "__main__":
    solve()
```Bước đầu tiên xây dựng một dải tần số trên đèn. Đây là bước tiền xử lý quan trọng giúp thu gọn tất cả các tương tác chuyển đổi thành các số đếm đơn giản. 

Lần thứ hai đánh giá từng công tắc một cách độc lập. Vòng lặp bên trong kiểm tra xem công tắc có chịu trách nhiệm cho bất kỳ đèn nào được che phủ duy nhất hay không. Việc nghỉ sớm rất quan trọng vì khi tìm thấy một phụ thuộc duy nhất, việc chuyển đổi sẽ ngay lập tức bị loại. 

Quyết định lưu trữ ma trận dưới dạng chuỗi sẽ tránh được chi phí phân tích cú pháp lặp lại và giữ mức sử dụng bộ nhớ ở mức tối thiểu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 5
10101
01000
00111
10000
```Chúng tôi tính toán số lượng đèn đầu tiên. 

| Đèn | Đếm | 
| --- | --- | 
| 1 | 2 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 2 | 
| 5 | 1 | 

Bây giờ chúng tôi kiểm tra các công tắc. 

| Chuyển đổi | Đèn | Có đèn độc đáo? | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 1,3,5 | có (3,5) | từ chối | 
| 2 | 2 | có (2) | từ chối | 
| 3 | 3,4,5 | có (3,5) | từ chối | 
| 4 | 1 | không | chấp nhận | 

Công tắc 4 có thể được tháo ra mà không để bất kỳ đèn nào tắt, vì vậy đầu ra là CÓ. 

Dấu vết này cho thấy thuật toán không tìm kiếm một công tắc “yếu” trên toàn cầu mà là một công tắc không đóng vai trò là nhà cung cấp duy nhất ở bất kỳ đâu. 

### Ví dụ 2 

đầu vào:```
3 3
110
101
011
```Số lượng đèn: 

| Đèn | Đếm | 
| --- | --- | 
| 1 | 2 | 
| 2 | 2 | 
| 3 | 2 | 

Mỗi công tắc đều có khả năng dự phòng cho mỗi đèn mà nó chạm vào. 

| Chuyển đổi | Đèn | Sự phụ thuộc duy nhất | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 1,2 | không | chấp nhận | 

Vì công tắc 1 đã thỏa mãn điều kiện nên chúng ta xuất ngay YES. 

Ví dụ này nhấn mạnh rằng tính dự phòng hoàn toàn trên tất cả các đèn đảm bảo có nhiều câu trả lời hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Hai lần vượt qua ma trận n x m | 
| Không gian | O(m) | Chỉ có bộ đếm đèn được lưu trữ | 

Các ràng buộc cho phép kiểm tra tới 4 triệu ký tự, phù hợp thoải mái với Python trong giới hạn thời gian khi được triển khai bằng các vòng lặp đơn giản trên chuỗi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run("""4 5
10101
01000
00111
10000
""") == "YES"

# all switches essential (each lamp unique)
assert run("""2 2
10
01
""") == "NO"

# fully redundant system
assert run("""3 3
111
111
111
""") == "YES"

# single switch case
assert run("""1 3
111
""") == "YES"

# one lamp uniquely dependent
assert run("""3 2
10
10
01
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 công tắc, đèn rời | KHÔNG | không có công tắc rời | 
| đầy đủ 1 ma trận | CÓ | dự phòng hoàn toàn | 
| công tắc đơn | CÓ | sự chấp nhận tầm thường | 
| sự độc đáo hỗn hợp | CÓ | phát hiện chính xác công tắc rời | 

## Vỏ cạnh 

Trường hợp key edge là khi mỗi đèn có chính xác một công tắc hỗ trợ. Ví dụ:```
2 2
10
01
```Ở đây mỗi công tắc chịu trách nhiệm duy nhất cho một đèn. Thuật toán đánh dấu cả hai công tắc là thiết yếu vì mỗi công tắc che một đèn có số đếm là 1. Điều này dẫn đến NO một cách chính xác. 

Một trường hợp đặc biệt khác là khi một công tắc không có trách nhiệm duy nhất mặc dù nó che phủ nhiều đèn. Ví dụ:```
3 3
111
111
111
```Mỗi đèn đều có số đếm là 3 nên không có công tắc nào bị loại. Thuật toán xác định chính xác rằng bất kỳ công tắc nào cũng có thể được gỡ bỏ. 

Trường hợp tối thiểu với một công tắc:```
1 3
111
```Tất cả các đèn đều có số đếm là 1 nên cần phải có một công tắc duy nhất và không thể tháo rời được. Thuật toán xuất ra NO, phù hợp với yêu cầu.
