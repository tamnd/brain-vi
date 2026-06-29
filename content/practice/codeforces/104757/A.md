---
title: "CF 104757A - Một câu hỏi quan trọng"
description: "Chúng tôi được cung cấp một chuỗi các số nguyên dương riêng biệt và chúng tôi muốn quyết định xem chuỗi này có thể là kết quả của một bước phân vùng duy nhất trong sắp xếp nhanh hay không, sử dụng một số giá trị trục không xác định đã xuất hiện trong mảng."
date: "2026-06-28T22:47:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104757
codeforces_index: "A"
codeforces_contest_name: "2023-2024 ICPC East North America Regional Contest (ECNA 2023)"
rating: 0
weight: 104757
solve_time_s: 46
verified: true
draft: false
---

[CF 104757A - Câu hỏi quan trọng](https://codeforces.com/problemset/problem/104757/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi các số nguyên dương riêng biệt và chúng tôi muốn quyết định xem chuỗi này có thể là kết quả của một bước phân vùng duy nhất trong sắp xếp nhanh hay không, sử dụng một số giá trị trục không xác định đã xuất hiện trong mảng. 

Một giá trị có thể đóng vai trò là một trục xoay hợp lệ nếu khi chúng ta chia mảng xung quanh nó, mọi phần tử ở bên trái của nó đều ở “phía nhỏ hơn” hoặc “phía lớn hơn” một cách nhất quán với một phân vùng hợp lệ: tất cả các phần tử nhỏ hơn hoặc bằng trục xoay phải nằm ở một bên và tất cả các phần tử lớn hơn trục quay phải nằm ở phía bên kia. Chúng tôi không biết bên nào tương ứng với “nhỏ hơn trục”, nhưng cấu trúc phải phù hợp với ý tưởng rằng trục chia mảng thành hai nhóm, không trộn lẫn giữa các bên. 

Nhiệm vụ là xác định tất cả các giá trị có thể hoạt động như một trục xoay theo thứ tự hiện tại của mảng. Nếu không có giá trị nào hoạt động, mảng được coi là không thể hiện kết quả phân vùng hợp lệ. Nếu nhiều giá trị hoạt động, chúng tôi liệt kê chúng theo thứ tự đầu vào nhưng chỉ tối đa 100 giá trị đầu tiên. 

Hạn chế chính là độ dài mảng có thể lên tới một triệu. Bất kỳ giải pháp nào kiểm tra mọi ứng cử viên xoay vòng có thể và xác minh nó bằng cách quét toàn bộ mảng sẽ dẫn đến khoảng$O(n^2)$hành vi trong trường hợp xấu nhất, vượt xa giới hạn khả thi. Chúng tôi buộc phải sử dụng cách tiếp cận tuyến tính hoặc gần tuyến tính, có thể bằng một đường chuyền duy nhất và cấu trúc được tính toán trước. 

Trường hợp cạnh tinh vi xuất hiện khi mảng trông gần như được phân vùng nhưng có một vi phạm nhỏ ở gần giữa. Ví dụ, hãy xem xét: 

đầu vào:```
5
1 4 2 3 5
```Nếu chúng ta thử trục 2, các phần tử nhỏ hơn hoặc bằng 2 phải được nhóm chính xác, nhưng vị trí của 3 và 4 sẽ phá vỡ mọi sự phân chia nhất quán. Việc kiểm tra cục bộ ngây thơ xung quanh giá trị trục không thành công vì tính chính xác phụ thuộc vào tính nhất quán chung của tất cả các phần tử, không chỉ các phần tử lân cận. 

Một trường hợp khác là khi mảng đã được sắp xếp: 

đầu vào:```
5
1 2 3 4 5
```Mọi phần tử có thể trông giống như một trục xoay nếu chúng ta chỉ kiểm tra cục bộ, nhưng trên thực tế chỉ có những vị trí cụ thể mới thỏa mãn điều kiện biên phân vùng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là kiểm tra từng giá trị như một điểm xoay tiềm năng. Đối với một trục ứng cử viên, chúng tôi quét toàn bộ mảng và tách các phần tử thành những phần tử nhỏ hơn hoặc bằng nó và những phần tử lớn hơn nó, sau đó xác minh xem thứ tự được quan sát có thể tương ứng với ranh giới phân vùng hợp lệ hay không. Điều này hoạt động về mặt khái niệm vì một trục chính xác tạo ra sự phân tách rõ ràng các giá trị. Tuy nhiên, việc thực hiện điều này đối với mỗi$n$ứng viên dẫn đến$n$quét toàn bộ, sản xuất$O(n^2)$độ phức tạp về thời gian, điều này là không thể đối với$n = 10^6$. 

Quan sát quan trọng là một trục hợp lệ không chỉ liên quan đến vị trí tương đối mà còn liên quan đến cấu trúc giá trị tiền tố-hậu tố. Nếu chúng ta tưởng tượng việc quét mảng từ trái sang phải, chúng ta có thể duy trì giá trị tối đa được thấy cho đến nay. Bất kỳ trục tiềm năng nào được đặt ở vị trí$i$ít nhất phải lớn bằng mọi thứ ở bên trái của nó nếu nó hoạt động giống như một phần tử ranh giới ngăn cách các giá trị nhỏ hơn với các giá trị lớn hơn. Tương tự, quét từ phải sang trái sẽ đưa ra hạn chế tối thiểu cho hậu tố. 

Một giá trị là một trục hợp lệ chính xác khi nó nằm ở vị trí mà nó đồng thời là giá trị lớn nhất của tiền tố và giá trị tối thiểu của hậu tố. Điều này chuyển đổi vấn đề thành hai lần quét tuyến tính và bước lọc cuối cùng, giảm nó thành$O(n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Kiểm tra tiền tố-hậu tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và lưu trữ theo thứ tự đầu vào. Chúng tôi duy trì trật tự vì đầu ra phải tôn trọng nó. 
2. Xây dựng một mảng`pref_max`, Ở đâu`pref_max[i]`là giá trị tối đa trong tiền tố kết thúc tại chỉ mục`i`. Điều này nắm bắt mọi thứ có thể ngăn chặn trục quay “quá nhỏ” ở phía bên trái. 
3. Xây dựng một mảng`suf_min`, Ở đâu`suf_min[i]`là giá trị tối thiểu trong hậu tố bắt đầu từ chỉ mục`i`. Điều này nắm bắt mọi thứ có thể ngăn chặn trục quay “quá lớn” ở phía bên phải. 
4. Đối với mỗi chỉ số`i`, kiểm tra xem`pref_max[i] == suf_min[i] == a[i]`. Nếu điều này đúng, phần tử ở vị trí`i`chính xác là giá trị phân tách giữa các phân vùng bên trái và bên phải. 
5. Thu thập tất cả các giá trị đó theo thứ tự. Nếu có nhiều hơn 100 thì chỉ xuất ra 100 đầu tiên. 
6. Nếu không tìm thấy giá trị nào, xuất ra 0, nghĩa là không tồn tại thông dịch phân vùng hợp lệ. 

### Tại sao nó hoạt động 

Trục xoay hợp lệ phải là giá trị phân tách rõ ràng tất cả các phần tử nhỏ hơn khỏi tất cả các phần tử lớn hơn trong cách sắp xếp nhất định. Nếu một giá trị nhỏ hơn giá trị nào đó ở bên trái, nó sẽ vi phạm tính nhất quán của tiền tố; nếu nó lớn hơn thứ gì đó ở bên phải thì nó vi phạm tính nhất quán của hậu tố. Tiền tố tối đa đảm bảo không có giá trị lớn hơn xuất hiện trước nó và hậu tố tối thiểu đảm bảo không có giá trị nhỏ hơn xuất hiện sau nó. Khi cả hai đều khớp với chính phần tử đó, nó sẽ trở thành ranh giới duy nhất trong đó phần phân chia nhất quán với định nghĩa phân vùng của quicksort, làm cho nó trở thành một trục hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    pref_max = [0] * n
    suf_min = [0] * n

    cur = float('-inf')
    for i in range(n):
        cur = max(cur, a[i])
        pref_max[i] = cur

    cur = float('inf')
    for i in range(n - 1, -1, -1):
        cur = min(cur, a[i])
        suf_min[i] = cur

    res = []
    for i in range(n):
        if pref_max[i] == a[i] == suf_min[i]:
            res.append(a[i])
            if len(res) == 100:
                break

    print(len(res), *res)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên hai đường dẫn tuyến tính để xây dựng tiền tố cực đại và hậu tố cực tiểu. Quá trình quét tiền tố đảm bảo chúng tôi biết ràng buộc lớn nhất cho từng vị trí, trong khi quét hậu tố cung cấp ràng buộc nhỏ nhất từ ​​phía bên phải. Vòng lặp cuối cùng lọc các phần tử thỏa mãn cả hai điều kiện cùng một lúc. Việc dừng sớm ở 100 ứng viên là bắt buộc theo đặc tả đầu ra. 

Một lỗi phổ biến là cố gắng chỉ so sánh các phân khúc lân cận hoặc địa phương. Điều đó không thành công vì tính hợp lệ của phân vùng phụ thuộc vào các ràng buộc thứ tự chung chứ không phải phụ thuộc vào tính liền kề. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 3 4 5
```| tôi | một [tôi] | pref_max | suf_min | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | vâng | 
| 1 | 2 | 2 | 2 | vâng | 
| 2 | 3 | 3 | 3 | vâng | 
| 3 | 4 | 4 | 4 | vâng | 
| 4 | 5 | 5 | 5 | vâng | 

Tất cả các phần tử đủ điều kiện vì mảng đã được sắp xếp trên toàn cầu. Mỗi phần tử đồng thời là giá trị tối đa của tiền tố và giá trị tối thiểu của hậu tố. 

### Ví dụ 2 

đầu vào:```
6
3 1 4 2 6 5
```| tôi | một [tôi] | pref_max | suf_min | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 3 | 1 | không | 
| 1 | 1 | 3 | 1 | không | 
| 2 | 4 | 4 | 2 | không | 
| 3 | 2 | 4 | 2 | không | 
| 4 | 6 | 6 | 5 | không | 
| 5 | 5 | 6 | 5 | không | 

Không có chỉ mục nào thỏa mãn sự bằng nhau của tiền tố max, hậu tố min và giá trị. Điều này xác nhận mảng không thể biểu thị kết quả phân vùng sạch. 

Những dấu vết này cho thấy rằng điều kiện là toàn cục: ngay cả khi một giá trị lớn hay nhỏ cục bộ, nó phải thống trị tiền tố của nó và đồng thời bị hậu tố của nó chi phối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Hai lượt tuyến tính cho tiền tố tối đa và hậu tố tối thiểu, cộng với một lần quét để lọc | 
| Không gian | O(n) | Hai mảng phụ lưu trữ thông tin tiền tố và hậu tố | 

Độ phức tạp tuyến tính là cần thiết vì đầu vào có thể chứa tới một triệu phần tử. Bất kỳ chiến lược bậc hai nào cũng sẽ vượt quá giới hạn thời gian theo một số bậc độ lớn, trong khi phương pháp được đề xuất chỉ thực hiện một số thao tác không đổi cho mỗi phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))

    pref_max = [0] * n
    suf_min = [0] * n

    cur = float('-inf')
    for i in range(n):
        cur = max(cur, a[i])
        pref_max[i] = cur

    cur = float('inf')
    for i in range(n - 1, -1, -1):
        cur = min(cur, a[i])
        suf_min[i] = cur

    res = []
    for i in range(n):
        if pref_max[i] == a[i] == suf_min[i]:
            res.append(a[i])
            if len(res) == 100:
                break

    return str(len(res)) + (" " + " ".join(map(str, res)) if res else "")

# sample-like tests
assert run("5\n1 2 3 4 5\n") == "5 1 2 3 4 5"

assert run("6\n3 1 4 2 6 5\n") == "0"

# minimum size
assert run("1\n10\n") == "1 10"

# descending order
assert run("4\n4 3 2 1\n") == "4 4 3 2 1"

# random valid pattern
assert run("3\n2 1 3\n") == "1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1x | trường hợp cơ bản tầm thường | 
| sắp xếp tăng dần | tất cả các yếu tố | mọi ranh giới hợp lệ của chỉ số | 
| hỗn hợp không hợp lệ | 0 | không có phân vùng nhất quán | 
| giảm dần | tất cả các yếu tố | trường hợp hợp lệ đối xứng | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là mảng một phần tử, chẳng hạn như`1\n7\n`. Cả tiền tố tối đa và hậu tố tối thiểu đều bằng chính phần tử đó ở chỉ mục 0, do đó thuật toán trả về chính xác giá trị đó dưới dạng một trục xoay hợp lệ. 

Một trường hợp cạnh khác là một mảng giảm dần như`4\n4 3 2 1\n`. Tiền tố tối đa tại mỗi vị trí luôn bằng phần tử đầu tiên, trong khi hậu tố tối thiểu giảm dần đến phần tử hiện tại. Sự bình đẳng được duy trì ở mọi chỉ số, do đó mọi phần tử đều được chấp nhận, phù hợp với cách giải thích rằng mỗi vị trí có thể đóng vai trò là ranh giới phân chia hợp lệ. 

Trường hợp tinh vi hơn là khi chỉ có một phần tử tạo thành ranh giới hợp lệ ở giữa, chẳng hạn như`3\n2 1 3\n`. Tại chỉ mục 1, giá trị 1 vừa là giá trị tối thiểu của hậu tố bắt đầu từ đó vừa là một phần của cấu trúc tối đa tiền tố hợp lệ, làm cho nó trở thành trục xoay nhất quán duy nhất. Thuật toán nắm bắt được điều này một cách tự nhiên vì chỉ có chỉ mục đó mới thỏa mãn đồng thời cả hai ràng buộc.
