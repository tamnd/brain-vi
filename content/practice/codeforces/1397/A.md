---
title: "CF 1397A - Trò chơi chữ tung hứng"
description: "Chúng ta được cung cấp một tập hợp các chuỗi và chúng ta có thể di chuyển các ký tự một cách tự do giữa bất kỳ chuỗi nào, bao gồm cả việc di chuyển một ký tự từ một chuỗi trở lại chính chuỗi đó. Mục đích là để xác định xem có thể sắp xếp lại tất cả các ký tự sao cho mọi chuỗi đều giống hệt nhau hay không."
date: "2026-06-11T09:19:22+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1397
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 666 (Div. 2)"
rating: 800
weight: 1397
solve_time_s: 105
verified: true
draft: false
---

[CF 1397A - Thư tung hứng](https://codeforces.com/problemset/problem/1397/A) 

**Đánh giá:** 800 
**Tags:** tham lam, dây 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các chuỗi và chúng ta có thể di chuyển các ký tự một cách tự do giữa bất kỳ chuỗi nào, bao gồm cả việc di chuyển một ký tự từ một chuỗi trở lại chính chuỗi đó. Mục đích là để xác định xem có thể sắp xếp lại tất cả các ký tự sao cho mọi chuỗi đều giống hệt nhau hay không. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chỉ định số lượng chuỗi và chính các chuỗi đó. Đầu ra là "CÓ" hoặc "KHÔNG" đơn giản tùy thuộc vào việc có thể sắp xếp lại hay không. 

Những hạn chế là vừa phải. Mỗi trường hợp thử nghiệm có thể có tối đa 1000 chuỗi và mỗi chuỗi có thể dài tối đa 1000 ký tự, nhưng tổng của tất cả các ký tự trong tất cả các trường hợp thử nghiệm được giới hạn ở mức 1000. Điều này có nghĩa là chúng tôi có thể cung cấp các thuật toán tuyến tính theo tổng số ký tự thay vì trên mỗi chuỗi hoặc mỗi trường hợp thử nghiệm. 

Một sự tinh tế quan trọng nảy sinh khi các ký tự được phân bố không đồng đều. Ví dụ: nếu chúng ta có ba chuỗi "a", "a" và "b", tổng số của 'a' là 2 và 'b' là 1. Không có cách nào để tạo ba chuỗi giống hệt nhau vì cuối cùng mỗi chuỗi phải có cùng số lượng mỗi chữ cái. Việc triển khai bất cẩn chỉ có thể kiểm tra xem tất cả các chuỗi có chứa cùng một bộ ký tự mà không tính đến tần số hay không, tạo ra kết quả "CÓ" sai trong trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là mô phỏng các ký tự chuyển động giữa các chuỗi cho đến khi tất cả chúng khớp nhau. Ví dụ, người ta có thể thử chuyển đổi mọi chuỗi để khớp với chuỗi đầu tiên bằng cách di chuyển liên tục các ký tự. Về mặt lý thuyết, điều này đúng nhưng không khả thi: việc di chuyển từng ký tự riêng lẻ có thể mất tới 1000 lần di chuyển cho mỗi ký tự trên mỗi chuỗi, dẫn đến số lượng thao tác trong trường hợp xấu nhất lên tới hàng triệu hoặc nhiều hơn, điều này là không cần thiết do các hạn chế của vấn đề. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng thứ tự các ký tự không quan trọng. Bất kỳ ký tự nào cũng có thể được chèn vào bất kỳ đâu trong bất kỳ chuỗi nào. Điều này làm giảm vấn đề kiểm tra tổng số ký tự trên tất cả các chuỗi. Để có giải pháp hợp lệ, tổng số ký tự phải chia hết cho số chuỗi. Nếu không, không thể phân bổ đều ký tự đó cho tất cả các chuỗi và do đó không thể làm cho tất cả các chuỗi giống hệt nhau. 

Điều này biến một vấn đề mô phỏng phức tạp thành một vấn đề đếm đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng_chars * n) | O(tổng_chars) | Quá chậm | 
| Tối ưu | O(tổng_chars) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm cho mỗi chữ cái trong bảng chữ cái, biểu thị tổng số chữ cái đó trên tất cả các chuỗi trong trường hợp kiểm thử hiện tại. Điều này được thực hiện bằng cách sử dụng một mảng có kích thước 26 được lập chỉ mục bởi các chữ cái 'a' đến 'z'. 
2. Lặp lại từng chuỗi. Đối với mỗi ký tự trong chuỗi, hãy tăng bộ đếm tương ứng. Khi kết thúc lần lặp này, bộ đếm giữ tổng số lần xuất hiện của từng ký tự trên tất cả các chuỗi. 
3. Kiểm tra từng giá trị bộ đếm. Nếu mọi bộ đếm đều chia hết cho số chuỗi`n`, có thể phân bố lại các ký tự một cách đồng đều nên in "CÓ". Nếu bất kỳ bộ đếm nào không chia hết cho`n`, in ngay "KHÔNG". Điều này có tác dụng vì tính chia hết đảm bảo rằng mọi chuỗi đều có thể nhận được số lượng chính xác của mỗi ký tự. 

Tại sao nó hoạt động: điều bất biến là việc phân phối lại các ký tự không bị ràng buộc; do đó, yêu cầu duy nhất cho sự bình đẳng là mỗi chuỗi nhận được một phần bằng nhau của mỗi chữ cái. Nếu tổng số chia hết cho số chuỗi thì có sự phân phối lại hợp lệ; nếu không thì không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    counts = [0] * 26  # total counts of 'a' to 'z'
    
    for _ in range(n):
        s = input().strip()
        for ch in s:
            counts[ord(ch) - ord('a')] += 1
    
    possible = all(count % n == 0 for count in counts)
    print("YES" if possible else "NO")
```Giải pháp đọc số lượng trường hợp kiểm thử và xử lý từng trường hợp kiểm thử một cách độc lập. Đối với mỗi trường hợp thử nghiệm, nó đếm số lần xuất hiện của mỗi chữ cái. Việc sử dụng toán tử modulo đảm bảo rằng mỗi ký tự có thể được phân bổ đồng đều. Việc lựa chọn mảng có kích thước 26 là an toàn và hiệu quả, tránh được chi phí sử dụng từ điển. Việc loại bỏ đầu vào là cần thiết để loại bỏ các dòng mới ở cuối. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chuỗi đầu vào: "caa", "cbb" 

| Thư | Tổng số | Chia hết cho 2? | 
| --- | --- | --- | 
| một | 2 | Có | 
| b | 2 | Có | 
| c | 2 | Có | 

Tất cả số đếm chia hết cho 2, đầu ra: CÓ. Các ký tự có thể được sắp xếp lại để tạo thành "cab" trong cả hai chuỗi. 

### Ví dụ 2 

Chuỗi đầu vào: "cba", "cba", "cbb" 

| Thư | Tổng số | Chia hết cho 3? | 
| --- | --- | --- | 
| một | 2 | Không | 
| b | 4 | Không | 
| c | 3 | Có | 

Một số số đếm không chia hết cho 3, đầu ra: KHÔNG. 

Điều này xác nhận thuật toán xác định chính xác khi không thể phân phối lại do số lượng ký tự không thể phân chia. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng_chars) | Chúng tôi lặp lại tất cả các ký tự chính xác một lần cho mỗi trường hợp thử nghiệm | 
| Không gian | O(26) | Mảng có kích thước cố định để đếm chữ cái | 

Với tổng của tất cả độ dài chuỗi ≤ 1000, điều này hoạt động tốt trong giới hạn thời gian. Không gian thật tầm thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution
    t = int(input())
    for _ in range(t):
        n = int(input())
        counts = [0] * 26
        for _ in range(n):
            s = input().strip()
            for ch in s:
                counts[ord(ch) - ord('a')] += 1
        possible = all(count % n == 0 for count in counts)
        print("YES" if possible else "NO")
    return output.getvalue().strip()

# Provided samples
assert run("4\n2\ncaa\ncbb\n3\ncba\ncba\ncbb\n4\nccab\ncbac\nbca\nacbcc\n4\nacb\ncaf\nc\ncbafc\n") == "YES\nNO\nYES\nNO"

# Custom test cases
assert run("1\n1\na") == "YES", "single string, always YES"
assert run("1\n2\na\na") == "YES", "already equal strings"
assert run("1\n3\na\nb\nc") == "NO", "distinct single characters cannot be equal"
assert run("1\n3\naaa\naaa\naaa") == "YES", "all equal large strings"
assert run("1\n2\nabc\ndef") == "NO", "cannot redistribute to make equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 chuỗi "a" | CÓ | Trường hợp đầu vào tối thiểu | 
| 2 chuỗi "a", "a" | CÓ | Đã có chuỗi bằng nhau | 
| 3 chuỗi "a", "b", "c" | KHÔNG | Các ký tự đơn không thể phân phối lại | 
| 3 chuỗi "aaa", "aaa", "aaa" | CÓ | Tất cả các chuỗi bằng nhau có nhiều chữ cái | 
| 2 chuỗi "abc", "def" | KHÔNG | Không có chữ cái thông thường, không thể | 

## Vỏ cạnh 

Trường hợp có một chuỗi luôn xuất ra CÓ. Nếu tổng số ký tự của một chữ cái chia hết cho số chuỗi thì thuật toán sẽ xử lý chính xác các chuỗi có độ dài khác nhau. Ví dụ: "abc", "a" cho số lượng a=2, b=1, c=1. Với n=2, b và c không chia hết cho 2, kết quả: NO. Điều này xác nhận thuật toán tính toán chính xác cho cả phân phối ký tự và số lượng chuỗi.
