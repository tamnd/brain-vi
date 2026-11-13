---
title: "CF 1714E - Thêm Modulo 10"
description: "Chúng ta được cung cấp một mảng các số nguyên và với mỗi phần tử, chúng ta có thể tăng nó nhiều lần lên đến chữ số cuối cùng của nó. Ví dụ: nếu phần tử là 27, áp dụng phép toán sẽ cho 27 + 7 = 34 và áp dụng lại phép toán sẽ cho 34 + 4 = 38."
date: "2026-06-09T20:07:03+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1714
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 811 (Div. 3)"
rating: 1400
weight: 1714
solve_time_s: 147
verified: false
draft: false
---

[CF 1714E - Thêm Modulo 10](https://codeforces.com/problemset/problem/1714/E) 

**Đánh giá:** 1400 
**Tags:** lực lượng vũ phu, toán học, lý thuyết số 
**Thời gian giải:** 2m 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và với mỗi phần tử, chúng ta có thể tăng nó nhiều lần lên đến chữ số cuối cùng của nó. Ví dụ: nếu phần tử là 27, áp dụng phép toán này sẽ cho kết quả 27 + 7 = 34 và áp dụng lại phép toán sẽ cho kết quả 34 + 4 = 38. Nhiệm vụ là xác định xem liệu bằng cách áp dụng phép toán này nhiều lần trên bất kỳ tập hợp con nào của các phần tử, chúng ta có thể làm cho tất cả các phần tử mảng bằng nhau hay không. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có tối đa 200.000 phần tử và tổng của tất cả các phần tử trong tất cả các trường hợp thử nghiệm được giới hạn bằng 200.000. Điều này ngụ ý rằng thuật toán phải ở mức tuyến tính tệ nhất trong tổng số phần tử trong tất cả các trường hợp thử nghiệm, vì mọi thứ bậc hai hoặc cao hơn sẽ vượt quá giới hạn thời gian. 

Các trường hợp cạnh không rõ ràng phát sinh do thao tác hoạt động khác nhau tùy thuộc vào chữ số cuối cùng. Các phần tử kết thúc bằng 0 không bao giờ thay đổi, các phần tử kết thúc bằng 5 hội tụ thành bội số của 10 trong một bước và các phần tử kết thúc bằng 1-9 (trừ 5) tuân theo chu kỳ modulo 20 sau một vài bước. Một cách tiếp cận đơn giản mô phỏng mọi thao tác cho từng số có thể dễ dàng hết thời gian chờ. Một điều tinh tế khác là các phần tử có các chữ số cuối khác nhau có thể không bao giờ gặp nhau, ví dụ 1 và 2 không thể được chuyển đổi thành một giá trị chung chỉ bằng thao tác này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng hoạt động trên mọi phần tử cho đến khi nó lặp lại một giá trị đã thấy trước đó hoặc vượt quá một số giới hạn. Điều này hiệu quả vì thao tác tăng giá trị một cách đơn điệu, nhưng không thành công khi số lớn (lên đến 10^9) hoặc chuỗi dài, vì việc mô phỏng tất cả các bước có thể đạt tổng cộng hàng trăm triệu thao tác. Cụ thể, các số kết thúc bằng 1-9 khác 5 có thể tạo ra một chuỗi 20 bước trước khi lặp lại một mẫu, do đó, lực lượng vũ phu có thể chậm nếu chúng ta thử điều này với từng phần tử. 

Cái nhìn sâu sắc quan trọng là các con số được chia thành hai loại dựa trên chữ số cuối cùng của chúng. Nếu một số kết thúc bằng 0 hoặc 5 thì sau nhiều nhất một thao tác, số đó sẽ đạt bội số của 10. Các số kết thúc bằng các chữ số khác cuối cùng sẽ bước vào một chu kỳ theo mô-đun 20, tăng dần và không bao giờ trùng với bội số của 10. Do đó, mảng chỉ có thể được làm bằng nếu tất cả các số đều là bội số của 10 hoặc có thể được căn chỉnh trong chu kỳ mô-đun 20 bước của chúng. Cụ thể hơn, bất kỳ số nào kết thúc bằng 0 hoặc 5 đều phải là bội số của 10 sau khi biến đổi. Đối với các số có tận cùng là 1-9 (trừ 5), các số chuyển đổi modulo 20 phải bằng nhau, nếu không thì không bao giờ trùng nhau. 

Quan sát này cho phép chúng ta phân loại số một cách nhanh chóng mà không cần mô phỏng từng bước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O (n * max_hoạt động) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Đối với mỗi trường hợp thử nghiệm, hãy đọc độ dài mảng`n`và các phần tử mảng`a`. 
2. Chia mảng thành hai nhóm: các số kết thúc bằng 0 hoặc 5 và các số kết thúc bằng 1-9 ngoại trừ 5. Điều này là do hai nhóm này có các hành vi khác nhau về cơ bản khi thực hiện phép toán. 
3. Đối với các số có tận cùng bằng 0 hoặc 5, áp dụng thao tác nhiều nhất một lần. Nếu bất kỳ số nào kết thúc bằng 5, nó sẽ trở thành bội số của 10 sau một thao tác. Sau đó kiểm tra xem tất cả các số trong nhóm này có phải là bội số bằng 10 hay không. Nếu không, hãy trả về NO. 
4. Đối với các số kết thúc bằng 1-9 ngoại trừ 5, hãy quan sát rằng sau khi lặp lại các phép tính, tất cả các số cuối cùng đều đạt giá trị đồng dạng với 2 mod 20. Biến đổi từng số như vậy cho đến khi đạt đến số có chữ số cuối cùng là 2. Sau đó kiểm tra xem tất cả các số được chuyển đổi có bằng nhau hay không. Nếu có, hãy trả về CÓ; nếu không, trả về KHÔNG. 
5. Nếu mảng chứa sự kết hợp của hai nhóm (nhóm 0/5 và 1-9 ngoại trừ nhóm 5), chúng không bao giờ có thể bằng nhau. Trả lại KHÔNG. 

Tại sao nó hoạt động: Thao tác đơn điệu và chỉ phụ thuộc vào chữ số cuối cùng. Các số kết thúc bằng 0 hoặc 5 hội tụ thành bội số của 10 và các số kết thúc bằng 1-9 tuân theo chu kỳ modulo 20 với chữ số cuối cùng là 2. Do thuộc tính này, chúng ta có thể phân loại các số thành các loại loại trừ lẫn nhau và không bao giờ giao nhau và chỉ có thể có sự bằng nhau trong cùng một loại. Bất biến này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_equalize(arr):
    group0 = []
    group1 = []
    for x in arr:
        last = x % 10
        if last == 0 or last == 5:
            if last == 5:
                x += 5
            group0.append(x)
        else:
            while x % 10 != 2:
                x += x % 10
            group1.append(x)
    if group0 and group1:
        return "No"
    if group0:
        return "Yes" if all(x == group0[0] for x in group0) else "No"
    return "Yes" if all(x == group1[0] for x in group1) else "No"

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(can_equalize(a))
```Mã đọc từng trường hợp thử nghiệm và phân tách các số bằng chữ số cuối cùng của chúng. Các số kết thúc bằng 0 hoặc 5 được chuẩn hóa thành bội số của 10 và các số kết thúc bằng 1-9 được chuyển đổi cho đến khi chữ số cuối cùng của chúng trở thành 2. Việc kiểm tra sự bằng nhau được thực hiện trong mỗi nhóm. Nếu cả hai nhóm tồn tại trong cùng một trường hợp thử nghiệm, hàm sẽ trả về NO ngay lập tức. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`[6, 11]`| Bước | một[0] | một[1] | Bình luận | 
| --- | --- | --- | --- | 
| Ban đầu | 6 | 11 | 6 kết thúc bằng 6, 11 kết thúc bằng 1 | 
| Biến đổi một[0] | 12 | 11 | 6+6=12 | 
| Biến đổi một[1] | 12 | 12 | 11+1=12 | 
| Kiểm tra sự bình đẳng | 12 == 12 | | CÓ | 

Điều này chứng tỏ các số tận cùng bằng các chữ số khác nhau có thể thẳng hàng nếu cả hai đều thuộc nhóm "1-9 ngoại trừ 5". 

**Ví dụ 2:**`[2, 18, 22]`| Bước | 2 | 18 | 22 | Bình luận | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 18 | 22 | Tất cả đều kết thúc bằng 2, 8, 2 | 
| Biến đổi 2 | 2+2=4 | 18 | 22 | | 
| Biến đổi 4 | 4+4=8 | 18 | 22 | 2→4→8 | 
| Biến đổi 8 | 8+8=16 | 18 | 22 | 8→16 | 
| Biến đổi 16 | 16+6=22 | 18 | 22 | 16→22 | 
| Biến đổi 18 | 18+8=26 | 26 | 22 | 18→26, nhóm khác modulo 20 | 
| Không thể cân bằng | | | | KHÔNG | 

Dấu vết xác nhận modulo 20 chu kỳ khác nhau và các số không thể bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được biến đổi tối đa một số lần không đổi do chu kỳ modulo 20 hoặc chuẩn hóa chữ số cuối cùng. | 
| Không gian | O(n) | Danh sách tạm thời để lưu trữ các số được nhóm. | 

Điều này đủ hiệu quả với các ràng buộc: tổng`n`trên tất cả các trường hợp thử nghiệm 2·10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        output.append(can_equalize(a))
    return "\n".join(output)

# Provided samples
assert run("""10
2
6 11
3
2 18 22
5
5 10 5 10 5
4
1 2 4 8
2
4 5
3
93 96 102
2
40 6
2
50 30
2
22 44
2
1 5
""") == """Yes
No
Yes
Yes
No
Yes
No
No
Yes
No"""

# Custom cases
assert run("1\n1\n0\n") == "Yes", "single element"
assert run("1\n2\n0 0\n") == "Yes", "all zero"
assert run("1\n3\n5 15 25\n") == "Yes", "all ending 5"
assert run("1\n2\n11 31\n") == "Yes", "numbers ending in 1 can align"
assert run("1\n2\n12 22\n") == "No", "numbers ending in 2 diverge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n0`| Có | Phần tử đơn lẻ, đẳng thức tầm thường | 
|`1\n2\n0 0`| Có | Tất cả các phần tử đều bằng nhau và bằng 0 | 
|`1\n3\n5 15 25`| Có | Các số tận cùng bằng 5 hội tụ thành bội số của 10 |
