---
title: "CF 1676B - Kẹo bình đẳng"
description: "Chúng ta được phát một bộ hộp, mỗi hộp chứa một số kẹo. Có chính xác một hộp cho mỗi người bạn và mục tiêu là làm cho tất cả các hộp đều chứa cùng một số kẹo để không người bạn nào cảm thấy bị bỏ rơi. Chúng ta chỉ có thể ăn kẹo trong hộp; chúng tôi không thể thêm bất kỳ."
date: "2026-06-10T01:01:39+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1676
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 790 (Div. 4)"
rating: 800
weight: 1676
solve_time_s: 337
verified: true
draft: false
---

[CF 1676B - Kẹo bình đẳng](https://codeforces.com/problemset/problem/1676/B) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học, phân loại 
**Thời gian giải:** 5 phút 37 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được phát một bộ hộp, mỗi hộp chứa một số kẹo. Có chính xác một hộp cho mỗi người bạn và mục tiêu là làm cho tất cả các hộp đều chứa cùng một số kẹo để không người bạn nào cảm thấy bị bỏ rơi. Chúng ta chỉ có thể ăn kẹo trong hộp; chúng tôi không thể thêm bất kỳ. Đầu vào cung cấp nhiều trường hợp kiểm thử, mỗi trường hợp có số hộp`n`và danh sách các số nguyên đại diện cho số kẹo trong mỗi hộp. Đầu ra là tổng số kẹo tối thiểu chúng ta phải ăn để cân bằng tất cả các hộp cho mỗi trường hợp thử nghiệm. Về cơ bản, vấn đề giảm xuống việc xác định số lượng kẹo tối đa mà chúng ta có thể để lại trong tất cả các hộp, đây là số lượng tối thiểu của mảng và tính toán số lượng kẹo mà chúng ta phải loại bỏ khỏi mỗi hộp để đạt được mức đó. 

Những hạn chế là nhỏ:`n`nhiều nhất là 50 và mỗi số kẹo có thể lên tới`10^7`. Bởi vì`n`rất nhỏ, chúng tôi không cần các cấu trúc dữ liệu hoặc thuật toán phức tạp; thậm chí có thể chấp nhận việc lặp lại các hộp nhiều lần cho mỗi trường hợp thử nghiệm. Tổng của tất cả`t * n`trên các trường hợp thử nghiệm có thể quản lý được trong vòng một giây. Các trường hợp cạnh không rõ ràng bao gồm các mảng trong đó tất cả các hộp đều có cùng số lượng kẹo, mảng có một hộp duy nhất hoặc các hộp có số lượng kẹo tối thiểu rất thấp so với các hộp khác, điều này có thể gây ra số tiền trừ lớn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả số kẹo cuối cùng có thể có từ 1 đến giá trị tối đa trong mảng và tính tổng số kẹo đã ăn cho mỗi ứng cử viên. Điều này đúng vì nó xem xét mọi mục tiêu thống nhất có thể, nhưng nó không hiệu quả. Đối với mỗi trường hợp thử nghiệm, phương pháp này có thể yêu cầu lặp lại tới`10^7`cho mỗi hộp, có thể dẫn đến hàng trăm triệu thao tác cho một trường hợp thử nghiệm. Rõ ràng, điều này là không cần thiết vì số lượng thống nhất tối ưu luôn là số kẹo tối thiểu trong mảng. Việc giảm bất kỳ hộp nào bên dưới sẽ chỉ làm tăng số lượng kẹo được ăn mà không ảnh hưởng đến những hộp khác, còn việc để các hộp phía trên cho phép chúng ta giảm thiểu tổng lượng tiêu thụ. 

Việc quan sát thấy số lượng kẹo mục tiêu là số lượng kẹo tối thiểu trong mảng cho phép chúng tôi giảm vấn đề xuống một lần tuyến tính duy nhất cho mỗi trường hợp thử nghiệm: tìm mức tối thiểu và sau đó tính tổng sự khác biệt giữa mỗi hộp và mức tối thiểu này. Cách tiếp cận này có độ phức tạp về thời gian tỷ lệ thuận với`n`cho mỗi trường hợp thử nghiệm, điều này hoàn toàn có thể chấp nhận được với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force tất cả các mục tiêu | O(n * max(a_i)) | O(1) | Quá chậm | 
| Tối ưu (trừ tối thiểu) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc số ô`n`và danh sách số lượng kẹo`a`. 
2. Tính số kẹo tối thiểu`min_candies`trong danh sách. Điều này thể hiện số lượng kẹo mà mỗi hộp có thể có mà không vượt quá số lượng ban đầu của bất kỳ hộp nào. 
3. Khởi tạo bộ đếm`candies_eaten`về không. Lặp lại qua từng hộp`i`trong danh sách, tính toán`a[i] - min_candies`và thêm sự khác biệt này vào`candies_eaten`. Điều này tượng trưng cho số kẹo chúng ta phải ăn từ hộp`i`để cân bằng nó với mức tối thiểu. 
4. Đầu ra`candies_eaten`cho trường hợp thử nghiệm hiện tại. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: Điều bất biến ở đây là không có hộp nào có thể có nhiều kẹo hơn số kẹo chứa ban đầu, do đó, bất kỳ số kẹo thống nhất nào cũng phải nhỏ hơn hoặc bằng mức tối thiểu. Bằng cách chọn mức tối thiểu, chúng tôi đảm bảo rằng tất cả các hộp đều được giảm thiểu tối thiểu để đạt được sự bình đẳng, đảm bảo số lượng kẹo đã ăn được giảm thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        min_candies = min(a)
        candies_eaten = sum(x - min_candies for x in a)
        print(candies_eaten)

solve()
```Giải pháp đọc từng trường hợp kiểm thử, tính toán mức tối thiểu trong danh sách và tính tổng các chênh lệch. Sử dụng biểu thức tạo cho tổng vừa ngắn gọn vừa hiệu quả. Từ`n`tối đa là 50, quá trình quét tuyến tính này diễn ra ngay lập tức. Điều tinh tế duy nhất là đảm bảo rằng chúng tôi xử lý đầu vào và đầu ra một cách chính xác bằng cách sử dụng I/O nhanh vì sự cố có nhiều trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

Đầu vào mẫu đầu tiên:```
5
5
1 2 3 4 5
6
1000 1000 5 1000 1000 1000
```| Trường hợp thử nghiệm | Hộp`a`| min_candies | Tính ăn kẹo | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | [1, 2, 3, 4, 5] | 1 | (1-1)+(2-1)+(3-1)+(4-1)+(5-1)=10 | 10 | 
| 2 | [1000, 1000, 5, 1000, 1000, 1000] | 5 | 995+995+0+995+995+995=4975 | 4975 | 

Những dấu vết này xác nhận rằng việc lấy số lượng tối thiểu và trừ đi từ mỗi hộp sẽ tạo ra tổng số kẹo đã ăn chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n) | Mỗi trường hợp thử nghiệm yêu cầu quét tuyến tính để tìm ra sự khác biệt tối thiểu và tổng. | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng hộp cho mỗi trường hợp thử nghiệm. | 

Được cho`t ≤ 1000`Và`n ≤ 50`, tổng số thao tác tối đa là 50.000, nằm trong giới hạn 1 giây. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("5\n5\n1 2 3 4 5\n6\n1000 1000 5 1000 1000 1000\n10\n1 2 3 5 1 2 7 9 13 5\n3\n8 8 8\n1\n10000000\n") == \
"10\n4975\n38\n0\n0", "sample 1"

# Custom test cases
assert run("1\n1\n7\n") == "0", "single box"
assert run("1\n3\n5 5 5\n") == "0", "all equal"
assert run("1\n4\n10 20 30 40\n") == "60", "linear difference"
assert run("1\n5\n1 1 1 1 2\n") == "1", "one box higher"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 hộp [7] | 0 | hộp đơn không cần giảm | 
| 3 hộp [5,5,5] | 0 | tất cả các hộp bằng nhau không yêu cầu giảm | 
| 4 hộp [10,20,30,40] | 60 | trường hợp tổng quát sai phân tuyến tính | 
| 5 hộp [1,1,1,1,2] | 1 | trên mức tối thiểu một ô | 

## Vỏ cạnh 

Hãy xem xét trường hợp cạnh chỉ có một hộp, ví dụ:`[7]`. Tối thiểu là 7 và không có ô nào khác để cân bằng. Thuật toán tính toán chính xác 0 viên kẹo đã ăn. Một trường hợp cạnh khác là khi tất cả các hộp đều bằng nhau, chẳng hạn như`[5,5,5]`. Thuật toán vẫn tính 0. Trường hợp một hộp cao hơn các hộp khác, chẳng hạn như`[1,1,1,2]`, dẫn đến chỉ ăn phần thừa và phép tính đưa ra kết quả chính xác là 1. Các ví dụ này xác nhận rằng thuật toán xử lý chính xác cả đầu vào tầm thường và hơi sai lệch.
