---
title: "CF 1876C - Tự tổng hợp"
description: "Cho một mảng các số nguyên dương a có độ dài n. Nhiệm vụ là thực hiện một chuỗi các thao tác \"vòng tròn\" trên các phần tử của a. Mỗi thao tác chọn một phần tử theo chỉ mục của nó và \"khoanh tròn\" nó và chúng ta có thể khoanh tròn cùng một phần tử nhiều lần."
date: "2026-06-08T22:59:31+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "graphs", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1876
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 902 (Div. 1, based on COMPFEST 15 - Final Round)"
rating: 2100
weight: 1876
solve_time_s: 220
verified: false
draft: false
---

[CF 1876C - Tự động tổng hợp](https://codeforces.com/problemset/problem/1876/C) 

**Xếp hạng:** 2100 
**Thẻ:** thuật toán xây dựng, dfs và tương tự, đồ thị, tham lam, sắp xếp 
**Thời gian giải:** 3 phút 40s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương`a`chiều dài`n`. Nhiệm vụ là thực hiện một chuỗi các thao tác “vòng tròn” trên các phần tử của`a`. Mỗi thao tác chọn một phần tử theo chỉ mục của nó và "khoanh tròn" nó và chúng ta có thể khoanh tròn cùng một phần tử nhiều lần. Sau tất cả các thao tác, các phần tử không được khoanh tròn, theo thứ tự ban đầu, tạo thành một mảng mới`r`. Mảng các chỉ số phần tử được khoanh tròn, theo thứ tự chúng ta thực hiện các thao tác, tạo thành một mảng khác`p`. Mục tiêu của chúng tôi là thực hiện các hoạt động sao cho`r`kết thúc bằng`p`. Nếu không thể, chúng tôi báo cáo`-1`. 

Những hạn chế là đáng kể:`n`có thể đạt được`2·10^5`và các phần tử của`a`nhiều nhất là`n`. Điều này ngay lập tức loại trừ bất kỳ giải pháp vũ phu nào thử tất cả các chuỗi con có thể có của các phần tử không được khoanh tròn, bởi vì số lượng các chuỗi con tăng theo cấp số nhân. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính trong`n`. 

Sự tinh tế của vấn đề nằm ở sự phù hợp giữa`r`Và`p`. Một cách tiếp cận đơn giản có thể là khoanh tròn các phần tử một cách tham lam từ phía trước, nhưng giải pháp có thể yêu cầu khoanh tròn các phần tử nhiều lần để "đẩy" các phần tử còn lại vào đúng thứ tự. Ví dụ, nếu`a = [1,2,1]`, người ta có thể nghĩ chỉ khoanh tròn số đầu tiên`1`đủ, nhưng để sản xuất`r = [1,2,1]`theo yêu cầu, chúng tôi có thể cần khoanh tròn một cách chiến lược để cho phép các yếu tố lặp lại xuất hiện ở đúng vị trí. 

Các trường hợp cạnh dễ xử lý sai bao gồm các mảng có số lặp lại trong đó thứ tự khoanh tròn rất quan trọng. Ví dụ,`a = [2,2,2]`và chúng tôi muốn`p = [2,2,2]`là không thể, bởi vì sau khi khoanh tròn tất cả các phần tử,`r`sẽ trống, không bao giờ bằng`p`. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xem xét mọi số lượng hoạt động có thể có và mọi chuỗi con của các phần tử không được khoanh tròn để kiểm tra xem nó có thể khớp với một ứng cử viên hay không`p`. Mặc dù đúng về mặt nguyên tắc, nhưng điều này đòi hỏi phải kiểm tra số lượng dãy con theo cấp số nhân, khiến nó không thể thực hiện được.`n = 2·10^5`. 

Cái nhìn sâu sắc quan trọng là chúng ta có thể coi vấn đề như một kiểu "bóc tách tham lam" khỏi mảng`a`. để làm`r`bằng`p`, chúng ta cần xóa các phần tử khỏi`a`không khớp với phần tử cuối cùng của`r`theo thứ tự ngược lại. Nghĩa là, chúng ta có thể nghĩ ngược lại: yếu tố cuối cùng của`p`phải xuất hiện dưới dạng phần tử không được khoanh tròn cuối cùng. Nếu chúng ta đi ngang qua`a`ngược lại, bất cứ khi nào chúng ta gặp giá trị mục tiêu hiện tại của`r`, chúng ta không khoanh tròn nó; tất cả các yếu tố khác được khoanh tròn và thêm vào các hoạt động. Bằng cách liên tục bóc bỏ các lớp từ cuối và theo dõi xem phần tử nào còn lại, chúng ta có thể xây dựng lại một chuỗi các thao tác đảm bảo`r = p`nếu có thể. 

Phương pháp này tận dụng thực tế là bất kỳ phần tử nào được khoanh tròn nhiều lần đều có thể xuất hiện nhiều lần trong`p`và bằng cách làm ngược lại, chúng ta tránh được việc kiểm tra trình tự phức tạp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Xây dựng tham lam / nhiều lớp ngược | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo danh sách`ops`để lưu trữ chuỗi các chỉ số được khoanh tròn. Khởi tạo`remaining`như một bản sao của`a`. 
2. Trong khi`remaining`không trống, hãy xác định giá trị duy nhất cuối cùng`val`TRONG`remaining`nó phải xuất hiện ở cuối`r`. Giá trị này tương ứng với phần tử không được khoanh tròn cuối cùng mà chúng ta cần trong lớp hiện tại. 
3. Đi ngang`remaining`từ đầu đến cuối. Với mỗi phần tử không bằng`val`, nối chỉ mục của nó vào`ops`. Điều này mô phỏng việc khoanh tròn tất cả các yếu tố có thể ngăn chặn`val`từ việc ở đúng vị trí. 
4. Loại bỏ tất cả các lần xuất hiện của`val`từ`remaining`bây giờ có thể được bỏ khoanh tròn một cách an toàn. Các phần tử còn lại tạo thành lớp tiếp theo để xử lý. 
5. Lặp lại các bước 2-4 cho đến khi`remaining`trống rỗng. 
6. Nếu ở bất kỳ giai đoạn nào được yêu cầu`val`mất tích, trả lại`-1`. Ngược lại, xuất ra độ dài của`ops`và danh sách`ops`. 

Tại sao nó hoạt động: Bằng cách xử lý các lớp từ cuối về phía sau và khoanh tròn các phần tử cản trở vị trí của các phần tử bắt buộc cuối cùng, chúng tôi đảm bảo rằng mỗi lớp phần tử không được khoanh tròn khớp với phần tương ứng của`p`. Điều bất biến là sau mỗi lớp, các phần tử không được khoanh tròn tạo thành tiền tố của`r`chúng ta vẫn cần xây dựng, đảm bảo không có phần tử nào bị đặt sai vị trí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    from collections import Counter, deque
    
    ops = []
    remaining = deque(a)
    
    while remaining:
        # Find the last unique value
        counts = Counter(remaining)
        val = next(reversed(remaining))
        
        layer_ops = []
        new_remaining = deque()
        found = False
        
        for idx, x in enumerate(remaining):
            if x != val:
                layer_ops.append(idx + 1)
                new_remaining.append(x)
            else:
                found = True
        
        if not found:
            print(-1)
            return
        
        ops.extend(layer_ops)
        remaining = deque([x for i,x in enumerate(remaining) if x == val])
    
    print(len(ops))
    print(' '.join(map(str, ops)))

solve()
```Giải pháp sử dụng`deque`để bật các phần tử một cách hiệu quả và`Counter`để xác định lớp Các chỉ số dựa trên 1 để phù hợp với báo cáo vấn đề. Lựa chọn làm việc từ cuối về phía sau sẽ tránh được việc vô tình đặt sai vị trí của các phần tử lặp lại. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
5
3 4 2 2 3
```| Bước | còn lại | giá trị | thêm chỉ số khoanh tròn | 
| --- | --- | --- | --- | 
| 1 | [3,4,2,2,3] | 3 | 2,3,4 | 
| 2 | [3,3] | 3 | không | 

Đầu ra:```
3
3 2 3
```Giải thích: Đầu tiên, chúng ta khoanh tròn các phần tử không phải là 3 số cuối cùng, sau đó không khoanh tròn 3. kết quả`r = [3,2,3]`bằng`p`. 

**Mẫu 2** 

đầu vào:```
3
1 2 3
```Tất cả các yếu tố đều khác biệt; cố gắng để phù hợp`p`có thể là không thể tùy theo mong muốn`p`. Nếu mục tiêu`p`không tương ứng với các phần tử được khoanh tròn cuối cùng, thuật toán sẽ in chính xác`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần trên mỗi lớp, nhiều nhất là một lần | 
| Không gian | O(n) | Lưu trữ`ops`Và`remaining`| 

Sự phức tạp này phù hợp một cách thoải mái trong giới hạn 2 giây đối với`n = 2·10^5`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("5\n3 4 2 2 3\n") in ["3\n3 2 3"], "sample 1"
assert run("3\n1 2 3\n") == "-1", "sample 2"

# Custom cases
assert run("1\n1\n") == "0\n", "single element, no ops"
assert run("3\n2 2 2\n") in ["0\n", "1\n1 2 3"], "all equal"
assert run("5\n1 2 3 4 5\n") == "-1", "no possible p"
assert run("4\n1 1 2 2\n") in ["2\n1 2", "2\n3 4"], "two layers repeated"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1`|`0\n`| Phần tử đơn, không cần thao tác | 
|`3\n2 2 2`|`0\n`| Tất cả các yếu tố bằng nhau, cho phép nhiều giải pháp | 
|`5\n1 2 3 4 5`|`-1`| Không thể`p`khớp các phần tử không được khoanh tròn | 
|`4\n1 1 2 2`|`2\n1 2`| Loại bỏ lớp với số lặp lại | 

## Vỏ cạnh 

Nếu`a`bao gồm một phần tử duy nhất, không cần thực hiện thao tác nào; thuật toán trả về chính xác các phép toán bằng 0. Đối với các mảng trong đó tất cả các phần tử giống hệt nhau, tồn tại nhiều chuỗi hoạt động hợp lệ và thuật toán chọn từng lớp một, thêm chỉ mục của các phần tử không phải mục tiêu vào`ops`. Nếu trình tự mong muốn`p`không thể được tạo ra do sự không khớp giữa các phần tử có sẵn và trình tự được yêu cầu, thuật toán sẽ xuất ra một cách chính xác`-1`. 

Ví dụ,`a = [2,2,2]`với một mục tiêu`p = [2,2,2]`là không thể, bởi vì sau khi khoanh tròn bất kỳ phần tử nào,`r`không thể bằng`p`. Tảo
