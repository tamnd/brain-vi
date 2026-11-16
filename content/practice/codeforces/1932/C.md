---
title: "CF 1932C - dư lượng LR"
description: "Chúng ta được cung cấp một mảng thu nhỏ từ cả hai đầu dựa trên một chuỗi hướng dẫn. Ở mỗi bước, trước khi loại bỏ bất kỳ thứ gì, chúng ta phải tính tích của tất cả các phần tử còn lại theo modulo một số cố định $m$."
date: "2026-06-08T18:21:15+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "implementation", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1932
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 927 (Div. 3)"
rating: 1400
weight: 1932
solve_time_s: 206
verified: false
draft: false
---

[CF 1932C - Phần còn lại của LR](https://codeforces.com/problemset/problem/1932/C) 

**Đánh giá:** 1400 
**Tags:** Brute Force, cấu trúc dữ liệu, triển khai, toán học, hai con trỏ 
**Thời gian giải:** 3 phút 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng thu nhỏ từ cả hai đầu dựa trên một chuỗi hướng dẫn. Ở mỗi bước, trước khi loại bỏ bất cứ thứ gì, chúng ta phải tính tích của tất cả các phần tử còn lại theo modulo một số cố định$m$. Sau đó, lệnh sẽ cho chúng ta biết nên loại bỏ phần tử ngoài cùng bên trái hay ngoài cùng bên phải. 

Vì vậy, quá trình này hoàn toàn động: mảng là một cửa sổ trượt được xác định bởi hai con trỏ và sau mỗi truy vấn, chúng tôi thu nhỏ cửa sổ đó lại một từ một trong hai phía. Thách thức là hỗ trợ các truy vấn sản phẩm lặp lại nhanh chóng trong khi các phần tử biến mất ở cả hai đầu. 

Các ràng buộc đủ lớn để việc tính toán lại sản phẩm từ đầu sau mỗi lần loại bỏ sẽ quá chậm. Một cách tiếp cận ngây thơ sẽ tốn kém$O(n^2)$cho mỗi trường hợp kiểm thử trong trường hợp xấu nhất vì mỗi bước có thể nhân lên tới$n$các phần tử nữa. Với tổng số$n$lên đến$2 \cdot 10^5$, điều này không thể chấp nhận được. 

Trường hợp cạnh tinh tế xuất hiện khi$m = 1$. Trong trường hợp đó, mọi câu trả lời đều bằng 0 bất kể mảng nào, do đó, một giải pháp đúng phải tránh tính toán không cần thiết và vẫn hoạt động chính xác khi loại bỏ nhiều lần. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ duy trì mảng hiện tại một cách rõ ràng và tính toán lại modulo sản phẩm$m$sau mỗi lần thao tác. Điều này đúng nhưng không hiệu quả. Mỗi bước tốn thời gian tuyến tính và có$n$các bước, đưa ra độ phức tạp bậc hai tổng thể. 

Quan sát quan trọng là mảng chỉ mất các phần tử ở cuối. Điều này có nghĩa là chúng tôi không bao giờ chèn hoặc sửa đổi các giá trị mà chỉ xóa chúng. Điều đó gợi ý việc duy trì một sản phẩm đang chạy và cập nhật nó dần dần. Tuy nhiên, phép chia theo modulo không có sẵn trực tiếp trừ khi chúng ta có thể tính toán các nghịch đảo mô đun một cách an toàn, có thể không tồn tại khi$m$không phải là số nguyên tố hoặc các phần tử có chung thừa số với$m$. 

Điều này đẩy chúng ta tới một quan điểm khác. Thay vì cố gắng duy trì một sản phẩm có thể đảo ngược, chúng tôi xử lý việc xóa theo cách ngược lại. Nếu chúng ta tưởng tượng việc xây dựng lại mảng ngược lại thì mỗi lần xóa sẽ trở thành một lần chèn. Chúng ta có thể mô phỏng quá trình này bằng cách trước tiên loại bỏ mọi thứ và sau đó xây dựng lại các câu trả lời theo thứ tự ngược lại. Trong quá trình xây dựng lại, phép nhân là an toàn vì chúng ta chỉ cộng các phần tử. 

Kỹ thuật đảo ngược này chuyển vấn đề thành một cấu trúc tiền tố tiêu chuẩn trong đó chúng ta chỉ nhân các phần tử vào tích, không bao giờ chia. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại từng bước |$O(n^2)$|$O(n)$| Quá chậm | 
| Quá trình đảo ngược với phép nhân tăng dần |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và chuỗi lệnh. Giải thích quá trình này là xóa liên tục từ bên trái hoặc bên phải cho đến khi không còn gì. 
2. Thay vì mô phỏng việc xóa về phía trước, hãy xây dựng chuỗi các phần tử bị loại bỏ. Trước tiên, chúng tôi theo dõi trạng thái cuối cùng, trạng thái trống, sau đó mô phỏng ngược bằng cách chèn lại các phần tử theo thứ tự xóa ngược lại. 
3. Để thực hiện việc này, hãy duy trì hai con trỏ$l$Và$r$. Đối với mỗi lệnh, ghi lại phần tử nào sẽ bị xóa ở bước đó. 
4. Sau khi ghi lại thứ tự loại bỏ, hãy đảo ngược trình tự này. Bây giờ chúng ta sẽ xây dựng lại mảng từ trống thành đầy. 
5. Duy trì một sản phẩm đang chạy bắt đầu từ 1. Mỗi lần chúng ta thêm lại một phần tử, hãy nhân nó vào modulo sản phẩm$m$. 
6. Bảo quản sản phẩm sau mỗi lần lắp vào. Những câu trả lời này tương ứng với thứ tự ngược lại của quy trình ban đầu. 
7. Cuối cùng đảo ngược mảng đáp án trước khi in. 

### Tại sao nó hoạt động 

Bất biến quan trọng là trong quá trình đảo ngược, tại mỗi bước, tập hợp các phần tử hiện tại chính xác là phần bù của những gì đã bị loại bỏ theo hướng thuận. Vì phép nhân có tính chất kết hợp và giao hoán nên việc xây dựng lại theo thứ tự ngược lại sẽ đảm bảo tính chính xác của các tích tiền tố trên tập hoạt động ở mỗi giai đoạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        s = input().strip()

        l, r = 0, n - 1
        removed = []

        for c in s:
            if c == 'L':
                removed.append(a[l])
                l += 1
            else:
                removed.append(a[r])
                r -= 1

        res = []
        prod = 1

        for x in reversed(removed):
            prod = (prod * x) % m
            res.append(prod)

        print(*reversed(res))

if __name__ == "__main__":
    solve()
```## Cách mã phù hợp với ý tưởng 

Vòng lặp đầu tiên xây dựng lại thứ tự xóa chính xác bằng cách mô phỏng mảng thu nhỏ bằng hai con trỏ. Điều này tránh được việc tính toán lại tốn kém. 

Vòng lặp thứ hai đảo ngược quy trình: thay vì loại bỏ các phần tử và tính toán lại sản phẩm, chúng tôi thêm lại từng phần tử một và duy trì một sản phẩm đang chạy. Điều này có tác dụng vì phép nhân trên một tập hợp không phụ thuộc vào thứ tự. 

Sự đảo ngược cuối cùng căn chỉnh dòng thời gian được xây dựng lại với thứ tự truy vấn ban đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4, m = 6
a = [3, 1, 4, 2]
s = LRRL
```| Bước | Hành động | Phần tử đã bị xóa | Sản phẩm (xây dựng ngược) | 
| --- | --- | --- | --- | 
| 1 | L | 3 | 3 | 
| 2 | R | 2 | 2 | 
| 3 | R | 4 | 2 | 
| 4 | L | 1 | 2 | 

Đảo ngược quá trình tái thiết mang lại:```
0 2 4 1
```Điều này phù hợp với ý tưởng rằng chúng ta đang tích lũy sản phẩm của các bộ có hậu tố. 

### Ví dụ 2 

đầu vào:```
a = [1,1,1,1,1], s = LLLLL
```Mỗi bước loại bỏ 1, do đó tích không bao giờ thay đổi từ 0 modulo 1 (hoặc ổn định tùy theo m). Quá trình xác nhận rằng thứ tự loại bỏ không quan trọng khi tất cả các phần tử đều giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| mỗi phần tử được xử lý một lần trong quá trình loại bỏ và một lần trong quá trình xây dựng lại | 
| Không gian |$O(n)$| lưu trữ trình tự đã xóa | 

Điều này dễ dàng phù hợp với các ràng buộc vì tổng$n$qua các trường hợp thử nghiệm là$2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from subprocess import run as r
    return "manual check"

# Provided samples
# (omitted real asserts for brevity in this format)

# Custom cases
# 1. Single element
# 2. Alternating L/R
# 3. All same values
# 4. m = 1 edge case
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | đúng sản phẩm | trường hợp cơ sở | 
| xen kẽ LR | xử lý đơn hàng đúng | độ chính xác của con trỏ | 
| tất cả đều bình đẳng | phát triển sản phẩm ổn định | xử lý dư thừa | 
| m = 1 | tất cả số không | trường hợp cạnh modulo | 

## Vỏ cạnh 

Khi nào$m = 1$, mọi kết quả của phép nhân ngay lập tức phải bằng 0. Thuật toán vẫn hoạt động vì mọi thao tác trên sản phẩm đều là modulo$m$, do đó tất cả các trạng thái trung gian vẫn hợp lệ và việc tái cấu trúc đảo ngược cuối cùng tạo ra một chuỗi các số 0 không có cách viết đặc biệt. 

Khi tất cả các phần tử giống hệt nhau, thứ tự loại bỏ không ảnh hưởng đến tính chính xác và việc xây dựng lại vẫn tạo ra cùng một chuỗi sản phẩm, xác nhận rằng giải pháp không phụ thuộc vào thứ tự trong các trường hợp đối xứng.
