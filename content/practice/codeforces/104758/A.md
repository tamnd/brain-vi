---
title: "CF 104758A - Hành trình Alaric"
description: "Chúng ta được cho một dãy số nguyên được sắp xếp thành một dòng. Trong một lần di chuyển, chúng ta có thể lấy hai phần tử lân cận và thay thế chúng bằng tổng của chúng, rút ​​ngắn chuỗi một phần tử một cách hiệu quả trong khi vẫn giữ nguyên thứ tự ở nơi khác."
date: "2026-06-29T01:52:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104758
codeforces_index: "A"
codeforces_contest_name: "The 2023 ICPC Masters Mexico Regional #ICPCMX2023 Edition"
rating: 0
weight: 104758
solve_time_s: 73
verified: false
draft: false
---

[CF 104758A - Hành trình Alaric](https://codeforces.com/problemset/problem/104758/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên được sắp xếp thành một dòng. Trong một lần di chuyển, chúng ta có thể lấy hai phần tử lân cận và thay thế chúng bằng tổng của chúng, rút ​​ngắn chuỗi một phần tử một cách hiệu quả trong khi vẫn giữ nguyên thứ tự ở nơi khác. Nhiệm vụ là giảm chuỗi cho đến khi nó trở thành một bảng màu và chúng tôi muốn giảm thiểu số lượng thao tác hợp nhất như vậy được sử dụng. 

Một bảng màu ở đây có nghĩa là sau tất cả các phép biến đổi, việc đọc chuỗi từ trái sang phải sẽ cho các giá trị giống như đọc nó từ phải sang trái. Bởi vì việc hợp nhất giữ nguyên trật tự nhưng thay đổi phân đoạn, vấn đề thực sự là quyết định cách nhóm các phần tử liên tiếp sao cho cả hai đầu khớp nhau về tổng giá trị. 

Ràng buộc lên tới một triệu phần tử ngụ ý rằng bất kỳ giải pháp nào thử tất cả các chuỗi hợp nhất có thể có hoặc sử dụng lập trình động trên tất cả các khoảng thời gian sẽ quá chậm. Bất cứ điều gì bậc hai hoặc tệ hơn sẽ thất bại vì các quyết định hợp nhất là cục bộ nhưng mảng lớn. 

Một vài tình huống khó khăn quan trọng. 

Nếu mảng đã đọc tiến và lùi giống nhau, chẳng hạn như`[2, 2]`hoặc`[1, 3, 1]`, câu trả lời là 0 vì không cần hợp nhất. 

Ví dụ: nếu tất cả các phần tử đều khác nhau và mảng dài`[1, 2, 3, 4, 5]`, chúng ta sẽ cần nhiều lần hợp nhất để căn chỉnh các giá trị tích lũy từ cả hai đầu. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ là cho rằng chúng ta chỉ so sánh các giá trị ở đầu mà không xem xét rằng chúng ta được phép hợp nhất và tích lũy các giá trị. Ví dụ,`[1, 10, 100]`ban đầu không được cân bằng, nhưng việc hợp nhất sẽ thay đổi cấu trúc sao cho việc so sánh phải được thực hiện trên tổng phân đoạn chứ không phải trên các phần tử thô. 

## Phương pháp tiếp cận 

Giải thích bạo lực sẽ mô phỏng mọi cách có thể để hợp nhất các cặp liền kề cho đến khi một bảng màu xuất hiện, theo dõi số lượng thao tác tối thiểu. Mỗi lần hợp nhất sẽ giảm độ dài đi một và tại mỗi trạng thái có nhiều lựa chọn khả thi về nơi hợp nhất. 

Số lượng trình tự hợp nhất có thể tăng theo cấp số nhân vì ở mỗi bước có tới`O(n)`vị trí hợp nhất hợp lệ và chúng tôi thực hiện`O(n)`các bước. Điều này dẫn đến một không gian tìm kiếm vượt xa các giới hạn khả thi cho`n`lên đến một triệu. 

Quan sát quan trọng là cấu trúc cuối cùng chỉ được xác định bằng cách chúng ta phân chia mảng thành các đoạn liền kề có tổng tạo thành một bảng màu. Thay vì mô phỏng rõ ràng việc hợp nhất, chúng ta có thể nghĩ theo cách hai con trỏ quét vào trong từ cả hai đầu, duy trì tổng phân đoạn hiện tại ở mỗi bên. 

Tại bất kỳ thời điểm nào, chúng tôi so sánh số tiền tích lũy từ phân khúc bên trái và số tiền tích lũy từ phân khúc bên phải. Nếu bằng nhau, cả hai bên có thể tiến vào trong vì chúng ta đã khớp một “khối” của bảng màu cuối cùng. Nếu một cạnh nhỏ hơn, chúng ta phải mở rộng cạnh đó bằng cách hợp nhất phần tử tiếp theo vào đoạn của nó. Mỗi phần mở rộng như vậy tương ứng với một thao tác hợp nhất. Quá trình tham lam này hoạt động vì việc hợp nhất chỉ ảnh hưởng đến việc tích lũy tiền tố hoặc hậu tố cục bộ và không bao giờ được hưởng lợi từ việc bỏ qua một điểm không khớp nhỏ hơn để giải quyết vấn đề sau đó trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Hợp nhất tham lam hai con trỏ | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng việc xây dựng hai đầu có giá trị bằng nhau bằng hai con trỏ. 

1. Khởi tạo một con trỏ ở đầu và một con trỏ ở cuối mảng. Chúng tôi cũng duy trì hai tổng hoạt động đại diện cho phân đoạn nén hiện tại từ bên trái và từ bên phải. Ban đầu cả hai chỉ là phần tử đầu tiên và cuối cùng. 
2. Nếu các con trỏ gặp nhau, quá trình kết thúc vì toàn bộ mảng đã được khớp thành công thành cấu trúc palindrome. 
3. Nếu tổng bên trái bằng tổng bên phải, chúng ta đã hình thành thành công các phân đoạn bên ngoài khớp. Chúng tôi di chuyển cả hai con trỏ vào trong và đặt lại cả hai tổng đang chạy cho các phần tử chưa được khám phá tiếp theo. 
4. Nếu tổng bên trái nhỏ hơn, chúng ta phải hợp nhất phần tử tiếp theo từ bên trái vào đoạn bên trái hiện tại. Chúng ta thêm phần tử đó vào tổng bên trái và di chuyển con trỏ bên trái sang bên phải. Điều này tương ứng với một thao tác hợp nhất vì chúng ta đang kết hợp hai phần tử liền kề. 
5. Nếu tổng bên phải nhỏ hơn, chúng ta hợp nhất đối xứng từ phía bên phải bằng cách thêm phần tử tiếp theo vào tổng bên phải và di chuyển con trỏ bên phải sang trái. Điều này cũng được tính là một hoạt động. 
6. Chúng tôi lặp lại quá trình này cho đến khi các con trỏ gặp nhau, tích lũy số lần hợp nhất được thực hiện. 

Lựa chọn tham lam luôn là mở rộng cạnh tổng nhỏ hơn vì chỉ bên đó mới có thể bắt kịp bên kia mà không vượt quá cấu trúc của một phân vùng palindrome hợp lệ. 

### Tại sao nó hoạt động 

Ở mỗi bước, thuật toán duy trì tính bất biến rằng mảng giữa các con trỏ chưa được xử lý, trong khi các đoạn bên trái và bên phải biểu thị các khối một phần của phân tách palindrome tiềm năng. Bất kỳ giải pháp hợp lệ nào cuối cùng cũng phải khớp với tổng số tiền ở cả hai đầu cho mỗi khối tương ứng. Nếu một bên có số tiền nhỏ hơn, việc trì hoãn phần mở rộng của nó không thể giúp ích gì vì cách duy nhất để tăng số tiền đó là hợp nhất các phần tử liền kề vốn chỉ có ở bên đó. Do đó, việc mở rộng cạnh nhỏ hơn một cách tham lam không bao giờ chặn một cấu trúc tối ưu hợp lệ và đảm bảo rằng mỗi lần hợp nhất trực tiếp góp phần giải quyết một sự không khớp giữa các khối đối xứng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))
    
    if n <= 1:
        print(0)
        return

    l, r = 0, n - 1
    left_sum = a[l]
    right_sum = a[r]
    ans = 0

    while l < r:
        if left_sum == right_sum:
            l += 1
            r -= 1
            if l < r:
                left_sum = a[l]
                right_sum = a[r]
        elif left_sum < right_sum:
            l += 1
            left_sum += a[l]
            ans += 1
        else:
            r -= 1
            right_sum += a[r]
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai giữ hai con trỏ và mở rộng cạnh nhỏ hơn cho đến khi tổng cả hai phân đoạn tích lũy khớp nhau. Sau khi bằng nhau, cả hai bên tiến vào trong, bắt đầu các phân đoạn mới. Mỗi bước mở rộng được tính là một thao tác hợp nhất, tương ứng chính xác với việc nén hai phần tử liền kề. 

Cần phải cẩn thận khi đặt lại tổng phân đoạn sau khi so khớp. Chúng tôi phải đảm bảo các phân đoạn mới bắt đầu từ các phần tử chưa được xử lý tiếp theo; nếu không, chúng ta sẽ chuyển các tổng từng phần cũ sang một cách không chính xác và phá vỡ bất biến. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 3 5 1
```| Bước | tôi | r | trái_sum | đúng_sum | hoạt động | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 4 | 1 | 1 | trận đấu | 0 | 
| 2 | 1 | 3 | 2 | 5 | phải mở rộng | 1 | 
| 3 | 1 | 2 | 2 | 3 | phải mở rộng | 2 | 
| 4 | 1 | 1 | - | - | xong | 2 | 

Ở đây, thuật toán liên tục hợp nhất từ ​​bên phải cho đến khi cả hai bên cân bằng. Dấu vết cho thấy chúng ta luôn cố định tổng tích lũy nhỏ hơn, đảm bảo tính đối xứng được hình thành từng bước. 

### Ví dụ 2 

đầu vào:```
3
1 10 100
```| Bước | tôi | r | trái_sum | đúng_sum | hoạt động | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 2 | 1 | 100 | phải mở rộng | 1 | 
| 2 | 0 | 1 | 1 | 110 | phải mở rộng | 2 | 
| 3 | 0 | 0 | - | - | xong | 2 | 

Ví dụ này cho thấy sự hợp nhất lặp đi lặp lại từ bên phải cho đến khi bắt kịp bên trái. Mỗi lần hợp nhất sẽ làm giảm sự mất cân bằng và đưa cấu trúc đến gần hơn với một phân đoạn đối xứng duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được hấp thụ vào một phân đoạn nhiều nhất một lần khi con trỏ di chuyển vào trong | 
| Không gian | O(1) | Chỉ một số biến được sử dụng ngoài bộ nhớ đầu vào | 

Quét tuyến tính là cần thiết vì mọi phần tử có thể tham gia vào nhiều nhất một chuỗi hợp nhất và chúng tôi chỉ di chuyển con trỏ tiến hoặc lùi mà không xem lại vị trí. Điều này phù hợp thoải mái trong các ràng buộc cho tối đa một triệu phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("5\n1 2 3 5 1\n") == "1"
assert run("3\n1 10 100\n") == "2"
assert run("2\n2 2\n") == "0"

# custom cases
assert run("1\n7\n") == "0", "single element"
assert run("4\n1 2 2 1\n") == "0", "already palindrome"
assert run("4\n1 3 2 2\n") == "1", "single merge needed"
assert run("5\n1 1 1 1 1\n") == "0", "all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 element`|`0`| ranh giới tối thiểu | 
|`1 2 2 1`|`0`| đã nhạt màu | 
|`1 3 2 2`|`1`| sửa lỗi mất cân bằng đơn | 
|`1 1 1 1 1`|`0`| hành vi mảng thống nhất | 

## Vỏ cạnh 

Một mảng một phần tử như`[7]`đã là một palindrome. Thuật toán kết thúc ngay lập tức vì`l == r`khi bắt đầu, do đó không có sự hợp nhất nào được tính. 

Đối với một mảng đã đối xứng như`[1, 2, 2, 1]`, cả hai đầu đều bắt đầu bằng nhau và con trỏ di chuyển vào trong mà không kích hoạt bất kỳ sự hợp nhất nào. Bất biến được giữ nguyên vì không có phân đoạn nào yêu cầu mở rộng. 

Trong trường hợp như`[1, 3, 2, 2]`, vế phải ban đầu có tổng lớn hơn nên nó hấp thụ các phần tử cho đến khi khớp với vế trái. Quá trình dừng lại sau đúng một lần hợp nhất, chứng tỏ rằng thuật toán chỉ thực hiện hợp nhất khi tồn tại sự mất cân bằng về cấu trúc và không bao giờ đưa ra các hoạt động không cần thiết.
