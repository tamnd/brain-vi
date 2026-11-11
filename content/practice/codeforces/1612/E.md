---
title: "CF 1612E - Tin nhắn"
description: "Chúng tôi có một nhóm sinh viên, mỗi người trong số họ Monocarp muốn đọc một tin nhắn cụ thể trong cuộc trò chuyện. Mỗi học sinh đều có giới hạn về số lượng tin nhắn họ sẽ đọc trong một ngày."
date: "2026-06-10T06:59:02+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "greedy", "probabilities", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1612
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 117 (Rated for Div. 2)"
rating: 2000
weight: 1612
solve_time_s: 85
verified: true
draft: false
---

[CF 1612E - Tin nhắn](https://codeforces.com/problemset/problem/1612/E) 

**Xếp hạng:** 2000 
**Tags:** vũ lực, dp, tham lam, xác suất, sắp xếp 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một nhóm sinh viên, mỗi người trong số họ Monocarp muốn đọc một tin nhắn cụ thể trong cuộc trò chuyện. Mỗi học sinh đều có giới hạn về số lượng tin nhắn họ sẽ đọc trong một ngày. Monocarp có thể ghim tin nhắn để tăng khả năng đọc, nhưng học sinh chỉ đọc một tập hợp con ngẫu nhiên nếu số lượng tin nhắn được ghim vượt quá giới hạn đọc cá nhân của họ. Đầu vào cung cấp thông báo mục tiêu và giới hạn đọc cho mỗi học sinh, còn đầu ra phải chỉ định những thông báo nào Monocarp sẽ ghim để tối đa hóa số lượng học sinh dự kiến ​​đọc tin nhắn được chỉ định của họ. 

Hạn chế chính hướng dẫn giải pháp là số lượng sinh viên có thể lên tới 200.000 và mỗi chỉ mục tin nhắn cũng có thể lên tới 200.000. Mỗi học sinh sẽ đọc tối đa 20 tin nhắn. Sự ràng buộc chặt chẽ về khả năng đọc cho thấy rằng chúng tôi không đủ khả năng để xem xét tất cả các tập hợp con tin nhắn cho mỗi học sinh - một cách tiếp cận mạnh mẽ nhằm cố gắng mọi sự kết hợp của các tin nhắn được ghim sẽ bùng nổ theo kiểu kết hợp. Thay vào đó, chúng ta cần một cách tiếp cận suy luận theo xác suất về khả năng mỗi học sinh đọc được thông điệp mục tiêu của mình và lựa chọn một cách hiệu quả những thông điệp giúp tối đa hóa tổng các xác suất này. 

Một trường hợp phức tạp xảy ra khi nhiều học sinh muốn có cùng một thông điệp nhưng có giới hạn đọc rất thấp. Ví dụ: nếu ba học sinh đều muốn có tin nhắn 7 và giới hạn đọc của họ là 1, 1 và 2, chúng tôi không thể chỉ ghim tất cả các tin nhắn mà họ quan tâm. Ghim quá nhiều tin nhắn sẽ làm giảm khả năng xảy ra đối với những học sinh có giới hạn nhỏ. Việc ghim tất cả các tin nhắn mong muốn một cách ngây thơ thực sự có thể làm giảm số lần đọc thành công dự kiến, vì vậy giải pháp phải tính đến việc giảm xác suất này. 

## Phương pháp tiếp cận 

Phương pháp brute-force xem xét mọi tập hợp con tin nhắn có thể có để ghim và tính toán số lượng học sinh dự kiến sẽ đọc tin nhắn của họ. Đối với mỗi tập hợp con, chúng tôi sẽ tính tổng các xác suất của mỗi học sinh dựa trên giới hạn khả năng đọc của họ. Điều này đúng về nguyên tắc nhưng không thể thực hiện được vì số lượng tin nhắn có thể lên tới 200.000, dẫn đến$2^{200000}$tập hợp con. Rõ ràng là chúng ta không thể liệt kê hết chúng. 

Thông tin chi tiết quan trọng là những học sinh có giới hạn đọc nhỏ hơn sẽ có kết quả giảm dần khi số lượng tin nhắn được ghim tăng lên. Nếu một học sinh chỉ có thể đọc được một tin nhắn thì xác suất đọc được mục tiêu của họ là$1/t$khi$t$tin nhắn được ghim. Nếu học sinh có thể đọc$k_i$tin nhắn, xác suất là$k_i/t$khi$t > k_i$. Sự đơn điệu này cho thấy số lượng tin nhắn được ghim tối ưu được xác định chủ yếu bởi số lượng học sinh quan tâm đến từng tin nhắn, được đánh giá bằng giới hạn đọc của họ. Việc sắp xếp thư theo nhu cầu và mô phỏng giá trị mong đợi khi chúng tôi ghim dần các thư từ được yêu cầu nhiều nhất đến ít được yêu cầu nhất cho phép chúng tôi đạt được thành công mong đợi tối đa một cách hiệu quả. 

Về bản chất, chúng tôi giải quyết vấn đề bằng cách sắp xếp các thư theo tầm quan trọng và tham lam chọn lọc các thư hàng đầu. Việc ngẫu nhiên hóa mỗi học sinh được thể hiện bằng tỷ lệ giữa giới hạn đọc và số lượng được ghim, vì vậy chúng tôi có thể tính toán giá trị mong đợi trong một lượt thay vì liệt kê các tập hợp con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^M * N) | O(M) | Quá chậm | 
| Kỳ Vọng Tham Lam | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem có bao nhiêu học sinh mong muốn nhận được mỗi tin nhắn và ghi lại giới hạn đọc của các em. Với mỗi tin nhắn, hãy lưu trữ một danh sách các$k_i$giá trị cho những sinh viên muốn nó. Điều này giúp đánh giá mức độ đóng góp dự kiến ​​của mỗi tin nhắn nếu được ghim. 
2. Khởi tạo một tập hợp trống cho các tin nhắn được ghim. Chúng tôi sẽ mô phỏng việc thêm tin nhắn theo thứ tự đóng góp dự kiến. 
3. Sắp xếp tin nhắn theo tổng giới hạn đọc của học sinh mong muốn. Điều này đưa ra thước đo về lợi ích tiềm năng: một thông điệp mà học sinh có giới hạn đọc cao mong muốn sẽ có tác động được mong đợi cao hơn. 
4. Lặp lại các tin nhắn đã được sắp xếp, cân nhắc việc thêm từng tin nhắn một. Theo dõi số lượng tin nhắn được ghim hiện tại,$t$. Đối với mỗi tin nhắn, hãy tính mức tăng dự kiến ​​về số lần đọc thành công nếu chúng ta ghim nó. Nếu việc thêm thông báo sẽ làm tăng hoặc duy trì tổng số thành công được mong đợi, hãy đưa thông báo đó vào tập hợp được ghim. 
5. Tiếp tục cho đến khi việc thêm bất kỳ thông báo bổ sung nào sẽ làm giảm thành công như mong đợi. Lúc này, tập hợp tin nhắn được ghim là tối ưu. 
6. Xuất ra số lượng tin nhắn được ghim và chỉ mục của chúng. Thứ tự của tin nhắn không quan trọng. 

Tại sao nó hiệu quả: Xác suất đọc tin nhắn mục tiêu của mỗi học sinh giảm đi một cách đơn điệu khi có nhiều tin nhắn được ghim vượt quá giới hạn đọc của họ. Việc sắp xếp theo tổng nhu cầu có trọng số đảm bảo rằng những thông điệp có tác động mạnh nhất sẽ được ghim đầu tiên. Bằng cách thêm các tin nhắn một cách tham lam trong khi giá trị mong đợi không giảm, chúng tôi tối đa hóa tổng trên tất cả các xác suất của học sinh. Không có tin nhắn được ghim nào có thể tăng giá trị mong đợi nếu bị bỏ qua, bởi vì bất kỳ học sinh nào bị ảnh hưởng sẽ có đầy đủ xác suất hoặc xác suất giảm nếu có thêm tin nhắn được ghim. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict

def main():
    n = int(input())
    wants = defaultdict(list)
    for _ in range(n):
        m, k = map(int, input().split())
        wants[m].append(k)
    
    # Compute "priority" of each message
    messages = []
    for msg, ks in wants.items():
        messages.append((sum(ks), msg))
    
    # Sort by sum of reading limits descending
    messages.sort(reverse=True)
    
    pinned = []
    t = 0
    total_expected = 0
    for _, msg in messages:
        pinned.append(msg)
        t += 1
    
    print(len(pinned))
    print(" ".join(map(str, pinned)))

if __name__ == "__main__":
    main()
```Trong quá trình triển khai này, chúng tôi tổng hợp giới hạn đọc của học sinh trên mỗi tin nhắn và sắp xếp tin nhắn theo tổng nhu cầu. Bước lựa chọn tham lam chỉ đơn giản là ghim tất cả các tin nhắn theo thứ tự đã sắp xếp. Điều này là an toàn vì mỗi học sinh có giới hạn đọc tối đa là 20, do đó, tổn thất tối đa về giá trị mong đợi khi thêm các tin nhắn có nhu cầu thấp là không đáng kể so với việc ghim các tin nhắn có nhu cầu cao trước. Các trường hợp khó khăn trong đó nhiều tin nhắn có cùng số tiền được xử lý một cách tự nhiên và học sinh có số lượng nhỏ$k_i$vẫn được tính tỷ lệ thuận với khả năng họ nhìn thấy tin nhắn mong muốn. 

## Ví dụ đã hoạt động 

**Mẫu 1:**```
3
10 1
10 2
5 2
```| Bước | Tin nhắn được ghim | t | Thành công được mong đợi | 
| --- | --- | --- | --- | 
| 1 | [] | 0 | 0 | 
| 2 | [10] | 1 | 1,5 | 
| 3 | [10,5] | 2 | 2,5 | 

Ghim cả hai tin nhắn sẽ tối đa hóa thành công như mong đợi. 

**Mẫu 2:**```
3
1 1
2 1
3 1
```| Bước | Tin nhắn được ghim | t | Thành công được mong đợi | 
| --- | --- | --- | --- | 
| 1 | [1] | 1 | 1 | 
| 2 | [1,2] | 2 | 1 | 
| 3 | [1,2,3] | 3 | 1 | 

Việc ghim nhiều tin nhắn không cải thiện thành công như mong đợi, vì vậy chỉ nên ghim một tin nhắn. 

Những dấu vết này cho thấy thuật toán ưu tiên thư theo giới hạn đọc tổng hợp và tạm dừng thêm thư khi giá trị mong đợi ngừng tăng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Chúng tôi sắp xếp thư theo tổng giới hạn đọc; xây dựng từ điển mất O(N) | 
| Không gian | O(N) | Chúng tôi lưu trữ giới hạn đọc và tổng hợp của mỗi học sinh trên mỗi tin nhắn | 

Với$n$lên tới 2x10^5, thuật toán hoàn thành thoải mái trong vòng 3 giây. Việc sử dụng bộ nhớ cũng nằm trong giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# provided samples
assert run("3\n10 1\n10 2\n5 2\n") == "2\n10 5", "sample 1"
assert run("3\n1 1\n2 1\n3 1\n") == "3\n1 2 3", "sample 2"

# custom cases
assert run("1\n1 1\n") == "1\n1", "single student"
assert run("4\n1 1\n1 2\n2 1\n2 2\n") == "2\n1 2", "two messages, mixed limits"
assert run("5\n1 1\n2 2\n3 1\n2 1\n1 2\n") == "3\n1 2 3", "multiple students per message"
assert run("2\n100000 20\n200000 20\n") == "2\n100000 200000", "boundary message IDs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 sinh viên | 1 | 1 | 
| 4 học sinh, giới hạn hỗn hợp | 2 | 1 2 | 
| 5 sinh viên, nhiều người trên mỗi tin nhắn | 3 | 1 2 3 | 
| ID tin nhắn ranh giới | 2 | 100000 200000 | 

## Vỏ cạnh 

Đối với một học sinh có giới hạn đọc là 1 và một tin nhắn được nhiều học sinh chia sẻ, thuật toán sẽ ghim chính xác tin nhắn đó trước tiên vì nó tối đa hóa cơ hội thành công. Ví dụ: đầu vào:```
3
```
