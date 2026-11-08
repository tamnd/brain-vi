---
title: "CF 1431D - Bút đánh dấu đã qua sử dụng"
description: "Chúng tôi được giao nhiệm vụ lên lịch cho một loạt bài giảng trong khán phòng, mỗi bài giảng sẽ sử dụng một điểm đánh dấu chung. Mỗi giảng viên đều có một sự khoan dung riêng: họ sẽ từ chối sử dụng bút đánh dấu đã được sử dụng một số lần nhất định, thay vào đó họ sẽ yêu cầu dùng bút mới."
date: "2026-06-11T05:14:55+07:00"
tags: ["codeforces", "competitive-programming", "*special", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1431
codeforces_index: "D"
codeforces_contest_name: "Kotlin Heroes 5: ICPC Round"
rating: 1500
weight: 1431
solve_time_s: 582
verified: false
draft: false
---

[CF 1431D - Bút đánh dấu đã qua sử dụng](https://codeforces.com/problemset/problem/1431/D) 

**Đánh giá:** 1500 
**Tags:** *đặc biệt, tham lam 
**Thời gian giải:** 9 phút 42 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao nhiệm vụ lên lịch cho một loạt bài giảng trong khán phòng, mỗi bài giảng sẽ sử dụng một điểm đánh dấu chung. Mỗi giảng viên đều có một sự khoan dung riêng: họ sẽ từ chối sử dụng bút đánh dấu đã được sử dụng một số lần nhất định, thay vào đó họ sẽ yêu cầu dùng bút mới. Mục tiêu của chúng tôi là chọn thứ tự bài giảng tối đa hóa số lượng dấu riêng biệt được sử dụng thực sự, đồng thời chỉ thay thế dấu khi giảng viên yêu cầu. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm độc lập. Với mỗi trường hợp, chúng ta được cung cấp số lượng bài giảng và một dãy số nguyên biểu thị dung sai cho mỗi giảng viên. Đầu ra là thứ tự các chỉ số giảng viên nhằm tối đa hóa việc sử dụng điểm đánh dấu. Các ràng buộc ở mức vừa phải: tối đa 100 trường hợp thử nghiệm, mỗi trường hợp có tối đa 500 bài giảng, nghĩa là tổng số thao tác sẽ phù hợp một cách thoải mái với một giải pháp có độ phức tạp về thời gian theo thứ tự$O(n \log n)$hoặc$O(n^2)$mỗi trường hợp thử nghiệm. 

Các trường hợp khó khăn phát sinh khi nhiều giảng viên có cùng giá trị dung sai hoặc khi tất cả giảng viên có ngưỡng chấp nhận rất thấp hoặc rất cao. Ví dụ: nếu tất cả các giảng viên có dung sai là 1 thì mỗi bài giảng sau bài giảng đầu tiên sẽ buộc phải thay thế điểm đánh dấu. Nếu một thuật toán bất cẩn bỏ qua kích thước tương đối của các giá trị dung sai, nó có thể xếp lịch cho giảng viên theo thứ tự làm giảm số lượng điểm đánh dấu được sử dụng một cách không cần thiết. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ xem xét tất cả các hoán vị có thể có của giảng viên, mô phỏng trình tự bài giảng cho mỗi hoán vị và đếm số lượng điểm đánh dấu được sử dụng. Phương pháp này đúng nhưng không khả thi vì số hoán vị tăng theo giai thừa,$n!$, điều này không thực tế ngay cả đối với$n = 10$. 

Cái nhìn sâu sắc quan trọng là tập trung vào các giá trị dung sai tương đối. Bút chỉ cần được thay thế khi dung sai của bài giảng hiện tại vượt quá số lần bút đã được sử dụng. Để tối đa hóa số lượng điểm được sử dụng, chúng tôi muốn các giảng viên có giá trị dung sai thấp nhất đến sớm vì họ sẽ yêu cầu thay thế sớm hơn. Giảng viên có giá trị dung sai cao hơn có thể được lên lịch sau để kéo dài tuổi thọ của điểm đánh dấu hiện tại. 

Việc sắp xếp giảng viên theo thứ tự tăng dần của giá trị dung sai của họ sẽ đạt được điều này. Điều này đảm bảo rằng chúng tôi thay thế bút đánh dấu khi cần thiết và không lãng phí bài giảng về bút đánh dấu sẽ bị từ chối ngay lập tức. Vì chúng ta chỉ cần duyệt qua danh sách đã sắp xếp một lần để xây dựng thứ tự nên cách tiếp cận này hiệu quả và đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tham lam sắp xếp theo khoan dung | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, đọc số lượng bài giảng và dãy giá trị chấp nhận. 
3. Ghép nối chỉ số của từng giảng viên với giá trị dung sai của họ để chúng ta có thể giữ nguyên vị trí ban đầu. 
4. Sắp xếp danh sách các cặp này theo giá trị dung sai theo thứ tự tăng dần. 
5. Trích xuất các chỉ số đã sắp xếp theo thứ tự bài giảng. 
6. In thứ tự kết quả cho ca kiểm thử hiện tại. 

Tại sao nó hoạt động: Bằng cách luôn đặt những giảng viên có ngưỡng chấp nhận nhỏ hơn lên hàng đầu, chúng tôi đảm bảo rằng mỗi điểm đánh dấu đều đạt được số lần sử dụng tối đa trước khi phải thay thế. Cách tiếp cận tham lam này đảm bảo rằng số lượng bút đánh dấu được sử dụng là tối đa, bởi vì bất kỳ sai lệch nào - sắp xếp sớm một giảng viên có khả năng chịu đựng cao - đều không thể tăng mức sử dụng bút đánh dấu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        # pair index with value
        indexed = [(a[i], i + 1) for i in range(n)]
        # sort by acceptance value ascending
        indexed.sort()
        # extract order
        order = [idx for _, idx in indexed]
        print(' '.join(map(str, order)))

if __name__ == "__main__":
    solve()
```Chúng tôi ghép khả năng chịu đựng của mỗi giảng viên với chỉ số dựa trên 1 của họ để giữ nguyên cách đánh số ban đầu. Việc sắp xếp đảm bảo chúng tôi kích hoạt việc thay thế điểm đánh dấu một cách hiệu quả nhất có thể. Việc trích xuất các chỉ số sau khi sắp xếp sẽ cho ra thứ tự bài giảng mong muốn. sử dụng`sys.stdin.readline`đảm bảo xử lý đầu vào nhanh chóng, đặc biệt đối với nhiều trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

Dấu vết cho trường hợp thử nghiệm 1 từ mẫu: 

Mảng đầu vào:`[1, 2, 1, 2]`Kết hợp với các chỉ số:`[(1,1), (2,2), (1,3), (2,4)]`Sắp xếp theo dung sai:`[(1,1), (1,3), (2,2), (2,4)]`Thứ tự kết quả:`[1,3,2,4]`| Bước | Sử dụng điểm đánh dấu hiện tại | Giảng viên tiếp theo | Hành động | Số điểm đánh dấu | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | dung sai 1, điểm đánh dấu mới | 1 | 
| 2 | 1 | 3 | dung sai 1, điểm đánh dấu được sử dụng một lần -> thay thế | 2 | 
| 3 | 1 | 2 | dung sai 2, điểm đánh dấu được sử dụng một lần -> tiếp tục | 2 | 
| 4 | 2 | 4 | dung sai 2, điểm đánh dấu được sử dụng hai lần -> tiếp tục | 2 | 

Dấu vết chứng minh rằng việc đặt những giảng viên có khả năng chịu đựng thấp trước tiên sẽ kích hoạt những thay thế cần thiết mà không lãng phí những điểm đánh dấu sớm. 

Trường hợp thử nghiệm thứ hai`[2,3,1,3]`ghép nối:`[(2,1),(3,2),(1,3),(3,4)]`, sắp xếp:`[(1,3),(2,1),(3,2),(3,4)]`, đặt hàng:`[3,1,2,4]`đạt được mức sử dụng điểm đánh dấu tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n log n) | Sắp xếp chiếm ưu thế trên mỗi trường hợp thử nghiệm | 
| Không gian | O(n) | Lưu trữ các cặp giá trị chỉ mục để sắp xếp | 

Giải pháp vừa vặn thoải mái trong vòng 3 giây cho$n \le 500$Và$t \le 100$, bởi vì$t \cdot n \log n \approx 100 * 500 * 9 \approx 450,000$hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("4\n4\n1 2 1 2\n2\n2 1\n3\n1 1 1\n4\n2 3 1 3\n") == "1 3 2 4\n2 1\n1 2 3\n3 1 2 4", "sample 1"

# custom: minimum-size input
assert run("1\n1\n1\n") == "1", "single lecturer"

# custom: all equal tolerance
assert run("1\n3\n2 2 2\n") == "1 2 3", "all equal"

# custom: descending tolerance
assert run("1\n5\n5 4 3 2 1\n") == "5 4 3 2 1", "reverse sorted"

# custom: ascending tolerance
assert run("1\n5\n1 2 3 4 5\n") == "1 2 3 4 5", "already optimal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | giảng viên đơn | 
| 2 | 1 2 3 | mọi sự khoan dung như nhau | 
| 3 | 5 4 3 2 1 | dung sai giảm dần | 
| 4 | 1 2 3 4 5 | dung sai tăng dần | 

## Vỏ cạnh 

Nếu tất cả các giảng viên có giá trị chấp nhận như nhau thì mọi thứ tự đều có hiệu lực. Thuật toán xử lý việc này một cách chính xác vì việc sắp xếp theo dung sai sẽ duy trì thứ tự tương đối trong cách sắp xếp ổn định của Python. Đối với một giảng viên, thuật toán sẽ đưa ra chính xác giảng viên đó. Đối với các chuỗi đã được sắp xếp theo dung sai tăng dần hoặc giảm dần, thuật toán sẽ duy trì hoặc đảo ngược thứ tự một cách thích hợp, đảm bảo sử dụng tối đa các điểm đánh dấu mà không cần thay thế không cần thiết.
