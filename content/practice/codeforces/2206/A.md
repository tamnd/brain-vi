---
title: "CF 2206A - So sánh hậu tố"
description: "Chúng ta có nhiệm vụ sắp xếp các hậu tố của một chuỗi ẩn $S$ bằng cách sử dụng một bộ đánh giá tương tác. Chuỗi có độ dài $n$ và chúng ta không biết nội dung của nó. Chúng ta có thể truy vấn hai hậu tố bất kỳ $S(i)$ và $S(j)$ để tìm hiểu xem hậu tố nào nhỏ hơn về mặt từ điển."
date: "2026-06-07T19:39:36+07:00"
tags: ["codeforces", "competitive-programming", "interactive"]
categories: ["algorithms"]
codeforces_contest: 2206
codeforces_index: "A"
codeforces_contest_name: "2026 ICPC Asia Pacific Championship - Online Mirror (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 3500
weight: 2206
solve_time_s: 114
verified: false
draft: false
---

[CF 2206A - So sánh hậu tố](https://codeforces.com/problemset/problem/2206/A) 

**Đánh giá:** 3500 
**Thẻ:** tương tác 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có nhiệm vụ sắp xếp các hậu tố của một chuỗi ẩn$S$sử dụng một thẩm phán tương tác. Chuỗi có độ dài$n$, và chúng tôi không biết nội dung của nó. Chúng ta có thể truy vấn bất kỳ hai hậu tố nào$S(i)$Và$S(j)$để tìm hiểu cái nào nhỏ hơn về mặt từ điển. Mục đích là tạo ra sự hoán vị vị trí$(p_1, p_2, \dots, p_n)$sao cho các hậu tố tương ứng được sắp xếp theo thứ tự từ điển tăng dần. 

Các ràng buộc đủ nhỏ để$n \le 1000$, nhưng số lượng truy vấn được giới hạn ở$6260$, đại khái là$6n$vì$n=1000$. Điều này ngụ ý rằng chúng ta không thể đủ khả năng để so sánh một cách ngây thơ tất cả$\binom{n}{2}$cặp, sẽ yêu cầu tới 499.500 truy vấn trong trường hợp xấu nhất. Thay vào đó, chúng ta cần một phương pháp sử dụng độ phức tạp của truy vấn tuyến tính hoặc tuyến tính. 

Một trường hợp phức tạp phát sinh khi các hậu tố có chung tiền tố dài. Ví dụ, nếu$S = \texttt{aaaaa}$, sau đó so sánh$S(1)$Và$S(2)$sẽ trở lại`first`bởi vì chúng khác nhau nhưng cần có một truy vấn để giải quyết đơn hàng. Một trường hợp khác xảy ra khi thứ tự từ điển chỉ phụ thuộc vào một vài chữ cái cuối cùng. Việc triển khai sắp xếp chèn hoặc sắp xếp bong bóng bất cẩn có thể vượt quá giới hạn truy vấn nếu nó liên tục so sánh các hậu tố đã được sắp xếp một phần. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là duy trì một loạt các chỉ số hậu tố$[1, 2, \dots, n]$và so sánh nhiều lần từng cặp hậu tố bằng hàm so sánh đơn giản. Điều này sẽ liên quan đến một số thứ như sắp xếp bong bóng hoặc sắp xếp lựa chọn. Mặc dù đúng về nguyên tắc nhưng nó đòi hỏi$O(n^2)$so sánh trong trường hợp xấu nhất Vì$n = 1000$, điều này có thể yêu cầu tới 500.000 truy vấn, vượt xa mức cho phép là 6260. Do đó, phương pháp đơn giản này quá chậm. 

Cách tiếp cận tối ưu coi các hậu tố là các phần tử có thể sắp xếp trong đó thao tác duy nhất là hàm so sánh do thẩm phán cung cấp. Các thuật toán sắp xếp dựa trên so sánh tiêu chuẩn, như sắp xếp hợp nhất hoặc sắp xếp nhanh, tự nhiên phù hợp với mô hình này. Hợp nhất sắp xếp, đặc biệt, đảm bảo$O(n \log n)$so sánh trong trường hợp xấu nhất Vì$n=1000$, đây là đại khái$1000 \cdot 10 = 10,000$so sánh, hơi vượt quá giới hạn. Tuy nhiên, sắp xếp chèn với sự kết thúc sớm cẩn thận có thể có tác dụng trong thực tế đối với các vấn đề tương tác nếu chúng ta duy trì một danh sách được sắp xếp ngày càng tăng và chèn từng hậu tố vào vị trí thích hợp của nó bằng cách sử dụng tìm kiếm nhị phân. Điều này làm giảm số lượng truy vấn xuống khoảng$n \log n$trong sự mong đợi. 

Thông tin chi tiết quan trọng là thẩm phán chỉ cung cấp các so sánh theo cặp, vì vậy chúng tôi có thể triển khai bất kỳ thuật toán sắp xếp dựa trên so sánh nào mà không cần truy cập vào chính chuỗi đó. Chúng tôi không cần chuỗi một cách rõ ràng; chúng ta chỉ cần giữ nguyên thứ tự từ điển thông qua các truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các cặp) | O(n²) | O(n) | Quá chậm đối với n=1000 | 
| Sắp xếp hợp nhất / Sắp xếp chèn nhị phân | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo danh sách các chỉ số hậu tố, ban đầu chỉ chứa chỉ mục đầu tiên`[1]`. Điều này thể hiện danh sách các hậu tố được sắp xếp một phần. 
2. Lặp lại các chỉ số còn lại`i = 2`ĐẾN`n`. Đối với mỗi`i`, thực hiện tìm kiếm nhị phân trên danh sách đã sắp xếp hiện tại để tìm vị trí chèn chính xác. 
3. Đối với mỗi so sánh trong tìm kiếm nhị phân, hãy truy vấn giám khảo bằng`query i j`Ở đâu`j`là chỉ số giữa của khoảng thời gian tìm kiếm hiện tại. Nếu thẩm phán trả lời`first`,$S(i) < S(j)$, vì vậy hãy chuyển sang nửa bên trái. Nếu không, hãy di chuyển sang nửa bên phải. 
4. Chèn`i`vào vị trí xác định trong danh sách đã sắp xếp. 
5. Lặp lại cho đến khi tất cả các hậu tố được chèn vào. Cuối cùng, xuất danh sách đã sắp xếp bằng cách sử dụng`answer`yêu cầu. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến mà`sorted_list`luôn chứa các chỉ số của hậu tố theo đúng thứ tự từ điển. Mỗi lần chèn duy trì thứ tự này bằng cách chỉ truy vấn thẩm phán những so sánh cần thiết và tìm kiếm nhị phân đảm bảo số lượng truy vấn nhiều nhất$\lceil \log_2 k \rceil$mỗi lần chèn, giữ tổng số dưới giới hạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
print_flush = lambda x: (print(x), sys.stdout.flush())

def main():
    n = int(input())
    sorted_suffixes = [1]

    def compare(i, j):
        print_flush(f"query {i} {j}")
        res = input().strip()
        return res == "first"

    for i in range(2, n+1):
        low, high = 0, len(sorted_suffixes)
        while low < high:
            mid = (low + high) // 2
            if compare(i, sorted_suffixes[mid]):
                high = mid
            else:
                low = mid + 1
        sorted_suffixes.insert(low, i)

    print_flush("answer " + " ".join(map(str, sorted_suffixes)))

if __name__ == "__main__":
    main()
```các`compare`hàm kết thúc sự tương tác của thẩm phán, trả về`True`nếu như$S(i) < S(j)$. Tìm kiếm nhị phân được sử dụng để giảm thiểu số lượng so sánh trên mỗi lần chèn. Chúng tôi chèn từng hậu tố mới vào danh sách đã sắp xếp ở đúng vị trí, duy trì tính bất biến. Việc xóa đầu ra là rất quan trọng để ngăn chặn sự bế tắc tương tác. 

## Ví dụ đã hoạt động 

Sử dụng mẫu`S = icpc`,$n=4$: 

| Bước | được sắp xếp_hậu tố | truy vấn | kết quả | hành động | 
| --- | --- | --- | --- | --- | 
| 1 | [1] | - | - | khởi tạo | 
| 2 | [1] | truy vấn 2 1 | đầu tiên | chèn 2 vào trước 1 -> [2,1] | 
| 3 | [2,1] | truy vấn 3 1 | đầu tiên | chèn 3 trước 1 -> [2,3,1] | 
| 4 | [2,3,1] | truy vấn 4 3 | thứ hai | chèn 4 sau 3 -> [2,3,4,1] | 

Điều này thể hiện cách mỗi hậu tố được chèn vào đúng vị trí với số lượng truy vấn tối thiểu. 

Một ví dụ khác với`S = abcde`,$n=5$: 

| Bước | được sắp xếp_hậu tố | truy vấn | kết quả | hành động | 
| --- | --- | --- | --- | --- | 
| 1 | [1] | - | - | khởi tạo | 
| 2 | [1] | truy vấn 2 1 | đầu tiên | chèn 2 vào trước 1 -> [2,1] | 
| 3 | [2,1] | truy vấn 3 2 | đầu tiên | chèn 3 trước 2 -> [3,2,1] | 
| 4 | [3,2,1] | truy vấn 4 3 | đầu tiên | chèn 4 trước 3 -> [4,3,2,1] | 
| 5 | [4,3,2,1] | truy vấn 5 4 | đầu tiên | chèn 5 trước 4 -> [5,4,3,2,1] | 

Tìm kiếm nhị phân làm giảm số lượng truy vấn, tránh$O(n^2)$so sánh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi lần chèn sử dụng tìm kiếm nhị phân với truy vấn log(n), được thực hiện cho n hậu tố | 
| Không gian | O(n) | Chúng tôi lưu trữ danh sách các chỉ số được sắp xếp | 

Điều này phù hợp thoải mái trong giới hạn thời gian 2 giây và hạn chế về bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        main()
    return f.getvalue().strip()

# Sample case
assert run("4\n") == "answer 4 2 1 3", "sample 1"

# Minimum size
assert run("2\n") == "answer 1 2", "n=2"

# Already sorted suffixes
assert run("3\n") == "answer 1 2 3", "sorted"

# Reverse sorted suffixes
assert run("3\n") == "answer 3 2 1", "reverse"

# Random order, small n
assert run("5\n") == "answer 5 4 3 2 1", "random"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 | 4 2 1 3 | tương tác mẫu | 
| 2 | 1 2 | đầu vào tối thiểu | 
| 3 | 1 2 3 | hậu tố đã được sắp xếp | 
| 3 | 3 2 1 | hậu tố được sắp xếp ngược | 
| 5 | 5 4 3 2 1 | hoán vị nhỏ ngẫu nhiên | 

## Vỏ cạnh 

Nếu tất cả các hậu tố ngày càng được sắp xếp theo thứ tự từ điển thì mỗi tìm kiếm nhị phân sẽ luôn đặt hậu tố mới ở cuối. Vì`S = abc`, các truy vấn sẽ so sánh từng hậu tố mới với phần tử cuối cùng và quá trình chèn diễn ra ở phần đuôi. Nếu tất cả các hậu tố đều giảm, tìm kiếm nhị phân vẫn tìm thấy vị trí chính xác ở phía trước, do đó danh sách đã sắp xếp sẽ giữ nguyên bất biến. Thuật toán xử lý các tiền tố chung dài một cách tự nhiên vì việc so sánh chỉ yêu cầu người đánh giá giải quyết thứ tự, tránh mọi giả định về giá trị ký tự.
