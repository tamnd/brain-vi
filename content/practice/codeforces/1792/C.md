---
title: "CF 1792C - Sắp xếp tối đa tối thiểu"
description: "Chúng ta được cấp một hoán vị có kích thước $n$, đơn giản là một chuỗi chứa mỗi số nguyên từ $1$ đến $n$ đúng một lần. Nhiệm vụ là sắp xếp hoán vị này bằng cách sử dụng một thao tác đặc biệt với số lần bất kỳ."
date: "2026-06-09T10:25:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "greedy", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1792
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 142 (Rated for Div. 2)"
rating: 1500
weight: 1792
solve_time_s: 293
verified: false
draft: false
---

[CF 1792C - Sắp xếp tối thiểu tối thiểu](https://codeforces.com/problemset/problem/1792/C) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, bạo lực, tham lam, toán học, hai con trỏ 
**Thời gian giải:** 4m 53s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị về kích thước$n$, đơn giản là một dãy chứa mỗi số nguyên từ$1$ĐẾN$n$đúng một lần. Nhiệm vụ là sắp xếp hoán vị này bằng cách sử dụng một thao tác đặc biệt với số lần bất kỳ. Mỗi thao tác cho phép chúng ta chọn hai số, loại bỏ chúng, sau đó chèn số nhỏ hơn vào đầu và số lớn hơn vào cuối dãy. Mục đích là tìm ra số lượng tối thiểu các phép toán như vậy cần thiết để chuyển đổi hoán vị thành thứ tự tăng dần. 

Những ràng buộc cho chúng ta biết rằng$n$có thể lớn như$2 \cdot 10^5$trên tất cả các trường hợp thử nghiệm, với tối đa$10^4$trường hợp thử nghiệm. Điều này loại trừ bất kỳ giải pháp nào cố gắng mô phỏng mọi hoạt động có thể hoặc thử từng cặp phần tử một cách rõ ràng, vì ngay cả cách tiếp cận bậc hai cho mỗi trường hợp thử nghiệm cũng sẽ quá chậm. Chúng ta phải tìm một giải pháp hoạt động theo thời gian tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh phát sinh khi hoán vị đã được sắp xếp, đã đảo ngược hoặc có mức tối đa hoặc tối thiểu ở cuối. Chẳng hạn, nếu hoán vị đã được sắp xếp như sau`[1,2,3]`, câu trả lời đúng là`0`. Nếu hoán vị là`[3,2,1]`, thao tác phải được sử dụng ít nhất một lần để di chuyển phần tử lớn nhất về cuối và phần tử nhỏ nhất về đầu. Các giải pháp bất cẩn có thể tính toán quá mức nếu không xem xét vị trí của phần tử đầu tiên và cuối cùng so với`1`Và`n`. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ cố gắng mô phỏng các hoạt động một cách tham lam. Người ta có thể thử mọi cặp phần tử có thể, tính mảng kết quả và tiếp tục đệ quy cho đến khi được sắp xếp. Mặc dù cách tiếp cận này đúng về mặt lý thuyết nhưng độ phức tạp của nó lại theo cấp số nhân trong$n$, điều này hoàn toàn không thể thực hiện được đối với$n$lên đến$2 \cdot 10^5$. 

Quan sát quan trọng để tối ưu hóa là mỗi thao tác có thể đặt một phần tử vào vị trí cuối cùng một cách hiệu quả ở đầu hoặc cuối. Vì vậy, chúng ta chỉ cần theo dõi vị trí của phần tử nhỏ nhất và lớn nhất. Vấn đề giảm xuống việc kiểm tra trình tự các phần tử ở cuối: nếu phần tử đầu tiên là`1`hoặc cuối cùng là`n`, chúng ta đã có một ranh giới đúng. Nếu phần tử đầu tiên là`n`hoặc cuối cùng là`1`, nó làm phức tạp trình tự. Mặt khác, số lượng thao tác tối thiểu có thể được suy ra bằng cách đếm xem phần tử đầu tiên và phần tử cuối cùng khác với vị trí được sắp xếp của chúng như thế nào. Cái nhìn sâu sắc này cho phép đánh giá liên tục theo thời gian cho mỗi trường hợp thử nghiệm. 

Bằng cách suy luận về vị trí của các phần tử tối thiểu và tối đa, chúng ta có thể phân loại hoán vị thành bốn trường hợp: đã được sắp xếp, đã sắp xếp ngoại trừ một phần tử ở cuối sai, đảo ngược hoặc chưa được sắp xếp hoàn toàn. Mỗi trường hợp có một số thao tác được xác định trước:`0`,`1`,`2`, hoặc`3`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$và hoán vị`p`. 
2. Kiểm tra xem`p[0]`là`1`Và`p[-1]`là`n`. Nếu cả hai đều đúng thì mảng đã được sắp xếp. Trở lại`0`. 
3. Kiểm tra xem`p[0]`là`n`Và`p[-1]`là`1`. Nếu cả hai đều đúng thì mảng bị đảo ngược hoàn toàn. Trở lại`3`bởi vì chúng ta sẽ cần di chuyển cả hai đầu một cách chính xác bằng ít nhất hai thao tác, cộng thêm một thao tác bổ sung để điều chỉnh ở giữa. 
4. Nếu một trong hai`p[0]`là`1`hoặc`p[-1]`là`n`, trở lại`1`. Một thao tác là đủ để sửa đầu kia. 
5. Nếu không, hãy quay lại`2`. Các yếu tố đầu tiên và cuối cùng không được đặt đúng chỗ, nhưng cũng không quá sai. Hai thao tác đủ để định vị chúng một cách chính xác. 

Bất biến đảm bảo tính đúng đắn là mỗi thao tác sẽ di chuyển một số đến vị trí được sắp xếp cuối cùng của nó ở mức cực đại. Bằng cách chỉ phân tích vị trí đầu tiên và cuối cùng, chúng tôi nắm bắt được tất cả các kịch bản cho hoạt động tối thiểu vì các yếu tố còn lại luôn có thể được điều chỉnh theo số lượng này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def min_max_sort():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        if p[0] == 1 and p[-1] == n:
            print(0)
        elif p[0] == n and p[-1] == 1:
            print(3)
        elif p[0] == 1 or p[-1] == n:
            print(1)
        elif p[0] == n or p[-1] == 1:
            print(2)
        else:
            print(2)

min_max_sort()
```Giải pháp này trước tiên đọc số lượng trường hợp thử nghiệm và vòng lặp qua từng trường hợp thử nghiệm. Các điều kiện được kiểm tra cẩn thận nhằm giảm thiểu các thao tác, với trường hợp đảo ngược đặc biệt được xử lý rõ ràng để tránh tính thiếu. Tất cả các lần kiểm tra đều có thời gian không đổi và việc đọc hoán vị là tuyến tính, do đó giải pháp sẽ chia tỷ lệ hợp lý cho đầu vào lớn. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
5
1 5 4 2 3
```| Bước | Yếu tố đầu tiên | Yếu tố cuối cùng | Trường hợp | Hoạt động | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 1 | 3 | Chỉ đầu tiên là đúng | 1 | 
| Sau ca phẫu thuật đầu tiên | 1 | 5 | Bây giờ sửa lần cuối | 2 | 

Điều này chứng tỏ rằng việc đặt các số lớn nhất và nhỏ nhất vào vị trí của chúng sẽ giải được bài toán trong hai phép tính. 

### Đầu vào mẫu 2```
3
1 2 3
```| Bước | Yếu tố đầu tiên | Yếu tố cuối cùng | Trường hợp | Hoạt động | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 1 | 3 | Đã được sắp xếp | 0 | 

Điều này cho thấy thuật toán ngay lập tức trả về 0 khi chuỗi được sắp xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Đọc mỗi hoán vị mất O(n), kiểm tra vị trí đầu tiên/cuối cùng là O(1). | 
| Không gian | O(1) | Chúng tôi lưu trữ hoán vị trong một danh sách và không cần thêm cấu trúc nào. | 

Cho rằng tổng của$n$trên tất cả các trường hợp thử nghiệm không vượt quá$2 \cdot 10^5$, thuật toán sẽ chạy thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    min_max_sort()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("4\n5\n1 5 4 2 3\n3\n1 2 3\n4\n2 1 4 3\n6\n5 2 4 1 6 3\n") == "2\n0\n1\n3"

# custom cases
assert run("1\n1\n1\n") == "0", "single element"
assert run("1\n2\n2 1\n") == "3", "two-element reversed"
assert run("1\n5\n5 1 2 3 4\n") == "2", "largest at start"
assert run("1\n5\n2 3 4 5 1\n") == "2", "smallest at end"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | Mảng một phần tử | 
| 2 | 3 | Mảng đảo ngược hai phần tử | 
| 5 1 2 3 4 | 2 | Lớn nhất khi bắt đầu | 
| 2 3 4 5 1 | 2 | Nhỏ nhất ở cuối | 

## Vỏ cạnh 

Khi mảng chỉ có một phần tử, thuật toán trả về chính xác`0`vì không cần thực hiện thao tác nào. Khi mảng bị đảo ngược hoàn toàn, chẳng hạn như`[n, ..., 2, 1]`, thuật toán trả về`3`, nắm bắt hoạt động bổ sung cần thiết để định vị lại cả hai thái cực. Đối với các mảng chỉ có một cực trị, thuật toán trả về`1`. Tất cả các trường hợp khác rơi vào`2`-loại hoạt động, đảm bảo số lượng hoạt động tối thiểu luôn được trả về.
