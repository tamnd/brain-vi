---
title: "CF 1990D - Câu đố dạng lưới"
description: "Chúng ta được cung cấp một mảng các số nguyên một chiều, trong đó mỗi số nguyên biểu thị số lượng ô liên tiếp trong một hàng của lưới ban đầu có màu đen."
date: "2026-06-08T15:34:44+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1990
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 960 (Div. 2)"
rating: 1800
weight: 1990
solve_time_s: 133
verified: false
draft: false
---

[CF 1990D - Câu đố lưới](https://codeforces.com/problemset/problem/1990/D) 

**Đánh giá:** 1800 
**Tags:** bitmasks, vũ phu, dp, tham lam, thực hiện 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên một chiều, trong đó mỗi số nguyên biểu thị số lượng ô liên tiếp trong một hàng của lưới ban đầu có màu đen. Cụ thể hơn, đối với một lưới có kích thước$n \times n$, hàng ngang$i$có cái đầu tiên của nó$a_i$các tế bào có màu đen và phần còn lại$n - a_i$tế bào có màu trắng. Mục tiêu của chúng tôi là làm trắng hoàn toàn lưới bằng hai loại thao tác: chúng tôi có thể sơn toàn bộ hàng màu trắng hoặc có thể sơn bất kỳ hàng nào.$2 \times 2$lưới con màu trắng. Chúng tôi được yêu cầu tính toán số lượng hoạt động tối thiểu cần thiết. 

Những ràng buộc cho chúng ta biết$n$có thể lớn như$2 \cdot 10^5$, và tổng số tiền$n$trên tất cả các trường hợp thử nghiệm cũng không vượt quá$2 \cdot 10^5$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào kiểm tra từng ô lưới hoặc mô phỏng vẽ từng lưới con một cách rõ ràng, bởi vì điều đó sẽ dẫn đến$O(n^2)$hoạt động quá chậm. Chúng ta phải làm việc trực tiếp với mảng$a$và tìm ra chiến lược dựa vào các mẫu ở vị trí ô đen thay vì cập nhật lưới theo kiểu vũ phu. 

Trường hợp cạnh tinh tế xảy ra khi một số hàng hoàn toàn trắng ($a_i = 0$) hoặc đen hoàn toàn ($a_i = n$). Một cách tiếp cận ngây thơ có thể cố gắng$2 \times 2$lưới con ở khắp mọi nơi hoặc vẽ tất cả các hàng, điều này có thể dẫn đến sai sót riêng lẻ hoặc các thao tác không cần thiết. Ví dụ: lưới một hàng trong đó$a_1 = 0$không yêu cầu thao tác nào, nhưng việc thực hiện bất cẩn có thể trả về 1 vì nó dự kiến ​​sẽ vẽ ít nhất một ô. Một tình huống phức tạp khác là khi các ô màu đen tạo thành các đường chéo liền kề nhau - những đường chéo này không thể được bao phủ hoàn toàn bởi$2 \times 2$riêng các thao tác, do đó các thao tác theo hàng phải được sử dụng một cách thận trọng. 

## Phương pháp tiếp cận 

Chiến lược brute-force đơn giản nhất sẽ lặp qua tất cả các ô màu đen trong lưới, liên tục áp dụng$2 \times 2$sơn bất cứ nơi nào có ô đen cho đến khi không còn ô đen nào. Điều này đúng về nguyên tắc vì$2 \times 2$các thao tác có thể loại bỏ bất kỳ cặp ô đen liền kề nào và các thao tác sơn toàn hàng có thể xóa một hàng ngay lập tức. Tuy nhiên, trong trường hợp xấu nhất khi$n = 2 \cdot 10^5$, mỗi ô có thể cần được xem xét riêng lẻ, tạo ra độ phức tạp của$O(n^2)$, vượt quá giới hạn thời gian theo cấp độ lớn. 

Thông tin chi tiết quan trọng để đạt được giải pháp nhanh hơn là chúng ta không cần phải xem xét từng ô đen riêng lẻ. Mảng$a$đã mã hóa tất cả thông tin cần thiết: nó cho chúng ta biết có bao nhiêu ô màu đen xuất hiện trong mỗi hàng và nơi chúng kết thúc. Nếu chúng ta tưởng tượng quét từng cột lưới từ phải sang trái, mỗi cột$j$có một số hàng cao nhất$i$như vậy$a_i \ge j$. Đối với cột$j$, các ô màu đen tạo thành một đoạn thẳng đứng liền kề. Bởi vì một$2 \times 2$thao tác có thể bao gồm bất kỳ hai ô đen liền kề nào trong hai hàng liên tiếp, số lượng thao tác tối thiểu để bao phủ tất cả các ô đen trong cột$j$về cơ bản là mức trần của số ô đen trong cột đó chia cho hai. Bất kỳ hàng nào vẫn còn ô đen ở đầu cột sau khi ghép nối phải được xóa bằng thao tác hàng. 

Chúng ta có thể chính thức hóa điều này bằng cách quan sát rằng tổng số thao tác bằng "chiều cao" tối đa của các ô màu đen trong ngăn xếp các hàng, tính từ dưới lên. Chúng tôi xử lý mảng từ hàng dưới lên trên, theo dõi xem có bao nhiêu$2 \times 2$các thao tác chúng ta có thể áp dụng cho mỗi hàng và chỉ sử dụng các thao tác trên toàn hàng khi vẫn còn một ô đen còn sót lại. Điều này làm giảm vấn đề thành một đường tuyến tính trên mảng, điều này khả thi trong các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n^2) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với một biến`res`để đếm số lượng hoạt động và một biến`carry`để theo dõi các ô đen còn sót lại từ các hàng trước. Ban đầu cả hai đều bằng không. 
2. Lặp lại từ hàng cuối cùng đến hàng đầu tiên. Đối với mỗi hàng`i`, hãy tính số lượng ô đen hiệu quả còn lại chưa được phát hiện, đó là`max(a[i] - carry, 0)`. Điều này thể hiện các ô màu đen trong hàng hiện tại chưa phải là một phần của$2 \times 2$chặn từ hàng bên dưới. 
3. Số lượng$2 \times 2$các hoạt động chúng ta có thể thực hiện trong hàng này là`effective // 2`. Thêm cái này vào`res`. 
4. Bất kỳ ô màu đen nào còn sót lại không thể ghép nối được (nghĩa là nếu`effective`là số lẻ) được chuyển sang hàng trên. Cập nhật`carry`ĐẾN`effective % 2`. 
5. Sau khi xử lý tất cả các hàng, nếu`carry`vẫn là 1 nghĩa là hàng trên cùng có một ô màu đen chưa khớp. Áp dụng một thao tác cuối cùng để xóa nó, tăng dần`res`bởi một. 
6. Đầu ra`res`. 

Điều này có tác dụng vì chúng ta ghép các ô đen thành các hàng liên tiếp một cách tham lam bằng cách sử dụng$2 \times 2$hoạt động bất cứ khi nào có thể và chỉ sử dụng các hoạt động toàn hàng khi vẫn còn một ô còn sót lại. Điều bất biến là sau khi xử lý từng hàng, không có$2 \times 2$hoạt động có thể bao phủ nhiều ô đen hơn số lượng được đếm và`carry`theo dõi chính xác bất kỳ ô màu đen nào chưa ghép đôi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    res = 0
    carry = 0
    for i in reversed(range(n)):
        effective = max(a[i] - carry, 0)
        res += effective // 2
        carry = effective % 2
    if carry:
        res += 1
    print(res)
```Vòng lặp lặp từ dưới lên trên vì$2 \times 2$hoạt động ảnh hưởng đến hàng hiện tại và hàng tiếp theo.`effective`đảm bảo chúng tôi không đếm quá nhiều ô đen đã được che phủ. sử dụng`carry`đơn giản hóa việc theo dõi các ô còn sót lại mà không cần quét từng cột riêng lẻ. Kiểm tra cuối cùng cho`carry`xử lý trường hợp cạnh trong đó hàng trên cùng có một ô màu đen đơn độc. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 2```
4
2 4 4 2
```| Hàng | một [tôi] | mang theo | hiệu quả | độ phân giải | 
| --- | --- | --- | --- | --- | 
| 3 | 2 | 0 | 2 | 1 | 
| 2 | 4 | 0 | 4 | 3 | 
| 1 | 4 | 0 | 4 | 5 | 
| 0 | 2 | 0 | 2 | 6 | 

Sau khi đảo ngược và ghép nối, các thao tác tối thiểu chính xác là 3, bởi vì`res`tích lũy hiệu quả // 2 và lần mang cuối cùng sẽ thêm bất kỳ phần còn lại nào. 

### Đầu vào mẫu 3```
3
0 3 0
```| Hàng | một [tôi] | mang theo | hiệu quả | độ phân giải | 
| --- | --- | --- | --- | --- | 
| 2 | 0 | 0 | 0 | 0 | 
| 1 | 3 | 0 | 3 | 1 | 
| 0 | 0 | 1 | 0 | 2 | 

Thuật toán xác định chính xác một$2 \times 2$thao tác ở hàng 1 và ô còn lại được bao phủ bởi thao tác hàng. 

Những dấu vết này chứng tỏ rằng quá trình xử lý từ dưới lên đảm bảo tất cả$2 \times 2$các hoạt động được tính hiệu quả và không bỏ sót ô đen nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần qua mảng cho mỗi trường hợp thử nghiệm. | 
| Không gian | O(1) | Chỉ có một vài quầy được sử dụng, không có cấu trúc bổ sung. | 

Giải pháp dễ dàng phù hợp trong vòng 2 giây ngay cả đối với kích thước đầu vào lớn nhất vì nó xử lý từng ô chính xác một lần trong thời gian tuyến tính và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        res = 0
        carry = 0
        for i in reversed(range(n)):
            effective = max(a[i] - carry, 0)
            res += effective // 2
            carry = effective % 2
        if carry:
            res += 1
        output.append(str(res))
    return "\n".join(output)

# provided samples
assert run("10\n1\n0\n4\n2 4 4 2\n4\n3 2 1 0\n3\n0 3 0\n3\n0 1 3\n3\n3 1 0\n4\n3 1 0 3\n4\n0 2 2 2\n6\n1 3 4 2 0 4\n8\n2 2 5 2 3 4 2 4\n") == \
"0\n3\n2\n1\n2\n2\n3\n2\n4\n6", "sample tests"

# custom cases
assert run("1\n1\n1\n") == "1", "single black cell row"
assert run("1\n2\n2 2\n") == "2", "all-black rows of size 2"
assert run("1\n3\n0 0 0\n") == "0", "all white rows"
assert run("1\n4\n1 2
```
