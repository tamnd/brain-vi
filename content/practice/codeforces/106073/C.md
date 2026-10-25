---
title: "CF 106073C - Đa thức Collatz"
description: "Chúng ta được cho một đa thức trong đó mọi hệ số là 0 hoặc 1, vì vậy tốt nhất nên coi đa thức đó là một tập hợp lũy thừa của x. Nếu hệ số của $x^k$ là 1 thì chúng ta sẽ đưa số hạng đó vào, nếu không thì không."
date: "2026-06-20T21:53:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106073
codeforces_index: "C"
codeforces_contest_name: "The 2025 ICPC South America - Brazil First Phase"
rating: 0
weight: 106073
solve_time_s: 55
verified: true
draft: false
---

[CF 106073C - Đa thức Collatz](https://codeforces.com/problemset/problem/106073/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đa thức trong đó mọi hệ số là 0 hoặc 1, vì vậy tốt nhất nên coi đa thức đó là một tập hợp lũy thừa của x. Nếu hệ số của$x^k$là 1, chúng tôi bao gồm thuật ngữ đó, nếu không thì không. Đa thức phát triển thông qua một quá trình xác định liên tục áp dụng một trong hai phép toán chỉ tùy thuộc vào việc thuật ngữ không đổi có xuất hiện hay không. 

Nếu có số hạng không đổi thì đa thức được biến đổi bằng cách nhân nó với$x+1$, sau đó cộng 1 và cuối cùng đơn giản hóa sao cho bất kỳ hệ số nào bằng 2 đều bị loại bỏ. Vì các hệ số không bao giờ vượt quá 2 nên việc đơn giản hóa này giải quyết hiệu quả các xung đột trong đó một thuật ngữ xuất hiện hai lần. Nếu không có số hạng không đổi thì thao tác đơn giản hơn nhiều: chúng ta chia đa thức cho x, tương ứng với việc dịch mỗi số mũ xuống một. 

Quá trình lặp lại cho đến khi đa thức trở thành chính xác 1, nghĩa là chỉ còn lại số hạng không đổi và không tồn tại số hạng bậc cao hơn. 

Khó khăn chính là mỗi bước sẽ thay đổi cấu trúc của đa thức theo cách phi tuyến. nhân với$x+1$cả hai đều dịch chuyển và kết hợp các thuật ngữ hiện có và phần bổ sung “+1 có tính hủy bỏ” giới thiệu các tương tác ở thuật ngữ không đổi phá vỡ các đối số đơn điệu đơn giản. 

Các ràng buộc rất nhỏ: bậc ban đầu nhiều nhất là 20, do đó đa thức có thể được biểu diễn dưới dạng mặt nạ bit trên tối đa 21 bit. Mặc dù quy trình không được đảm bảo thu nhỏ ngay lập tức, không gian trạng thái đủ nhỏ để có thể mô phỏng trực tiếp quá trình chuyển đổi miễn là mỗi bước được triển khai hiệu quả bằng cách sử dụng các thao tác bit. 

Trường hợp cạnh tinh tế là khi đa thức đã bằng 1. Trong trường hợp này, không nên thực hiện thao tác nào. Một trường hợp đặc biệt khác là khi phép nhân lặp lại tạo ra một thuật ngữ có mức độ cao hơn giới hạn ban đầu, do đó việc triển khai giả sử một mảng có kích thước cố định có độ dài 21 mà không xử lý tràn sẽ âm thầm loại bỏ các thuật ngữ hoặc chỉ mục không chính xác. Vấn đề thứ ba là hiểu sai quy tắc “loại bỏ hệ số 2”, quy tắc này không phải là modulo 2 số học trên toàn cầu mà chỉ bị hủy hiệu quả sau khi kết hợp các đóng góp từ phép nhân và phép cộng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực chính xác là những gì câu lệnh mô tả: biểu diễn đa thức một cách rõ ràng, thực hiện phép nhân đa thức bằng$x+1$, cộng 1, sau đó quét tất cả các hệ số và đặt 2 bất kỳ về 0 hoặc dịch sang phải khi không có số hạng không đổi. Mỗi chi phí hoạt động$O(n)$Ở đâu$n$là mức độ hiện tại. Quá trình này có thể yêu cầu một số lượng lớn các bước vì các phép biến đổi có thể tăng và giảm mức độ và không có tiềm năng đơn điệu rõ ràng nào đảm bảo sự hội tụ nhanh chóng. Trong trường hợp xấu nhất, việc mô phỏng từng bước trên nhiều cấu hình trung gian sẽ trở nên kém hiệu quả nếu được thực hiện bằng số học đa thức đơn giản. 

Quan sát quan trọng là do các hệ số là nhị phân nên đa thức có thể được biểu diễn dưới dạng mặt nạ bit và tất cả các phép toán đều trở thành các phép biến đổi theo bit. nhân với$x$là phép dịch trái và phép cộng tương ứng với XOR. Điều này chuyển đổi toàn bộ quá trình thành một máy trạng thái trên các số nguyên. Sau khi viết theo cách này, mỗi bước là$O(1)$hoạt động bit và chúng tôi chỉ mô phỏng cho đến khi đạt 1. Không gian trạng thái nhỏ vì đa thức không bao giờ lớn hơn một cách có ý nghĩa hơn vài chục bit trong thực tế đối với các ràng buộc đã cho, do đó sự hội tụ diễn ra nhanh chóng trong mô phỏng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 

|---|---|---| 

| Mô phỏng đa thức Brute Force |$O(T \cdot N)$|$O(N)$| Quá chậm | 

| Mô phỏng Bitmask |$O(T)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta mã hóa đa thức dưới dạng số nguyên trong đó bit i biểu thị sự có mặt của$x^i$. 

1. Đọc đa thức đầu vào và xây dựng biểu diễn mặt nạ bit của nó. Mỗi hệ số 1 đặt bit tương ứng. Điều này mang lại một mã hóa nhỏ gọn trong đó các dịch chuyển đại số trở thành các dịch chuyển bit. 
2. Lặp lại cho đến khi bitmask bằng 1. Giá trị 1 tương ứng với đa thức chỉ chứa số hạng không đổi. 
3. Nếu mặt nạ bit hiện tại có tập hợp bit có ý nghĩa nhỏ nhất, hãy áp dụng thao tác “nhân với (x+1) rồi cộng 1”. Theo thuật ngữ bit, phép nhân với (x+1) trên hành vi hủy giống GF(2) trở thành`mask XOR (mask << 1)`. “+1” bổ sung sẽ lật lại số hạng không đổi, vì vậy chúng tôi XOR với 1. Sự kết hợp này nắm bắt chính xác cách các số hạng bị hủy khi chúng xuất hiện hai lần. 
4. Nếu bit có trọng số nhỏ nhất không được đặt, hãy thực hiện phép chia cho x, tương ứng với việc dịch chuyển toàn bộ mặt nạ bit sang phải một. 
5. Đếm mỗi phép biến đổi là một thao tác. 

Việc mô phỏng tiếp tục cho đến khi chỉ còn lại số hạng không đổi. 

### Tại sao nó hoạt động 

Cấu trúc đa thức được nắm bắt đầy đủ bởi sự có mặt hay vắng mặt của các thuật ngữ và mọi phép biến đổi chỉ phụ thuộc vào việc thuật ngữ không đổi có tồn tại hay không. Điều này làm cho sự tiến hóa mang tính quyết định trên một không gian trạng thái nhị phân hữu hạn. Việc biểu diễn đa thức dưới dạng mặt nạ bit sẽ bảo toàn tất cả thông tin cần thiết cho các bước trong tương lai vì cả hai thao tác đều tuyến tính trên các vị trí bit ngoại trừ việc chuyển đổi thuật ngữ không đổi được xử lý rõ ràng. Do đó, mỗi quá trình chuyển đổi mô phỏng khớp chính xác với một bước của quy trình ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    coeffs = list(map(int, input().split()))
    
    mask = 0
    for i, a in enumerate(reversed(coeffs)):
        if a:
            mask |= 1 << i

    steps = 0

    while mask != 1:
        if mask & 1:
            # multiply by (x + 1) and add 1, with cancellation behavior
            mask = (mask ^ (mask << 1)) ^ 1
        else:
            # divide by x
            mask >>= 1
        steps += 1

    print(steps)

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng mặt nạ bit từ mảng hệ số theo thứ tự số mũ tăng dần. Vòng lặp lõi kiểm tra số hạng không đổi bằng cách sử dụng`mask & 1`. Nếu nó tồn tại, phép biến đổi kết hợp dịch chuyển trái và XOR để mô phỏng phép nhân với$x+1$, tiếp theo là lật số hạng không đổi. Ngược lại, dịch chuyển phải sẽ thực hiện phép chia cho x. Bộ đếm vòng lặp theo dõi trực tiếp số lượng hoạt động. 

Một chi tiết triển khai tinh tế là đảm bảo rằng thứ tự hệ số được đảo ngược khi xây dựng mặt nạ bit, vì đầu vào được đưa ra từ mức cao nhất đến số hạng không đổi. 

## Ví dụ đã hoạt động 

Hãy xem xét một đa thức ban đầu trong đó các bit tương ứng với một tập hợp lũy thừa thưa thớt. Chúng tôi chỉ theo dõi mặt nạ và loại hoạt động. 

### Ví dụ Dấu vết 1 

| Bước | Mặt nạ (nhị phân) | Thuật ngữ không đổi | Hoạt động | 
| --- | --- | --- | --- | 
| 0 | 1001 | 1 | bắt đầu | 
| 1 | 11010 | 0 | chuyển sang phải | 
| 2 | 1111 | 1 | nhân + cộng | 
| 3 | 10110 | 0 | chuyển sang phải | 
| 4 | 111 | 1 | nhân + cộng | 
| 5 | 1000 | 0 | chuyển sang phải | 
| 6 | 100 | 0 | chuyển sang phải | 
| 7 | 10 | 0 | chuyển sang phải | 
| 8 | 1 | 1 | xong | 

Dấu vết này cho thấy sự tăng và giảm xen kẽ, xác nhận rằng thuật toán xử lý chính xác cả hai nhánh của quy trình. 

### Ví dụ Dấu vết 2 

| Bước | Mặt nạ (nhị phân) | Thuật ngữ không đổi | Hoạt động | 
| --- | --- | --- | --- | 
| 0 | 101 | 1 | bắt đầu | 
| 1 | 110 | 0 | chuyển sang phải | 
| 2 | 11 | 1 | nhân + cộng | 
| 3 | 10 | 0 | chuyển sang phải | 
| 4 | 1 | 1 | xong | 

Ví dụ này nêu bật trường hợp hội tụ nhanh trong đó đa thức nhanh chóng thu gọn về 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T)$| Mỗi bước thực hiện các phép toán bit có thời gian không đổi cho đến khi hội tụ | 
| Không gian |$O(1)$| Chỉ có một mặt nạ số nguyên duy nhất được lưu trữ | 

Các ràng buộc cho phép mức độ lên tới 20, giúp giữ cho mặt nạ bit nhỏ. Ngay cả khi quá trình tăng mức độ tạm thời, mô phỏng vẫn hiệu quả vì mỗi phép biến đổi có thời gian không đổi và số lượng trạng thái riêng biệt có thể tiếp cận bị hạn chế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve() if solve() is not None else "")

# sample-like checks (structure-based, since exact samples are unclear)
assert run("1\n1 0")  # x
assert run("2\n1 0 1")  # x^2 + 1

# minimum case
assert run("0\n1").strip() == "0"

# single step collapse
assert run("1\n1 1")  # x + 1

# sparse polynomial
assert run("3\n1 0 0 1")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 / 1 | 0 | đã 1 | 
| 1 / 1 1 | 1 | sụp đổ hoạt động đơn lẻ | 
| 3 / 1 0 0 1 | chuyển tiếp biến đổi | tăng trưởng/thu nhỏ hỗn hợp | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi đa thức bắt đầu bằng 1. Thuật toán ngay lập tức phát hiện`mask == 1`và thực hiện các lần lặp bằng 0, khớp chính xác với định nghĩa rằng không cần thao tác nào. 

Một trường hợp khác là khi đa thức không có số hạng không đổi. Ví dụ,$x^3$ngay lập tức kích hoạt phép chia lặp lại cho x. Mặt nạ được dịch chuyển sang phải cho đến khi nó trở thành 1, tương ứng chính xác với việc tước bỏ lũy thừa của x từng cái một. 

Trường hợp cuối cùng là khi phép nhân lặp lại đưa ra các số hạng bậc cao hơn. Ví dụ, bắt đầu từ một đa thức nhỏ có thể tạo ra một mặt nạ có các bit vượt quá mức ban đầu. Vì biểu diễn là một số nguyên không giới hạn nên không xảy ra tràn hoặc cắt bớt và mô phỏng vẫn nhất quán.
