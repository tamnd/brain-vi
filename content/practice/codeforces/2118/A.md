---
title: "CF 2118A - Dãy số bằng nhau"
description: "Chúng ta được yêu cầu xây dựng một chuỗi nhị phân có độ dài $n$, chứa chính xác $k$ đơn vị, với một ràng buộc cấu trúc bổ sung liên quan đến các chuỗi con có độ dài ba. Trong số tất cả các bộ ba chỉ số $i < j < k$, chúng ta đếm có bao nhiêu dạng mẫu “101” và bao nhiêu dạng “010”."
date: "2026-06-08T04:00:15+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2118
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1030 (Div. 2)"
rating: 800
weight: 2118
solve_time_s: 87
verified: false
draft: false
---

[CF 2118A - Dãy số bằng nhau](https://codeforces.com/problemset/problem/2118/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xây dựng một chuỗi nhị phân có độ dài$n$, chứa chính xác$k$những cái đó, với một ràng buộc cấu trúc bổ sung liên quan đến các chuỗi con có độ dài ba. Trong số tất cả các bộ ba chỉ số$i < j < k$, chúng ta đếm có bao nhiêu mẫu “101” và bao nhiêu mẫu “010”. Mục tiêu là tạo ra một chuỗi trong đó hai số này bằng nhau. 

Dãy con ở đây có nghĩa là chúng ta có thể tự do bỏ qua các ký tự, vì vậy điều quan trọng không phải là sự liền kề mà là thứ tự tương đối. Điều này làm cho điều kiện nhạy cảm với vị trí tổng thể của số 0 và số 1 thay vì các mẫu cục bộ. 

Các hạn chế là nhỏ:$n \le 100$Và$t \le 500$. Điều này ngay lập tức cho chúng ta biết rằng chúng ta không cần bất cứ thứ gì chặt chẽ tiệm cận ngoài$O(n^2)$cho mỗi trường hợp thử nghiệm và thậm chí lý luận bậc ba cũng được chấp nhận để hiểu. Tuy nhiên, vấn đề xây dựng gợi ý một mô hình cấu trúc hoặc tham lam hơn là tính toán một cách rõ ràng. 

Trường hợp cạnh xuất hiện khi$k = 0$,$k = n$, hoặc khi$k = 1$hoặc$k = n-1$. Trong những trường hợp này, một trong các mẫu là không thể vì ít nhất một trong số “101” hoặc “010” yêu cầu cả hai ký hiệu xuất hiện hai lần ở các vị trí xen kẽ. Ví dụ: nếu tất cả các ký tự giống hệt nhau thì cả hai số đếm đều bằng 0, do đó mọi chuỗi thống nhất đều hợp lệ. Một cách tiếp cận bất cẩn có thể cố gắng “cân bằng” các chuỗi con bằng cách xen kẽ quá mạnh mẽ, nhưng điều đó là không cần thiết và có thể vi phạm số lượng yêu cầu. 

Một trường hợp khó phát hiện khác là khi các cạnh rất thưa thớt hoặc rất dày đặc. Ví dụ, nếu$k = 1$, bất kỳ chuỗi nào có số 1 duy nhất đều không xuất hiện cả hai mẫu, vì vậy mọi vị trí đều có tác dụng. Tương tự cho$k = n-1$. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là tạo ra tất cả các chuỗi nhị phân có độ dài$n$với chính xác$k$những cái một, và với mỗi cái, hãy tính số dãy con “101” và “010” bằng cách lặp lại tất cả các bộ ba chỉ số. Đếm các dãy con trực tiếp$O(n^3)$, và có$\binom{n}{k}$chuỗi ứng cử viên. Ngay cả đối với$n = 100$, điều này rất lớn về mặt thiên văn, khiến cho việc sử dụng vũ lực hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là vấn đề không thực sự yêu cầu kiểm soát sự khác biệt chính xác giữa hai số đếm. Chúng ta chỉ cần họ bình đẳng. Một cách đơn giản hóa cấu trúc mạnh mẽ là chúng ta có thể buộc cả hai số đếm bằng 0. Điều này xảy ra nếu chúng ta loại bỏ hoàn toàn tất cả các mẫu xen kẽ có độ dài bằng ba. 

Một chuỗi nhị phân không có dãy con “101” hoặc “010” khi và chỉ khi nó không bao giờ luân phiên hai lần giữa 0 và 1 theo thứ tự. Điều này được đảm bảo nếu chuỗi đơn điệu trong các khối: tất cả các số 0 theo sau là tất cả các số 1 hoặc tất cả các số 1 theo sau là tất cả các số 0. Trong một chuỗi như vậy, bất kỳ bộ ba nào chỉ chứa một loại ký hiệu hoặc có nhiều nhất một chuyển đổi, do đó không mẫu nào có thể xuất hiện. 

Điều này làm giảm nhiệm vụ thành một vấn đề xây dựng thuần túy: đặt$k$những cái và$n-k$số không trong một cấu trúc khối liền kề duy nhất. Cả “000…111…” và “111…000…” đều hoạt động và cả hai đều đáp ứng sự bình đẳng cần thiết vì cả hai số đếm tiếp theo đều bằng 0. 

Đây là mức giảm tới hạn: thay vì cân bằng hai số tổ hợp, chúng ta tránh cả hai cùng một lúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê và đếm Brute Force |$O(\binom{n}{k} \cdot n^3)$|$O(n)$| Quá chậm | 
| Xây dựng khối đơn điệu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$Và$k$. Chúng xác định số lượng chuỗi phải xuất hiện trong chuỗi cuối cùng và tổng chiều dài của nó. 
2. Xây dựng một chuỗi gồm$k$ký tự '1' liên tiếp. Điều này đảm bảo ràng buộc đếm được thỏa mãn ngay lập tức mà không cần chỉnh sửa sau này. 
3. Nối thêm$n-k$ký tự '0' liên tiếp. Điều này tạo ra một ranh giới chuyển tiếp duy nhất giữa các ký hiệu. 
4. Xuất chuỗi kết quả. 

Việc lựa chọn đặt mọi thứ lên hàng đầu là tùy ý. Việc đặt tất cả các số 0 lên đầu tiên có giá trị như nhau vì điều kiện chỉ phụ thuộc vào các dãy con có dạng xen kẽ chứ không phụ thuộc vào thứ tự tuyệt đối. 

### Tại sao nó hoạt động 

Chuỗi được xây dựng có nhiều nhất một chuyển tiếp giữa các ký hiệu. Bất kỳ chuỗi con nào có độ dài ba xen kẽ giữa 0 và 1 sẽ yêu cầu ít nhất hai lần chuyển đổi theo thứ tự, 0→1→0 hoặc 1→0→1. Vì chỉ có một quá trình chuyển đổi tồn tại trong toàn bộ chuỗi nên không thể hình thành mẫu nào. Do đó cả hai số đều bằng 0 và chúng bằng nhau. 

Việc xây dựng cũng thực thi một cách tầm thường số lượng đơn vị chính xác, vì chúng tôi đặt chúng một cách rõ ràng mà không cần sắp xếp lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    # place all 1s first, then 0s
    print("1" * k + "0" * (n - k))
```Mã trực tiếp thực hiện việc xây dựng khối đơn điệu. Điểm tinh tế duy nhất là đảm bảo số lượng khớp chính xác: độ dài chuỗi là cố định, vì vậy một khi$k$những vị trí được đặt thì các vị trí còn lại phải bằng 0. 

Không cần xác nhận hoặc tính toán thêm vì cấu trúc tự động đảm bảo điều kiện tuần tự. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$n = 4, k = 2$Chúng tôi xây dựng: 

| Bước | Trạng thái chuỗi | 
| --- | --- | 
| Bắt đầu | "" | 
| Nơi 1s | "11" | 
| Đặt số 0 | "1100" | 

Chuỗi kết quả chứa chính xác hai chuỗi và có một quá trình chuyển đổi duy nhất từ ​​1 sang 0, do đó không tồn tại chuỗi con xen kẽ có độ dài ba. Do đó số lượng “101” và “010” đều bằng 0. 

### Ví dụ 2 

đầu vào:$n = 5, k = 3$| Bước | Trạng thái chuỗi | 
| --- | --- | 
| Bắt đầu | "" | 
| Nơi 1s | "111" | 
| Đặt số 0 | "11100" | 

Điều này một lần nữa chỉ tạo ra một ranh giới giữa các ký hiệu. Bất kỳ bộ ba nào đều nằm hoàn toàn bên trong một khối hoặc vượt qua ranh giới một lần, không bao giờ hai lần, do đó không xuất hiện dãy con bị cấm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Chúng ta xây dựng một chuỗi có độ dài$n$trực tiếp | 
| Không gian |$O(1)$thêm | Chỉ chuỗi đầu ra được lưu trữ | 

Tổng số công việc trên tất cả các trường hợp thử nghiệm nhiều nhất là$500 \cdot 100$, không đáng kể trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        n, k = map(int, sys.stdin.readline().split())
        out.append("1" * k + "0" * (n - k))
    return "\n".join(out) + "\n"

# provided samples
assert run("""5
4 2
5 3
5 5
6 2
1 1
""") == """1100
11100
11111
110000
1
"""

# custom cases
assert run("""3
1 0
1 1
2 1
""") == """0
1
10
"""

assert run("""2
6 0
6 6
""") == """000000
111111
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp phần tử đơn | 0 / 1 | độ đúng ranh giới tối thiểu | 
| tất cả số không / tất cả những cái | dây thống nhất | độ chính xác mật độ cực cao | 
| trộn nhỏ n | 10 | cấu trúc không tầm thường đơn giản nhất | 

## Vỏ cạnh 

Khi nào$k = 0$, đầu ra của thuật toán$n$số không. Không có chuỗi nào nên cả hai dãy con “101” và “010” đều không thể xảy ra. Việc xây dựng tạo ra một khối trống theo sau là các số 0, vẫn thỏa mãn bất biến của nhiều nhất một chuyển đổi. 

Khi$k = n$, đầu ra là$n$những cái đó. Một lần nữa, không có số 0 nào tồn tại nên cả hai số dãy con đều bằng 0. Cấu trúc thoái hóa thành một khối duy nhất không có chuyển tiếp. 

Khi$k = 1$, chuỗi là “1000…0”. Bất kỳ dãy con nào có độ dài ba đều không thể luân phiên vì chỉ có một lần xuất hiện của số 1, nên cả hai mẫu đều vắng mặt. Lý do tương tự áp dụng đối xứng cho$k = n-1$.
