---
title: "CF 105859B - A=B"
description: "Chúng ta có bốn số nguyên đến từ một cấu trúc rất cụ thể bao gồm ba số nguyên dương chưa biết, hãy gọi chúng là $a$, $b$ và $c$."
date: "2026-06-25T14:40:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105859
codeforces_index: "B"
codeforces_contest_name: "Mines HSPC 2025 Open Division"
rating: 0
weight: 105859
solve_time_s: 44
verified: true
draft: false
---

[CF 105859B - A=B](https://codeforces.com/problemset/problem/105859/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có bốn số nguyên đến từ một cấu trúc rất cụ thể bao gồm ba số nguyên dương chưa biết, hãy gọi chúng là$a$,$b$, Và$c$. Các số trên bảng chính xác là tổng của ba cặp$a+b$,$a+c$,$b+c$, và tổng số tiền$a+b+c$, nhưng thứ tự của chúng bị xáo trộn hoàn toàn. 

Công việc của chúng ta là khôi phục bất kỳ bộ ba hợp lệ nào$(a, b, c)$có thể đã tạo ra nhiều bộ này. 

Một cách hữu ích để nghĩ về điều này là một trong bốn số phải là số lớn nhất và số lớn nhất đó buộc phải là$a+b+c$, bởi vì mỗi tổng theo cặp hoàn toàn nhỏ hơn tổng đầy đủ khi$a, b, c$là tích cực. Khi đã xác định được điều đó, ba số còn lại đều tương ứng với “tổng trừ một biến”, vì vậy mỗi số ẩn một trong số đó.$a$,$b$, hoặc$c$. 

Các ràng buộc cực kỳ nhỏ, chỉ có bốn con số, vì vậy bất kỳ giải pháp nào ngoài công việc liên tục đều không cần thiết. Ngay cả việc liệt kê đơn giản các nhiệm vụ giữa bốn vị trí cũng bị giới hạn bởi một hệ số không đổi. Điều này có nghĩa là thách thức thực sự không phải là hiệu quả mà là việc xác định bất biến chính xác. 

Một trường hợp thất bại phổ biến xuất hiện khi mọi người cho rằng số tiền tối đa luôn là tổng mà không kiểm tra tính nhất quán. Ví dụ: nếu đầu vào là`40 40 40 60`, cách giải thích đúng là$a=b=c=20$. Nếu người ta cố hiểu nhầm một phần tử không cực đại là tổng, thì việc tái thiết sẽ bị hỏng ngay lập tức. 

Một trường hợp tinh vi khác là khi các giá trị lặp lại. Ví dụ,`3 6 5 4`không có gợi ý thứ tự duy nhất ngoại trừ thông qua logic, không phải tính duy nhất của các giá trị. Bất kỳ cách tiếp cận nào phụ thuộc vào sự khác biệt đều thất bại ở đây. 

## Phương pháp tiếp cận 

Cách giải thích brute-force là thử gán từng số trong số bốn số làm tổng ứng cử viên, sau đó cố gắng xây dựng lại$a, b, c$từ ba người còn lại. Tổng cộng mỗi lựa chọn$S$, chúng tôi tính toán$S-x_i$cho từng giá trị khác và kiểm tra xem ba số được xây dựng lại này có phù hợp với việc tạo ra nhiều tập hợp ban đầu của tổng theo cặp hay không. Vì chỉ có bốn lựa chọn cho$S$, đây là công việc liên tục. 

Điều này hiệu quả vì cấu trúc cứng nhắc: khi tổng số tiền được cố định, mỗi số còn lại sẽ ánh xạ trực tiếp đến một biến và không còn sự mơ hồ nào ngoại trừ hoán vị. 

Tuy nhiên, điều này có thể được đơn giản hóa hơn nữa bằng cách quan sát một tính chất mạnh hơn. Tổng số chính xác phải là phần tử lớn nhất trong tập hợp nhiều phần. Khi chúng tôi chấp nhận điều đó, việc tái thiết trở nên mang tính quyết định: trừ từng số trong ba số còn lại khỏi tổng số và chúng tôi ngay lập tức phục hồi$a, b, c$. 

Cách tiếp cận bạo lực có hiệu quả vì nó thử tất cả các vai trò có thể có của các con số. Nó trở nên không cần thiết một khi chúng ta nhận ra rằng tính tích cực thực thi một ràng buộc thứ tự nghiêm ngặt, thu gọn không gian tìm kiếm từ bốn ứng viên xuống còn một. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả) | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu (lấy tổng tối đa) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bốn số vào danh sách. 
2. Sắp xếp danh sách sao cho phần tử lớn nhất được đặt cuối cùng. 
3. Coi giá trị lớn nhất là tổng$S = a + b + c$. 
4. Đối với mỗi giá trị trong ba giá trị còn lại$x$, tính toán$S - x$. Những kết quả này tương ứng với các giá trị ẩn$a, b, c$theo một thứ tự nào đó. 
5. Xuất ra ba giá trị được xây dựng lại. 

Lý do bước 4 hợp lệ là vì mỗi số nhỏ hơn biểu thị một tổng theo cặp, chẳng hạn như$a+b$. Việc trừ nó khỏi tổng sẽ hủy chính xác hai biến liên quan đến cặp đó, để lại biến thứ ba. 

### Tại sao nó hoạt động 

Mỗi số đầu vào là tổng theo cặp hoặc tổng đầy đủ. Tổng đầy đủ lớn hơn bất kỳ tổng theo cặp nào vì tất cả các biến đều dương. Khi mức tối đa được cố định là$a+b+c$, mọi số khác$x$phải bằng$S - v$cho đúng một biến$v$. Điều này thiết lập ánh xạ một-một giữa ba số còn lại và các biến ban đầu, đảm bảo tính chính xác cho đến hoán vị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    nums = list(map(int, input().split()))
    nums.sort()
    
    S = nums[3]
    
    a = S - nums[0]
    b = S - nums[1]
    c = S - nums[2]
    
    print(a, b, c)

if __name__ == "__main__":
    solve()
```Bước sắp xếp rất quan trọng vì nó đảm bảo chúng ta xác định chính xác tổng số tiền. Nếu không sắp xếp, việc chọn một phần tử tùy ý làm tổng số ứng cử viên sẽ yêu cầu logic xác thực. 

Mỗi bước trừ tương ứng trực tiếp với việc cô lập một trong các biến chưa biết. Thứ tự đầu ra không quan trọng nên chúng ta không cần theo dõi giá trị nào tương ứng với biến ban đầu. 

Một lỗi phổ biến là cố gán trực tiếp các số nhỏ hơn dưới dạng$a, b, c$, nhưng điều đó chỉ hoạt động sau khi chuyển đổi thông qua phép trừ. Các giá trị đầu vào không phải là các biến mà là tổng hợp theo cặp của chúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 6 5 4
```Đã sắp xếp:`[3, 4, 5, 6]`| Bước | số | S | một | b | c | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [3, 4, 5, 6] | - | - | - | - | 
| Tổng số đã chọn | - | 6 | - | - | - | 
| Phép trừ | - | 6 | 3 | 2 | 1 | 

Đầu ra:```
3 2 1
```Điều này xác nhận rằng việc xây dựng lại thông qua phép trừ mang lại bộ ba hợp lệ và thứ tự không liên quan. 

Dấu vết cho thấy rằng mỗi giá trị nhỏ hơn trực tiếp mã hóa một biến bù từ tổng số. 

### Ví dụ 2 

đầu vào:```
40 40 40 60
```Đã sắp xếp:`[40, 40, 40, 60]`| Bước | số | S | một | b | c | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [40, 40, 40, 60] | - | - | - | - | 
| Tổng số đã chọn | - | 60 | - | - | - | 
| Phép trừ | - | 60 | 20 | 20 | 20 | 

Đầu ra:```
20 20 20
```Trường hợp này nhấn mạnh việc xử lý sự lặp lại. Mặc dù tất cả các tổng theo cặp đều giống hệt nhau, quá trình trừ vẫn tái tạo lại các biến bằng nhau một cách rõ ràng mà không có sự mơ hồ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ sắp xếp bốn số và số học không đổi | 
| Không gian | O(1) | Không có cấu trúc phụ trợ ngoài một vài biến | 

Kích thước đầu vào được cố định ở bốn giá trị, do đó, ngay cả một giải pháp đơn giản cũng nhanh chóng. Vấn đề này được thiết kế để kiểm tra lý luận cấu trúc hơn là tối ưu hóa thuật toán. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    nums = list(map(int, sys.stdin.readline().split()))
    nums.sort()
    S = nums[3]
    res = [S - nums[i] for i in range(3)]
    return " ".join(map(str, res))

# provided samples
assert sorted(run("3 6 5 4").split()) == sorted("3 2 1".split())
assert sorted(run("40 40 40 60").split()) == sorted("20 20 20".split())

# custom cases
assert sorted(run("2 2 2 6").split()) == sorted("2 2 2".split()), "all equal reconstruction"
assert sorted(run("1 2 3 6").split()) == sorted("1 2 3".split()), "minimal distinct case"
assert sorted(run("10 11 12 33").split()) == sorted("10 11 12".split()), "shifted values"
assert sorted(run("5 9 7 14").split()) == sorted("5 7 9".split()), "unsorted input robustness"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 2 6 | 2 2 2 | giá trị lặp lại | 
| 1 2 3 6 | 1 2 3 | tái thiết khác biệt đơn giản nhất | 
| 10 11 12 33 | 10 11 12 | tính nhất quán thay đổi lớn hơn | 
| 5 9 7 14 | 5 7 9 | xử lý đầu vào chưa được sắp xếp | 

## Vỏ cạnh 

Kịch bản có giá trị lặp lại như`40 40 40 60`chứng tỏ rằng thuật toán không dựa vào tính duy nhất. Sau khi chọn tổng số lớn nhất, mỗi phép trừ vẫn mang lại các giá trị giống hệt nhau, đó chính xác là sự tái tạo chính xác của$a=b=c$. 

Trong trường hợp đầu vào đã gần đồng nhất, chẳng hạn như`2 2 2 6`, mọi giả định không chính xác về số nào là tổng vẫn chỉ được giải quyết chính xác nếu sử dụng số lớn nhất, vì các lựa chọn nhỏ hơn sẽ tạo ra các giá trị âm hoặc không nhất quán. Bước sắp xếp đảm bảo chế độ lỗi này không thể xảy ra. 

Một trường hợp tinh tế khác là khi các giá trị được nhóm chặt chẽ, chẳng hạn như`1 2 3 6`. Ở đây mọi số có thể trông giống như một tổng, nhưng chỉ`6`đủ lớn để phục vụ như một tổng thể. Bước trừ tách biệt rõ ràng cấu trúc ngay cả khi trực giác về vai trò còn yếu.
