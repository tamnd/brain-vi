---
title: "CF 104819I - Không thích"
description: "Chúng ta được cho một hoán vị có độ dài $n$, và đối với bất kỳ mảng con liền kề nào, chúng ta xem xét phần tử lớn nhất của nó. Đối với một hoán vị cố định, chúng tôi tính giá trị toàn cục $G(S)$, là tổng của các giá trị cực đại này trên tất cả các mảng con $n(n+1)/2$."
date: "2026-06-28T13:03:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104819
codeforces_index: "I"
codeforces_contest_name: "2023 Sun Yat-sen University Collegiate Programming Contest, Onsite"
rating: 0
weight: 104819
solve_time_s: 67
verified: true
draft: false
---

[CF 104819I - Không thích](https://codeforces.com/problemset/problem/104819/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị độ dài$n$và đối với bất kỳ mảng con liền kề nào, chúng ta xem xét phần tử lớn nhất của nó. Đối với hoán vị cố định, chúng tôi tính giá trị toàn cầu$G(S)$, là tổng của các cực đại này trên tất cả$n(n+1)/2$mảng con. 

Trong số tất cả các hoán vị của$1 \ldots n$, chúng tôi xác định$K$là giá trị nhỏ nhất có thể có của tổng này. Nhiệm vụ không chỉ là tìm$K$, nhưng để đếm xem có bao nhiêu hoán vị có tính chất ổn định mạnh hơn: nếu chúng ta liên tục xoay hoán vị bằng cách lấy phần tử đầu tiên và di chuyển đến cuối thì mọi phép quay kết quả vẫn phải đạt được giá trị tối thiểu này$K$. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp cho một giá trị là$n$. Đối với mỗi$n$, chúng ta phải xuất ra bao nhiêu hoán vị thỏa mãn điều kiện ổn định xoay này, modulo$998244353$. 

Ràng buộc$n \le 10^5$và lên đến$10^5$các trường hợp thử nghiệm buộc mọi giải pháp về cơ bản phải$O(1)$hoặc$O(\log n)$mỗi truy vấn. Bất cứ điều gì liên quan đến việc xây dựng các hoán vị hoặc đánh giá các số liệu thống kê mảng con một cách trực tiếp đều không thể thực hiện được, vì ngay cả việc tính toán cũng$G(S)$một lần là$O(n^2)$. 

Một vấn đề tế nhị là tình trạng này có tính chu kỳ. Nó không đủ để một hoán vị là tối ưu. Mỗi sự thay đổi theo chu kỳ cũng phải tối ưu. Điều này tạo ra một hạn chế về cấu trúc toàn cầu trên tất cả các vị trí cùng một lúc, không chỉ là sự tối ưu cục bộ. 

Một sai lầm phổ biến là cho rằng tất cả các hoán vị tối ưu đều tự động thỏa mãn thuộc tính xoay. Như chúng ta sẽ thấy, tính tối ưu và tính đóng xoay là những điều kiện rất khác nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả$n!$hoán vị, tính toán$G(S)$cho mỗi người sử dụng một cách ngây thơ$O(n^2)$quét qua các mảng con, sau đó kiểm tra xem tất cả các phép quay có giữ nguyên giá trị hay không. Điều này đã vượt quá$O(n! \cdot n^2)$, vượt xa mọi giới hạn khả thi ngay cả đối với$n = 8$. 

Do đó chúng ta cần hiểu cấu trúc nào giảm thiểu$G(S)$. Một quan sát quan trọng là mỗi phần tử đóng góp vào các mảng con ở mức tối đa. Đối với một hoán vị, mỗi giá trị có một “phạm vi ảnh hưởng” liền kề được xác định bởi các phần tử lớn hơn gần nhất ở bên trái và bên phải của nó. Các phần tử lớn tạo ra những đóng góp lớn, do đó việc giảm thiểu tổng số đòi hỏi phải sắp xếp các phần tử sao cho giá trị lớn không tạo ra khoảng ưu thế rộng không cần thiết. 

Giải quyết các vụ việc nhỏ sẽ bộc lộ một khuôn mẫu cứng nhắc. Vì$n = 3$, giá trị tối thiểu đạt được bằng bốn hoán vị, nhưng chúng thuộc hai nhóm tuần hoàn. Điều quan trọng là trong mỗi nhóm phép quay theo chu kỳ, một số phép quay là tối ưu trong khi những phép quay khác thì không. Điều này cho thấy tính tối ưu không được bảo toàn khi xoay vòng. 

Nhận xét mang tính quyết định là đối với$n \ge 3$, mọi lớp hoán vị xoay theo chu kỳ nhất thiết phải chứa cả cấu hình tối ưu và không tối ưu. Điều này xảy ra bởi vì bất kỳ sự sắp xếp nào của ba phần tử liên tiếp ở đâu đó trong chu trình cuối cùng sẽ xoay thành một cấu hình tạo ra cấu trúc “thung lũng”, tăng sự đóng góp cho$G(S)$. 

Vì$n = 1$, tình hình là tầm thường. Vì$n = 2$, cả hai hoán vị đều hoạt động đối xứng và cả hai phép quay đều bảo toàn tính tối ưu. Đối với bất kỳ$n \ge 3$, không hoán vị nào có thể duy trì tính tối ưu trong mọi phép dịch chuyển theo chu kỳ. 

Điều này thu gọn toàn bộ vấn đề thành một câu trả lời liên tục cho mỗi$n$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n^2)$|$O(n)$| Quá chậm | 
| Phân tích kết cấu |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

###Các bước suy luận tối ưu 

1. Đầu tiên xác định hành vi cho nhỏ$n$, vì các ràng buộc theo chu kỳ thường chỉ vượt quá một ngưỡng. Vì$n = 1$, chỉ có một hoán vị nên nó thỏa mãn điều kiện. 
2. Đối với$n = 2$, cả hai hoán vị đều là phép quay của nhau và tính toán trực tiếp cho thấy chúng tạo ra cùng một giá trị của$G(S)$. Vì mọi phép quay vẫn tối ưu nên cả hai hoán vị đều thỏa mãn điều kiện. 
3. Đối với$n \ge 3$, phân tích cách đóng góp tối đa của mảng con khi ba phần tử riêng biệt xuất hiện ở các vị trí tuần hoàn khác nhau. Bất kỳ hoán vị nào cũng sẽ chứa một số bộ ba phần tử có thứ tự tương đối tạo thành một “thung lũng” cục bộ sau một vòng quay nhất định. 
4. Cấu trúc thung lũng đó làm tăng sự đóng góp của ít nhất một phần tử trong tổng trên cực đại của mảng con, phá vỡ tính tối ưu. Vì các phép quay hoán vị các vị trí nên mọi hoán vị cuối cùng đều tạo ra một cấu hình như vậy dưới một số phép quay. 
5. Do đó không có hoán vị độ dài$n \ge 3$có thể có tất cả các phép quay đạt được mức tối thiểu toàn cầu$K$. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là phép quay theo chu kỳ không bảo toàn vị trí tương đối của tất cả các bộ ba cùng một lúc. Mặc dù một hoán vị có thể tránh được sự thiếu hiệu quả cục bộ theo một hướng, nhưng việc xoay vòng nó sẽ chuyển những sự thiếu hiệu quả này sang tồn tại ở nơi khác. Vì$n \ge 3$, sự hiện diện của ba giá trị riêng biệt đảm bảo rằng một số phép quay tạo ra một cấu hình trong đó phần tử ở giữa trở thành mức tối thiểu cục bộ giữa hai phần tử lớn hơn, điều này làm tăng nghiêm ngặt sự đóng góp của nó vào tổng tối đa của mảng con. Điều này ngăn chặn việc đóng hoàn toàn vòng quay tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n = int(input())
        if n == 1:
            print(1)
        elif n == 2:
            print(2)
        else:
            print(0)

if __name__ == "__main__":
    solve()
```Giải pháp hoàn toàn dựa vào việc phân loại cấu trúc của các hoán vị hợp lệ. Không tính toán$G(S)$là cần thiết. 

Chi tiết triển khai duy nhất quan trọng là xử lý nhiều trường hợp thử nghiệm một cách hiệu quả. Mỗi truy vấn được trả lời trong thời gian không đổi, do đó tổng độ phức tạp chỉ phụ thuộc vào việc đọc dữ liệu đầu vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét$n = 2$. 

| Hoán vị | Xoay | Có hiệu lực? | 
| --- | --- | --- | 
| [1,2] | [1,2], [2,1] | Có | 
| [2,1] | [2,1], [1,2] | Có | 

Cả hai hoán vị đều thỏa mãn rằng mọi phép quay đều đạt được mức tối thiểu như nhau$G(S)$. 

Điều này chứng tỏ rằng đối với$n=2$, đối xứng quay không gây ra bất kỳ sự mất cân bằng cấu trúc nào. 

### Ví dụ 2 

Hãy xem xét$n = 3$. 

| Hoán vị | Xoay | Tối ưu trong mọi vòng quay? | 
| --- | --- | --- | 
| [1,2,3] | 123 → 231 → 312 | Không | 
| [1,3,2] | 132 → 321 → 213 | Không | 
| [2,1,3] | 213 → 132 → 321 | Không | 
| [2,3,1] | 231 → 312 → 123 | Không | 
| [3,1,2] | 312 → 123 → 231 | Không | 
| [3,2,1] | 321 → 213 → 132 | Không | 

Mặc dù một số hoán vị riêng lẻ đạt được mức tối thiểu$K$, mọi quỹ đạo tuần hoàn đều chứa ít nhất một sự sắp xếp không tối ưu. 

Điều này khẳng định rằng đối với$n \ge 3$, không có hoán vị nào thỏa mãn yêu cầu đóng xoay. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T)$| Mỗi trường hợp thử nghiệm được trả lời với số lần kiểm tra không đổi | 
| Không gian |$O(1)$| Không có cấu trúc phụ trợ ngoài các biến đầu vào | 

Các ràng buộc cho phép lên đến$10^5$truy vấn và giải pháp chỉ thực hiện phân nhánh theo thời gian không đổi cho mỗi truy vấn, dễ dàng khớp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        T = int(input())
        out = []
        for _ in range(T):
            n = int(input())
            if n == 1:
                out.append("1")
            elif n == 2:
                out.append("2")
            else:
                out.append("0")
        return "\n".join(out)

    return solve()

# provided-style checks
assert run("3\n1\n2\n3\n") == "1\n2\n0"

# custom cases
assert run("1\n1\n") == "1"
assert run("1\n2\n") == "2"
assert run("1\n4\n") == "0"
assert run("5\n3\n3\n3\n3\n3\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$n=1$| 1 | trường hợp đơn cơ sở | 
|$n=2$| 2 | đóng cửa xoay đối xứng | 
|$n=4$| 0 | thu gọn cho tất cả các kích thước lớn hơn | 
| lặp đi lặp lại$n=3$| 0 | sự ổn định trên nhiều truy vấn | 

## Vỏ cạnh 

cho$n = 1$, hoán vị duy nhất không có cấu trúc nào có thể bị phá vỡ khi quay, vì vậy nó thỏa mãn điều kiện một cách tầm thường. 

Vì$n = 2$, hai hoán vị là phép quay của nhau và cả hai đều bảo toàn cấu trúc mảng tối đa con giống nhau, vì vậy cả hai đều đủ điều kiện. 

Vì$n \ge 3$, mọi hoán vị chắc chắn chứa ba phần tử mà thứ tự tương đối của chúng trở nên không ổn định khi quay, tạo ra ít nhất một phép quay tăng$G(S)$. Điều này phá vỡ yêu cầu rằng mọi vòng quay phải duy trì ở mức tối ưu, buộc câu trả lời về 0 cho tất cả các vòng quay lớn hơn.$n$.
