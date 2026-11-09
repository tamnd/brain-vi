---
title: "CF 1468N - Phân loại rác thải"
description: "Monocarp cần vứt một số vật phẩm thuộc các loại khác nhau vào ba thùng chứa có dung tích cố định. Hộp đựng đầu tiên chỉ chấp nhận các mặt hàng giấy, hộp đựng thứ hai chỉ bằng nhựa và hộp thứ ba chứa tất cả các loại khác."
date: "2026-06-11T01:33:25+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1468
codeforces_index: "N"
codeforces_contest_name: "2020-2021 ICPC, NERC, Southern and Volga Russian Regional Contest (Online Mirror, ICPC Rules)"
rating: 900
weight: 1468
solve_time_s: 109
verified: true
draft: false
---

[CF 1468N - Phân loại rác](https://codeforces.com/problemset/problem/1468/N) 

**Xếp hạng:** 900 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Monocarp cần vứt một số vật phẩm thuộc các loại khác nhau vào ba thùng chứa có dung tích cố định. Hộp đựng đầu tiên chỉ chấp nhận các mặt hàng giấy, hộp đựng thứ hai chỉ bằng nhựa và hộp thứ ba chứa tất cả các loại khác. Monocarp có một số mặt hàng hoàn toàn là giấy, hoàn toàn bằng nhựa hoặc hoàn toàn khác, nhưng anh ấy cũng có những mặt hàng hoàn toàn là giấy hoặc một phần nhựa, có thể đựng vào thùng chính tương ứng hoặc vào thùng thứ ba. Câu hỏi đặt ra là liệu tất cả các mặt hàng có thể chứa vừa mà không vượt quá sức chứa của bất kỳ thùng chứa nào hay không. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chỉ định dung lượng của ba thùng chứa và số lượng từng loại mặt hàng mà Monocarp có. Đầu ra là CÓ nếu có thể đặt tất cả các mục phù hợp với dung lượng, nếu không thì KHÔNG. 

Các ràng buộc cho phép lên đến$3 \cdot 10^4$trường hợp thử nghiệm và mỗi giá trị có thể lớn bằng$10^8$. Điều này loại trừ bất kỳ giải pháp nào cố gắng liệt kê sự kết hợp của các phần tử một phần vì số lượng các khả năng có thể rất lớn. Lời giải phải dựa vào kiểm tra số học và so sánh đơn giản. 

Trường hợp cạnh phát sinh khi dung lượng của vùng chứa nhỏ hơn các mục bắt buộc đối với vùng chứa đó. Ví dụ: nếu hộp đựng giấy chỉ có thể chứa 1 mặt hàng nhưng Monocarp có 2 mặt hàng giấy nghiêm ngặt thì việc phân phối các mặt hàng một phần giấy có thể bù đắp được. Một tình huống tinh tế khác xảy ra khi các mặt hàng một phần giấy hoặc một phần nhựa phải tràn vào thùng chứa thứ ba. Nếu thùng thứ ba đã gần đầy do có các mặt hàng khác, chúng ta phải kiểm tra xem việc bổ sung các mặt hàng linh hoạt này có còn phù hợp hay không. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi cách phân bổ có thể các mặt hàng một phần giấy và một phần nhựa giữa hộp đựng chính và hộp đựng thứ ba. Ví dụ, nếu có$a_4$các mặt hàng chỉ có một phần giấy, chúng ta có thể thử tất cả các phần tách từ 0 trong vùng chứa đầu tiên đến tất cả trong vùng chứa đầu tiên. Tổng số kết hợp sẽ là$(a_4+1) \cdot (a_5+1)$, điều đó là không thể khi$a_4$hoặc$a_5$có thể$10^8$. 

Cách tiếp cận tối ưu thừa nhận rằng chúng ta không cần phải liệt kê các phần tách. Đối với hộp đựng đầu tiên, chúng ta phải đặt tất cả các vật dụng bằng giấy nghiêm ngặt. Bất kỳ dung lượng còn lại nào cũng có thể được lấp đầy bằng các mặt hàng giấy một phần và phần còn lại phải được chuyển vào thùng chứa thứ ba. Logic tương tự áp dụng cho thùng chứa thứ hai và các mặt hàng bằng nhựa một phần. Cuối cùng, chúng tôi kiểm tra xem vùng chứa thứ ba có thể chứa tất cả các mục cơ bản của nó cộng với phần tràn từ các mục một phần hay không. Điều này làm giảm vấn đề để so sánh số học đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(a4 * a5) | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc dung lượng$c_1$,$c_2$,$c_3$và số lượng vật phẩm$a_1$bởi vì$a_5$. 
2. Kiểm tra xem các mặt hàng giấy nghiêm ngặt có vừa với hộp đựng đầu tiên hay không. Tính toán dung lượng còn lại sau khi đặt tất cả các mặt hàng giấy nghiêm ngặt:`rem1 = c1 - a1`. Nếu như`rem1`là âm tính, in NO và tiếp tục test case tiếp theo. 
3. Sử dụng dung lượng còn lại trong hộp đựng đầu tiên để đặt các đồ vật có một phần giấy. Nếu như`a4`vượt quá`rem1`, phần thừa phải chuyển sang thùng thứ ba. Tính toán tràn này:`overflow_paper = max(0, a4 - rem1)`. 
4. Lặp lại logic tương tự cho hộp đựng thứ hai với các đồ vật bằng nhựa nghiêm ngặt và các đồ vật chỉ bằng nhựa một phần. Tính toán lượng tràn vào thùng chứa thứ ba:`overflow_plastic = max(0, a5 - (c2 - a2))`. Nếu các mục nghiêm ngặt không phù hợp, hãy in NO. 
5. Cuối cùng, hãy kiểm tra xem thùng thứ ba có thể chứa tất cả các mặt hàng nghiêm ngặt khác cộng với phần tràn ra từ các mặt hàng một phần giấy và một phần nhựa hay không:`total_in_third = a3 + overflow_paper + overflow_plastic`. Nếu như`total_in_third > c3`, in số Ngược lại in ra CÓ. 

Tại sao nó hoạt động: ở mỗi bước, chúng tôi luôn đặt các mục bắt buộc vào các thùng chứa bắt buộc trước tiên. Các mặt hàng linh hoạt một phần chỉ được chuyển sang thùng chứa thứ ba nếu thùng chứa chính của chúng không thể chứa chúng. Điều này đảm bảo rằng không có thùng chứa nào vượt quá sức chứa và chúng tôi tôn trọng các hạn chế về mặt hàng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    c1, c2, c3 = map(int, input().split())
    a1, a2, a3, a4, a5 = map(int, input().split())
    
    rem1 = c1 - a1
    if rem1 < 0:
        print("NO")
        continue
    overflow_paper = max(0, a4 - rem1)
    
    rem2 = c2 - a2
    if rem2 < 0:
        print("NO")
        continue
    overflow_plastic = max(0, a5 - rem2)
    
    if a3 + overflow_paper + overflow_plastic <= c3:
        print("YES")
    else:
        print("NO")
```Trước tiên, mã sẽ trừ các mục bắt buộc khỏi vùng chứa của chúng, sau đó tính toán số lượng mục linh hoạt tràn vào vùng chứa thứ ba. sử dụng`max(0, ...)`đảm bảo chúng tôi không bao giờ trừ nhiều hơn dung lượng sẵn có. Mỗi so sánh tương ứng trực tiếp với một hạn chế về dung lượng vùng chứa. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
c1=2, c2=2, c3=3
a1=1, a2=2, a3=3, a4=1, a5=0
```| Bước | rem1 | tràn_paper | rem2 | tràn_plastic | tổng_in_thứ ba | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2-1=1 | tối đa(0,1-1)=0 | 2-2=0 | tối đa(0,0-0)=0 | 3+0+0=3 | CÓ | 

Giấy nghiêm ngặt vừa với hộp đựng đầu tiên, mặt hàng giấy một phần cũng vừa vặn ở đó, hộp thứ ba có thể chứa tất cả các mặt hàng. 

### Đầu vào mẫu 2```
c1=2, c2=2, c3=3
a1=1, a2=2, a3=3, a4=0, a5=1
```| Bước | rem1 | tràn_paper | rem2 | tràn_plastic | tổng_in_thứ ba | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2-1=1 | tối đa(0,0-1)=0 | 2-2=0 | tối đa(0,1-0)=1 | 3+0+1=4 | KHÔNG | 

Vật phẩm bằng nhựa một phần tràn sang thùng thứ ba, sau đó vượt quá sức chứa của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm yêu cầu một vài phép tính số học. | 
| Không gian | O(1) | Không có thang đo bộ nhớ bổ sung với kích thước đầu vào. | 

Với những hạn chế$t \le 3\cdot10^4$, mỗi trường hợp thử nghiệm chỉ sử dụng thời gian không đổi, do đó giải pháp chạy tốt trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        c1, c2, c3 = map(int, input().split())
        a1, a2, a3, a4, a5 = map(int, input().split())
        
        rem1 = c1 - a1
        if rem1 < 0:
            output.append("NO")
            continue
        overflow_paper = max(0, a4 - rem1)
        
        rem2 = c2 - a2
        if rem2 < 0:
            output.append("NO")
            continue
        overflow_plastic = max(0, a5 - rem2)
        
        if a3 + overflow_paper + overflow_plastic <= c3:
            output.append("YES")
        else:
            output.append("NO")
    return "\n".join(output)

# Provided samples
assert run("7\n1 2 3\n1 2 3 0 0\n2 2 3\n1 2 3 1 0\n2 2 3\n1 2 3 0 1\n1 2 5\n1 2 3 1 1\n0 0 0\n0 0 0 0 0\n0 0 4\n1 0 0 0 0\n13 37 42\n0 0 0 40 47\n") == "YES\nYES\nNO\nYES\nYES\nNO\nYES"

# Custom cases
assert run("1\n0 0 0\n0 0 0 0 0\n") == "YES"  # all zero capacities, no items
assert run("1\n1 1 1\n1 0 0 0 0\n") == "YES"  # exactly fits in first container
assert run("1\n1 1 1\n2 0 0 0 0\n") == "NO"   # first container over capacity
assert run("1\n1 1 1\n0 0 1 1 1\n") == "
```
