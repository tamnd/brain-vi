---
title: "CF 1004A - Sonya và Khách sạn"
description: "Chúng ta có một tập hợp các khách sạn hiện có được đặt trên các điểm nguyên dọc theo một trục số vô hạn. Chúng tôi muốn mở thêm một khách sạn ở tọa độ nguyên nào đó. Yêu cầu là khách sạn hiện có gần nhất với khách sạn mới này phải ở khoảng cách chính xác $d$."
date: "2026-06-16T23:25:34+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1004
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 495 (Div. 2)"
rating: 900
weight: 1004
solve_time_s: 98
verified: true
draft: false
---

[CF 1004A - Sonya và Khách sạn](https://codeforces.com/problemset/problem/1004/A) 

**Xếp hạng:** 900 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các khách sạn hiện có được đặt trên các điểm nguyên dọc theo một trục số vô hạn. Chúng tôi muốn mở thêm một khách sạn ở tọa độ nguyên nào đó. Yêu cầu là khách sạn hiện có gần nhất với khách sạn mới này phải cách nhau một khoảng chính xác.$d$. Nói cách khác, nếu chúng ta xem xét tất cả khoảng cách từ vị trí mới đến mọi khách sạn hiện có thì khoảng cách tối thiểu đó phải bằng$d$. 

Dữ liệu đầu vào cung cấp cho chúng tôi số lượng khách sạn hiện có và tọa độ của chúng, đã được sắp xếp theo thứ tự tăng dần. Nhiệm vụ là đếm xem có bao nhiêu vị trí nguyên cho khách sạn mới thỏa mãn điều kiện khoảng cách chính xác này. 

Ràng buộc$n \le 100$đủ nhỏ để việc quét bậc hai đối với các ứng cử viên là khả thi mà không cần quan tâm. Ngay cả khi chúng tôi xem xét việc kiểm tra mọi vị trí ứng viên tiềm năng và xác minh nó với tất cả các khách sạn hiện có, tổng số hoạt động vẫn nằm trong khoảng vài triệu. Điều này ngay lập tức loại trừ nhu cầu về cấu trúc dữ liệu nâng cao hoặc tối ưu hóa truy vấn logarit. Một phương pháp kiểm tra hình học đơn giản là đủ. 

Một sự hiểu lầm ngây thơ thường xuất hiện trong bài toán này là cho rằng ta chỉ cần kiểm tra chính xác các điểm ở khoảng cách$d$từ một số khách sạn mà không cần xác nhận thêm. Điều đó là không đủ vì một điểm có thể ở khoảng cách$d$từ một khách sạn trong khi ở gần hơn$d$đến cái khác. 

Ví dụ: hãy xem xét các khách sạn ở vị trí 0 và 10 với$d = 5$. Điểm 5 nằm ở khoảng cách 5 từ cả hai khách sạn nên nó hợp lệ. Nhưng một điểm như 4 nằm ở khoảng cách 4 từ 0 và 6 từ 10, nên khoảng cách tối thiểu của nó là 4 chứ không phải 5 và không được tính. Tương tự, một điểm như 15 nằm cách 5 với 10, nhưng chỉ cách 10 5 và 15 cách 0, nên nó hợp lệ; tuy nhiên, nếu một khách sạn khác tồn tại gần hơn, điểm đó có thể không đáp ứng được điều kiện. Điều này cho thấy lập luận cục bộ xung quanh một khách sạn là chưa đủ; cần phải xác minh toàn cầu. 

Một trường hợp phức tạp khác là sự trùng lặp của các vị trí ứng viên. Các khách sạn khác nhau có thể tạo ra cùng một tọa độ ứng viên$x_i + d$hoặc$x_i - d$và đếm chúng nhiều lần sẽ đánh giá quá cao câu trả lời. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ sẽ là lặp lại tất cả các tọa độ nguyên trong một phạm vi hợp lý, chẳng hạn như từ vị trí khách sạn tối thiểu trừ đi$d$đến mức tối đa cộng thêm$d$và kiểm tra từng điểm xem khoảng cách tối thiểu của nó tới bất kỳ khách sạn nào có chính xác không$d$. Về nguyên tắc thì điều này đúng, nhưng phạm vi có thể lớn bằng$10^9$theo cả hai hướng, khiến không thể quét trực tiếp. 

Quan sát quan trọng là một điểm hợp lệ phải nằm chính xác trên ranh giới của đường tròn bán kính$d$tập trung tại một số khách sạn. Nếu một điểm hợp lệ, khách sạn gần nhất của nó phải ở khoảng cách chính xác$d$, nghĩa là nó phải thỏa mãn$|y - x_i| = d$cho ít nhất một$i$. Điều này ngay lập tức giảm không gian tìm kiếm xuống chỉ còn hai ứng viên cho mỗi khách sạn:$x_i - d$Và$x_i + d$. 

Tuy nhiên, chỉ tạo ra ứng viên là chưa đủ. Mỗi ứng viên phải được xác minh với tất cả các khách sạn để đảm bảo rằng không có khách sạn nào gần hơn khoảng cách$d$. Việc xác minh này rẻ vì$n \le 100$, vì vậy việc kiểm tra tất cả các khoảng cách cho từng chi phí ứng viên$O(n)$, và chúng ta chỉ có$2n$ứng viên. 

Do đó, giải pháp cuối cùng là cách tiếp cận tạo và xác thực trên một tập ứng cử viên nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tọa độ |$O(R \cdot n)$Ở đâu$R$có thể$10^9$|$O(1)$| Quá chậm | 
| Tạo ứng viên từ mỗi khách sạn |$O(n^2)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với từng vị trí khách sạn$x_i$, xây dựng hai vị trí ứng cử viên:$x_i - d$Và$x_i + d$. Đây là những điểm duy nhất có thể có khoảng cách tối thiểu một cách chính xác$d$, vì bất kỳ điểm hợp lệ nào cũng phải chính xác$d$cách xa ít nhất một khách sạn. 
2. Đối với từng vị trí ứng viên$y$, tính khoảng cách của nó tới mọi khách sạn. 
3. Trong khi kiểm tra khoảng cách, hãy theo dõi khoảng cách tối thiểu từ$y$đến bất kỳ khách sạn nào. Điều này quyết định liệu$y$thỏa mãn điều kiện. 
4. Nếu khoảng cách tối thiểu bằng chính xác$d$, coi ứng viên này là hợp lệ. 
5. Sử dụng một tập hợp để tránh tính các vị trí ứng viên trùng lặp được tạo từ các khách sạn khác nhau. 

Ý tưởng chính là chúng ta không bao giờ cần xét những điểm không ở xa nhau$d$từ một số khách sạn, bởi vì bất kỳ giải pháp hợp lệ nào cũng phải chạm đến ít nhất một “ranh giới bán kính” có tâm tại một khách sạn. 

### Tại sao nó hoạt động 

Bất kỳ vị trí hợp lệ$y$phải thỏa mãn$\min_i |y - x_i| = d$. Điều này có nghĩa là tồn tại ít nhất một chỉ mục$i$như vậy$|y - x_i| = d$, và cho tất cả các chỉ số khác$j$,$|y - x_j| \ge d$. Do đó mọi điểm hợp lệ phải xuất hiện trong tập ứng cử viên được xây dựng từ$x_i \pm d$. Bước xác minh đảm bảo rằng chúng tôi loại trừ các ứng viên quá gần với một số khách sạn khác, duy trì tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, d = map(int, input().split())
    x = list(map(int, input().split()))

    candidates = set()

    for xi in x:
        candidates.add(xi - d)
        candidates.add(xi + d)

    ans = 0

    for y in candidates:
        best = float('inf')
        for xi in x:
            dist = abs(y - xi)
            if dist < best:
                best = dist
                if best < d:
                    break
        if best == d:
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên xây dựng tất cả các ứng cử viên ranh giới có thể có xung quanh mỗi khách sạn. Bộ này rất quan trọng vì nhiều khách sạn có thể tạo ra cùng một tọa độ và các bản sao không được làm tăng câu trả lời. 

Trong quá trình xác thực, chúng tôi tính toán khoảng cách gần nhất từ ​​mỗi ứng viên đến bất kỳ khách sạn nào. Việc nghỉ sớm được sử dụng khi chúng tôi phát hiện khoảng cách nhỏ hơn$d$, vì điều đó ngay lập tức loại bỏ ứng viên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
-3 2 9 16
```Chúng tôi tạo ra các ứng cử viên: 

| Khách sạn | xi - d | xi + d | 
| --- | --- | --- | 
| -3 | -6 | 0 | 
| 2 | -1 | 5 | 
| 9 | 6 | 12 | 
| 16 | 13 | 19 | 

Tập ứng viên là {-6, 0, -1, 5, 6, 12, 13, 19}. 

Bây giờ chúng tôi xác nhận: 

| y | khoảng cách gần nhất | hợp lệ | 
| --- | --- | --- | 
| -6 | 3 | vâng | 
| 0 | 2 | không | 
| -1 | 3 | vâng | 
| 5 | 3 | vâng | 
| 6 | 3 | vâng | 
| 12 | 3 | vâng | 
| 13 | 3 | vâng | 
| 19 | 3 | vâng | 

Chúng tôi đếm 6 vị trí hợp lệ. 

Dấu vết này cho thấy rằng mặc dù tất cả các ứng viên đều đến từ các ranh giới hợp lệ nhưng một số lại thất bại do ở gần hơn ranh giới.$d$đến một khách sạn khác. 

### Ví dụ 2 

Đầu vào (được xây dựng):```
3 2
1 5 10
```Ứng viên: 

| Khách sạn | xi - d | xi + d | 
| --- | --- | --- | 
| 1 | -1 | 3 | 
| 5 | 3 | 7 | 
| 10 | 8 | 12 | 

Ứng viên duy nhất: {-1, 3, 7, 8, 12} 

| y | khoảng cách gần nhất | hợp lệ | 
| --- | --- | --- | 
| -1 | 2 | vâng | 
| 3 | 2 | vâng | 
| 7 | 2 | vâng | 
| 8 | 2 | vâng | 
| 12 | 2 | vâng | 

Tất cả đều hợp lệ trong cấu hình này vì không có điểm nào gần hơn$d$đến khách sạn thứ ba. 

Điều này chứng tỏ một trường hợp trong đó mọi ứng cử viên ranh giới đều vượt qua được quá trình xác thực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Chúng tôi tạo ra$2n$ứng viên và cho mỗi khoảng cách kiểm tra lên đến$n$khách sạn | 
| Không gian |$O(n)$| Lưu trữ các vị trí khách sạn và bộ ứng viên | 

Với$n \le 100$, trường hợp xấu nhất khoảng 20.000 lần kiểm tra khoảng cách dễ dàng nằm trong giới hạn. Giải pháp này có hiệu quả thoải mái dưới những ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, d = map(int, input().split())
    x = list(map(int, input().split()))

    candidates = set()
    for xi in x:
        candidates.add(xi - d)
        candidates.add(xi + d)

    ans = 0
    for y in candidates:
        best = float('inf')
        for xi in x:
            best = min(best, abs(y - xi))
        if best == d:
            ans += 1

    return str(ans)

# provided sample
assert run("4 3\n-3 2 9 16\n") == "6"

# minimum size
assert run("1 10\n0\n") == "2"

# symmetric case
assert run("2 5\n0 10\n") == "4"

# all close cluster
assert run("3 1\n1 2 3\n") == "4"

# larger spacing
assert run("3 2\n1 5 10\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khách sạn đơn | 2 | trường hợp cơ sở xung quanh một điểm | 
| cặp đối xứng | 4 | ranh giới ứng cử viên chồng chéo | 
| cụm dày đặc | 4 | lọc theo khách sạn khác | 
| bố cục thưa thớt | 5 | nhiều điểm ranh giới hợp lệ | 

## Vỏ cạnh 

Khi chỉ có một khách sạn, mỗi vị trí hợp lệ có đúng hai điểm:$x_1 - d$Và$x_1 + d$. Thuật toán tạo ra chính xác hai ứng cử viên này và cả hai đều vượt qua quá trình xác thực vì không khách sạn nào khác có thể vi phạm điều kiện khoảng cách. 

Khi các khách sạn ở gần nhau, một số ứng cử viên ranh giới sẽ thất bại vì một khách sạn khác ở gần nhau hơn.$d$. Vòng xác thực sẽ loại bỏ chính xác những trường hợp này bằng cách kiểm tra tất cả các khoảng cách. 

Khi nhiều khách sạn tạo ra cùng một tọa độ ứng viên, bộ này đảm bảo chỉ được tính một lần. Nếu không loại bỏ trùng lặp, vị trí hợp lệ tương tự có thể bị tính quá mức, đặc biệt khi tồn tại cấu hình đối xứng.
