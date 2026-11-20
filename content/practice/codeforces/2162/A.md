---
title: "CF 2162A - Đẹp trung bình"
description: "Chúng ta được cung cấp một mảng các số nguyên, mỗi số từ 1 đến 10 và chúng ta được yêu cầu tìm giá trị trung bình tối đa của bất kỳ mảng con liền kề nào. Mảng con đơn giản là một đoạn liên tiếp của mảng, có thể ngắn bằng một phần tử hoặc dài bằng toàn bộ mảng."
date: "2026-06-07T23:53:22+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2162
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1059 (Div. 3)"
rating: 800
weight: 2162
solve_time_s: 97
verified: true
draft: false
---

[CF 2162A - Trung bình đẹp](https://codeforces.com/problemset/problem/2162/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên, mỗi số từ 1 đến 10 và chúng ta được yêu cầu tìm giá trị trung bình tối đa của bất kỳ mảng con liền kề nào. Mảng con đơn giản là một đoạn liên tiếp của mảng, có thể ngắn bằng một phần tử hoặc dài bằng toàn bộ mảng. Đầu ra phải là một số nguyên, điều này được đảm bảo trong bài toán này vì tất cả các phần tử đều là số nguyên và giá trị trung bình lớn nhất xảy ra trên chính một trong các số nguyên đó. 

Ràng buộc đầu tiên là độ dài của mảng,$n$, nhiều nhất là 10. Giá trị này đủ nhỏ để bất kỳ nghiệm nào có độ phức tạp hàm mũ trong$n$là khả thi. Mỗi trường hợp thử nghiệm là độc lập và có thể có tới 10.000 trường hợp thử nghiệm. Kể cả khi chúng ta biểu diễn$O(n^2)$hoạt động cho mỗi trường hợp thử nghiệm, điều này sẽ khoảng$10^4 \times 100 = 10^6$hoạt động, nằm trong giới hạn 1 giây. 

Trường hợp cạnh không rõ ràng phát sinh khi tất cả các phần tử đều bằng nhau. Ví dụ: với mảng [3, 3, 3, 3], giá trị trung bình tối đa là 3 và mọi nỗ lực xem xét tổng của nhiều phần tử đều không làm tăng kết quả. Một điểm tinh tế khác là khi mảng có sự kết hợp giữa các giá trị thấp và cao, như [1, 9, 2]; mức trung bình tối đa chỉ đơn giản là phần tử lớn nhất, bởi vì bất kỳ sự kết hợp nào với các phần tử nhỏ hơn sẽ làm giảm mức trung bình. Một cách tiếp cận bạo lực bất cẩn tính toán tất cả các giá trị trung bình của mảng con mà không nhận ra thuộc tính này vẫn có thể tạo ra câu trả lời đúng do những ràng buộc nhỏ, nhưng nó che khuất cái nhìn sâu sắc đơn giản hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản rất đơn giản: xem xét mọi phân mảng có thể có, tính tổng của nó, chia cho độ dài của nó và theo dõi mức tối đa. Điều này hoạt động vì chỉ có$n(n+1)/2$mảng con, tối đa là 55 cho$n = 10$. Tuy nhiên, cách tiếp cận này thực hiện công việc dư thừa: đối với mỗi mảng con, nó tính lại tổng từ đầu, việc này không cần thiết đối với các mảng nhỏ như vậy nhưng lại có tỷ lệ kém đối với các mảng lớn hơn. 

Điểm mấu chốt là vì tất cả các phần tử đều là số nguyên dương nên mảng con tối đa hóa giá trị trung bình luôn là phần tử lớn nhất. Việc thêm các phần tử nhỏ hơn vào mảng con chỉ làm giảm giá trị trung bình, trong khi việc thêm các phần tử bằng nhau không cải thiện được giá trị đó. Do đó, chúng ta có thể bỏ qua tất cả các kết hợp và chỉ cần lấy phần tử lớn nhất của mảng làm câu trả lời. Điều này làm giảm việc tính toán từ$O(n^2)$ĐẾN$O(n)$cho mỗi trường hợp thử nghiệm, điều này là quá đủ với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Chấp nhận được do n nhỏ | 
| Tối ưu (phần tử tối đa) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Chúng tôi xử lý từng trường hợp kiểm thử một cách độc lập nên có thể lặp lại từng trường hợp kiểm thử một. 
2. Với mỗi test case, hãy đọc độ dài của mảng$n$và các phần tử mảng$a_1, \dots, a_n$. 
3. Tìm phần tử lớn nhất của mảng bằng cách quét tuyến tính đơn giản. Điều này thể hiện mức trung bình lớn nhất có thể có của bất kỳ mảng con nào. 
4. In phần tử tối đa. Vì giá trị trung bình tối đa của mảng con luôn là số nguyên nên không cần làm tròn. 

Tại sao nó hoạt động: tính bất biến là bất kỳ mức trung bình nào của mảng con đều bị giới hạn bởi phần tử lớn nhất của nó. Việc bao gồm các phần tử nhỏ hơn trong một mảng con sẽ làm giảm giá trị trung bình và việc bao gồm các phần tử bằng nhau sẽ không làm tăng giá trị trung bình. Do đó, phần tử lớn nhất của mảng luôn là giá trị trung bình lớn nhất của mảng con, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(max(a))
```Giải pháp đọc số lượng trường hợp thử nghiệm, sau đó lặp lại từng trường hợp. Chúng tôi chuyển đổi dòng số nguyên thành một danh sách và sử dụng tính năng tích hợp sẵn của Python`max`để tìm phần tử lớn nhất một cách hiệu quả. sử dụng`sys.stdin.readline`đảm bảo đầu vào nhanh chóng cho một số lượng lớn các trường hợp thử nghiệm. Từ`n`rất nhỏ nên chi phí chuyển đổi danh sách là không đáng kể. Giải pháp tránh tính toán tổng hoặc trung bình không cần thiết cho các mảng con vượt quá phần tử tối đa. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
4
3 3 3 3
7 1 6 9 9
3 4 4 4 3
1 2 3
```| Trường hợp thử nghiệm | Mảng | Phần tử tối đa | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | [3, 3, 3, 3] | 3 | 3 | 
| 2 | [7, 1, 6, 9, 9] | 9 | 9 | 
| 3 | [3, 4, 4, 4, 3] | 4 | 4 | 
| 4 | [1, 2, 3] | 3 | 3 | 

Dấu vết này chứng minh rằng bất kể độ dài mảng con hay sự kết hợp phần tử như thế nào, phần tử riêng lẻ lớn nhất đều đạt được mức trung bình tối đa. Bảng xác nhận rằng quét tuyến tính xác định chính xác nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n) | Mỗi trường hợp kiểm thử thực hiện một lần quét có độ dài n, lặp lại cho t trường hợp | 
| Không gian | O(n) | Lưu trữ mảng có độ dài n cho mỗi trường hợp thử nghiệm | 

Được cho$n \le 10$Và$t \le 10^4$, tổng số thao tác tối đa là 10^5, vừa vặn thoải mái trong giới hạn thời gian 1 giây. Việc sử dụng bộ nhớ là tối thiểu vì chúng ta chỉ lưu trữ một mảng tại một thời điểm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        output.append(str(max(a)))
    return "\n".join(output)

# provided samples
assert run("3\n4\n3 3 3 3\n5\n7 1 6 9 9\n5\n3 4 4 4 3\n") == "3\n9\n4"

# custom test cases
assert run("1\n1\n10\n") == "10", "single element array"
assert run("1\n10\n1 2 3 4 5 6 7 8 9 10\n") == "10", "maximum-size array"
assert run("1\n5\n5 5 5 5 5\n") == "5", "all-equal values"
assert run("2\n3\n1 10 2\n4\n2 2 9 3\n") == "10\n9", "mixed small arrays"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1\n10 | 10 | Trường hợp cạnh mảng một phần tử | 
| 1\n10\n1..10 | 10 | Mảng kích thước tối đa | 
| 1\n5\n5 5 5 5 5 | 5 | Các phần tử bằng nhau | 
| 2\n3\n1 10 2\n4\n2 2 9 3 | 10\n9 | Giá trị hỗn hợp, nhiều trường hợp thử nghiệm | 

## Vỏ cạnh 

Nếu mảng chỉ chứa một phần tử, chẳng hạn như [10], thuật toán sẽ đưa ra 10. Điều này đúng vì mảng con duy nhất chính là mảng đó. Nếu tất cả các phần tử đều bằng nhau, chẳng hạn như [5, 5, 5, 5], thì thuật toán sẽ đưa ra 5, nhất quán với mức trung bình tối đa giữa bất kỳ mảng con nào. Đối với các mảng có giá trị cao và thấp hỗn hợp, chẳng hạn như [1, 10, 2], thuật toán sẽ chọn chính xác phần tử lớn nhất, 10, làm giá trị trung bình tối đa của mảng con mà không bị phân tâm bởi tổng các số nhỏ hơn.
