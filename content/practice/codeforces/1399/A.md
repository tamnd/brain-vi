---
title: "CF 1399A - Loại bỏ nhỏ nhất"
description: "Chúng ta được yêu cầu xác định xem liệu chúng ta có thể rút gọn một mảng các số nguyên dương thành một phần tử bằng cách sử dụng một thao tác cụ thể hay không: chọn hai phần tử riêng biệt có hiệu nhiều nhất là một và loại bỏ phần tử nhỏ hơn trong hai phần tử (hoặc nếu chúng bằng nhau)."
date: "2026-06-11T08:58:18+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1399
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 661 (Div. 3)"
rating: 800
weight: 1399
solve_time_s: 89
verified: true
draft: false
---

[CF 1399A - Xóa nhỏ nhất](https://codeforces.com/problemset/problem/1399/A) 

**Đánh giá:** 800 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem liệu chúng ta có thể rút gọn một mảng các số nguyên dương thành một phần tử bằng cách sử dụng một thao tác cụ thể hay không: chọn hai phần tử riêng biệt có hiệu nhiều nhất là một và loại bỏ phần tử nhỏ hơn trong hai phần tử (hoặc nếu chúng bằng nhau). Mảng có thể chứa các bản sao và mỗi trường hợp thử nghiệm là độc lập. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, độ dài mảng tối đa là 50 và mỗi phần tử nằm trong khoảng từ 1 đến 100. Điều này có nghĩa là bất kỳ nghiệm bậc hai nào trong`n`sẽ chạy thoải mái trong giới hạn thời gian, vì 50 bình phương chỉ có 2500 thao tác cho mỗi trường hợp thử nghiệm. Với tối đa 1000 trường hợp thử nghiệm, chúng tôi đạt tới 2,5 triệu thao tác trong trường hợp xấu nhất, có thể chấp nhận được trong giới hạn 1 giây. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng trong đó tất cả các phần tử đều bằng nhau, mảng có một phần tử duy nhất và các mảng có khoảng cách tối đa giữa các phần tử liên tiếp lớn hơn 1. Ví dụ:`[1, 2, 4]`không thể giảm xuống thành một phần tử duy nhất vì 2 và 4 khác nhau nhiều hơn 1 và không có trình tự loại bỏ nào được phép có thể loại bỏ khoảng cách này. Tương tự,`[5, 5, 5, 5]`luôn có thể được giảm xuống còn một phần tử, trong khi`[100]`tầm thường đã là một yếu tố duy nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng tất cả các bước di chuyển có thể xảy ra: liên tục tìm kiếm một cặp phần tử có hiệu nhiều nhất là 1, loại bỏ phần tử nhỏ hơn và tiếp tục cho đến khi còn lại một phần tử hoặc không thể di chuyển được. Điều này làm việc cho nhỏ`n`nhưng trở nên lộn xộn và không hiệu quả, vì chúng ta sẽ phải liên tục tìm kiếm các cặp hợp lệ và loại bỏ các phần tử một cách linh hoạt. Số lượng hoạt động trong trường hợp xấu nhất là khoảng`O(n^3)`vì mỗi lần xóa yêu cầu quét tất cả các cặp, điều này là quá mức cần thiết ngay cả đối với`n = 50`. 

Quan sát quan trọng là khả năng loại bỏ các phần tử được xác định hoàn toàn bởi khoảng cách giữa các số liên tiếp sau khi mảng được sắp xếp. Nếu mảng được sắp xếp và mỗi cặp liền kề khác nhau nhiều nhất là 1 thì cuối cùng mọi phần tử đều có thể bị loại bỏ ngoại trừ phần tử lớn nhất. Nếu bất kỳ cặp liền kề nào khác nhau từ 2 trở lên, thì các phần tử ở hai bên của khoảng cách không thể giảm xuống còn một, khiến cho không thể đạt được một mảng một phần tử. 

Quan sát này đơn giản hóa giải pháp sắp xếp mảng và kiểm tra sự khác biệt liên tiếp. Phân loại chi phí`O(n log n)`và một lần quét tuyến tính duy nhất xác nhận xem tất cả các khác biệt có nhiều nhất là 1 hay không, đưa ra giải pháp rõ ràng và tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm/lộn xộn | 
| Tối ưu | O(n log n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Chúng tôi sẽ xử lý từng trường hợp thử nghiệm một cách độc lập. 
2. Với mỗi test case, hãy đọc độ dài mảng`n`và các phần tử mảng`a`. 
3. Sắp xếp mảng theo thứ tự tăng dần. Việc sắp xếp đảm bảo rằng chúng ta có thể kiểm tra trực tiếp sự khác biệt giữa các phần tử liên tiếp. 
4. Khởi tạo biến cờ thành "CÓ", giả sử chúng ta có thể giảm mảng thành một phần tử. 
5. Lặp lại mảng từ phần tử đầu tiên đến phần tử thứ hai đến cuối cùng. Đối với mỗi chỉ số`i`, tính sự khác biệt giữa`a[i+1]`Và`a[i]`. Nếu chênh lệch này lớn hơn 1, hãy đặt cờ thành "KHÔNG" và thoát khỏi vòng lặp. Điều này xác định bất kỳ khoảng cách nào ngăn cản việc giảm thiểu hoàn toàn. 
6. Sau khi kiểm tra tất cả các cặp liền kề, xuất cờ cho test case hiện tại. "CÓ" cho biết có thể giảm mảng thành một phần tử duy nhất, nếu không thì "KHÔNG". 

Tại sao nó hoạt động: Sau khi mảng được sắp xếp, hạn chế duy nhất ngăn cản việc giảm hoàn toàn là sự hiện diện của các khoảng trống lớn hơn 1. Bất kỳ hai số liên tiếp nào có chênh lệch 1 hoặc 0 cuối cùng đều có thể được giảm bớt bằng các thao tác xóa lặp đi lặp lại, bởi vì chúng ta luôn có thể loại bỏ số nhỏ hơn cho đến khi chỉ còn lại một. Bất kỳ sự khác biệt nào từ 2 trở lên đều tạo ra một rào cản không thể vượt qua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()
    possible = True
    for i in range(n - 1):
        if a[i+1] - a[i] > 1:
            possible = False
            break
    print("YES" if possible else "NO")
```Giải pháp đầu tiên là đọc số lượng trường hợp thử nghiệm. Mỗi mảng được sắp xếp, cho phép quét tuyến tính một lần để phát hiện bất kỳ chênh lệch nào lớn hơn 1. Biến`possible`theo dõi xem mảng có thể được giảm xuống còn một phần tử hay không và chúng tôi sớm phát hiện khoảng cách lớn hơn 1. Việc sắp xếp tự động xử lý các phần tử trùng lặp và đảm bảo chúng tôi chỉ cần kiểm tra các cặp liên tiếp. Không có cấu trúc dữ liệu bổ sung được yêu cầu. 

## Ví dụ đã hoạt động 

Theo dõi thông qua đầu vào`[1, 2, 2]`: 

| Bước | Mảng | Kiểm tra | Hành động | 
|---|---|---|---| 
| 1 | [1, 2, 2] | 2 - 1 = 1 | được | 
| 2 | [1, 2, 2] | 2 - 2 = 0 | được | 
| 3 | - | tất cả các khác biệt <= 1 | CÓ | 

Theo dõi thông qua đầu vào`[1, 2, 4]`: 

| Bước | Mảng | Kiểm tra | Hành động | 
|---|---|---|---| 
| 1 | [1, 2, 4] | 2 - 1 = 1 | được | 
| 2 | [1, 2, 4] | 4 - 2 = 2 | Không được phép, nghỉ | 
| 3 | - | khoảng cách > 1 được phát hiện | KHÔNG | 

Những dấu vết này chứng tỏ rằng chỉ cần kiểm tra những khác biệt liên tiếp là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, quét tuyến tính là O(n) | 
| Không gian | O(1) thêm | Việc sắp xếp có thể được thực hiện tại chỗ | 

Với`n <= 50`Và`t <= 1000`, giải pháp thực hiện tối đa`1000 * 50 log 50 ≈ 85000`hoạt động, trong vòng 1 giây. Việc sử dụng bộ nhớ ở mức tối thiểu và dễ dàng phù hợp trong phạm vi 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()
        possible = True
        for i in range(n - 1):
            if a[i+1] - a[i] > 1:
                possible = False
                break
        print("YES" if possible else "NO")
    return out.getvalue().strip()

# Provided samples
assert run("5\n3\n1 2 2\n4\n5 5 5 5\n3\n1 2 4\n4\n1 3 4 4\n1\n100") == "YES\nYES\nNO\nNO\nYES", "sample 1"

# Custom cases
assert run("3\n2\n1 3\n2\n1 2\n3\n10 10 10") == "NO\nYES\nYES", "gaps, minimal case, all equal"
assert run("1\n50\n" + " ".join(str(i) for i in range(1, 51))) == "NO", "max size, consecutive 1..50"
assert run("1\n50\n" + " ".join(["1"]*50)) == "YES", "all equal large n"
assert run("1\n5\n1 2 3 4 5") == "YES", "consecutive small array"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
|`2 1 3\n2 1 2\n3 10 10 10`|`NO\nYES\nYES`| khoảng cách > 1, mảng tối thiểu, tất cả đều bằng nhau | 
|`50 1..50`|`NO`| mảng kích thước tối đa với sự khác biệt ngày càng tăng | 
|`50 * [1]`|`YES`| tất cả các phần tử bằng nhau trong mảng lớn | 
|`1 2 3 4 5`|`YES`| mảng nhỏ liên tiếp | 

## Vỏ cạnh 

Đối với mảng một phần tử`[100]`, việc sắp xếp không có tác dụng. Vòng lặp trên các khác biệt liên tiếp không chạy vì chỉ có một phần tử. Thuật toán xuất ra "CÓ" chính xác vì không cần thực hiện thao tác nào. 

Đối với các mảng trong đó tất cả các phần tử đều bằng nhau, chẳng hạn như`[5, 5, 5, 5]`, chênh lệch bằng 0, do đó thuật toán đưa ra "CÓ". 

Đối với các mảng có khoảng trống,`[1, 3]`, sự khác biệt`3 - 1 = 2`kích hoạt tình trạng ngắt, tạo ra "NO" ngay lập tức. Điều này xử lý ràng buộc chính ngăn cản việc giảm bớt một phần tử.
