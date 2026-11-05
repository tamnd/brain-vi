---
title: "CF 1209A - Vẽ các con số"
description: "Chúng ta được cung cấp một danh sách các số nguyên và chúng ta muốn phân chia chúng thành càng ít nhóm càng tốt. Mỗi nhóm có một ràng buộc về cấu trúc: nếu bạn nhìn vào số nhỏ nhất trong nhóm đó, mọi số khác được gán cho cùng một nhóm phải chia hết cho số nhỏ nhất đó."
date: "2026-06-11T23:18:39+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1209
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 584 - Dasha Code Championship - Elimination Round (rated, open for everyone, Div. 1 + Div. 2)"
rating: 800
weight: 1209
solve_time_s: 101
verified: true
draft: false
---

[CF 1209A - Vẽ các con số](https://codeforces.com/problemset/problem/1209/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên và chúng ta muốn phân chia chúng thành càng ít nhóm càng tốt. Mỗi nhóm có một ràng buộc về cấu trúc: nếu bạn nhìn vào số nhỏ nhất trong nhóm đó, mọi số khác được gán cho cùng một nhóm phải chia hết cho số nhỏ nhất đó. 

Bạn có thể coi mỗi nhóm được “neo” bởi phần tử tối thiểu của nó. Khi bạn chọn mức tối thiểu đó, mọi thứ khác trong nhóm phải là bội số của nó. Nhiệm vụ là gán mọi số cho một số nhóm đồng thời giảm thiểu số lượng nhóm được tạo. 

Ràng buộc các giá trị tối đa là 100 sẽ thay đổi đáng kể bản chất của vấn đề. Thay vì cần một đồ thị phức tạp hoặc DP trên các trạng thái lớn, chúng ta có thể suy luận trực tiếp về tần số và cấu trúc chia hết giữa các số nguyên nhỏ. Bất kỳ giải pháp nào cố gắng liệt kê rõ ràng các phân vùng hoặc gán các phần tử một cách tham lam mà không có chế độ xem toàn cục đều có nguy cơ thiếu các nhóm tối ưu, đặc biệt vì cùng một giá trị có thể xuất hiện nhiều lần và phải được xử lý nhất quán. 

Một trường hợp thất bại tinh vi đối với cách suy luận ngây thơ xuất hiện khi các số chia sẻ các ước số nhưng không trực tiếp trong một chuỗi. Ví dụ, trong`[6, 10, 15]`, một cách tiếp cận tham lam có thể cố gắng kết hợp 6 và 10 hoặc 10 và 15 dựa trên khả năng tương thích một phần, nhưng không ai trong số chúng có thể chia sẻ một nhóm trừ khi tất cả các phần tử đều là bội số của mức tối thiểu đã chọn. Vì không có cặp nào có chung mức tối thiểu hợp lệ để chia cả hai nên câu trả lời đúng là 3 nhóm. 

Một trường hợp tinh tế khác là sử dụng lại số chia và số chia. TRONG`[2, 4, 8, 3, 9]`, thật hấp dẫn khi nghĩ rằng chúng ta có thể “tái sử dụng” một nhóm cho nhiều chuỗi không liên quan, nhưng mức tối thiểu của mỗi nhóm buộc phải chia hết nghiêm ngặt, vì vậy chúng ta phải quyết định cẩn thận những số nào đóng vai trò là điểm neo. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng gán từng phần tử vào một trong một số nhóm và kiểm tra tính hợp lệ. Đối với mỗi phép gán, chúng ta sẽ xác minh rằng trong mỗi nhóm, tất cả các số đều chia hết cho phần tử nhỏ nhất của nhóm đó. Điều này dẫn đến không gian tìm kiếm theo cấp số nhân vì mỗi phần tử có nhiều lựa chọn nhóm và chúng ta không biết trước số lượng nhóm tối ưu. Ngay cả khi cắt tỉa, số lượng phân vùng của n phần tử tăng quá nhanh khi n lên tới 100. 

Quan sát cấu trúc quan trọng là tư cách thành viên nhóm được xác định hoàn toàn bởi mức tối thiểu đã chọn. Nếu một số x là số nhỏ nhất của một nhóm thì mọi phần tử trong nhóm đó phải là bội số của x. Điều này có nghĩa là với mỗi giá trị x có thể, chúng ta có thể xem xét liệu nó có nên đóng vai trò đại diện cho nhóm hay không và sau đó gán tất cả các bội số của nó cho nhóm đó nếu chúng chưa được gán ở nơi khác. 

Khi chúng ta diễn giải vấn đề theo cách này, mục tiêu sẽ là chọn một tập hợp các “mỏ neo” sao cho mỗi số thuộc về chính xác một nhóm neo và mỗi mỏ neo chỉ có thể bao gồm các số chia hết cho chính nó. Chiến lược tối ưu là xử lý các số từ nhỏ nhất đến lớn nhất và bất cứ khi nào chúng ta gặp một số chưa được gán cho bất kỳ nhóm nào trước đó, nó phải bắt đầu một nhóm mới. Sau đó, chúng tôi đánh dấu tất cả các bội số của nó là thuộc nhóm đó. 

Điều này hiệu quả vì số lượng nhỏ hơn luôn là điểm neo tốt hơn: chúng chia nhiều số hơn và do đó tạo ra các nhóm linh hoạt hơn. Số lượng lớn hơn không thể đóng vai trò là điểm neo hữu ích cho số lượng nhỏ hơn, vì vậy việc trì hoãn các quyết định sẽ chỉ có nguy cơ mất đi sự phân nhóm tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân vùng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Neo số chia tham lam | O(n + maxA log maxA) | O(maxA) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng cấu trúc mảng hoặc tần số để theo dõi những giá trị nào vẫn chưa được gán. 

1. Khởi tạo một mảng boolean`used`trên phạm vi giá trị. Điều này theo dõi xem một số đã được gán cho một số nhóm hay chưa. 
2. Lặp lại các giá trị có thể từ 1 đến giá trị tối đa trong đầu vào. Thứ tự này đảm bảo chúng tôi luôn xem xét các nhóm tiềm năng từ nhỏ nhất đến lớn nhất. 
3. Khi chúng ta gặp một giá trị`x`xuất hiện trong đầu vào và chưa được đánh dấu là đã sử dụng, chúng tôi quyết định rằng`x`bắt đầu một nhóm mới. Lựa chọn này là bắt buộc vì không tồn tại ước số nhỏ hơn trước đó có thể neo hợp lệ`x`. 
4. Một lần`x`được chọn làm nhóm tối thiểu, chúng tôi đánh dấu mọi bội số của`x`như đã sử dụng. Điều này thể hiện việc gán tất cả các số chia hết cho`x`vào nhóm này. Lý do điều này an toàn là vì mọi bội số của`x`thỏa mãn ràng buộc về tính chia hết đối với`x`. 
5. Tiếp tục quét lên trên, lặp lại quá trình bất cứ khi nào chúng ta tìm thấy số chưa sử dụng xuất hiện trong mảng. 

Câu trả lời cuối cùng là số lần chúng ta bắt đầu một nhóm mới. 

Tính chính xác phụ thuộc vào tính bất biến mà bất cứ khi nào chúng ta bắt đầu một nhóm ở giá trị x, mọi số nhỏ hơn x đều đã được xử lý và gán đầy đủ và x là giá trị nhỏ nhất chưa được gán. Điều này đảm bảo x không thể được đặt vào bất kỳ nhóm nào sớm hơn, bởi vì bất kỳ nhóm nào sớm hơn sẽ có mức tối thiểu nhỏ hơn không chia hết x. Do đó, việc bắt đầu một nhóm mới tại x là không thể tránh khỏi và việc tham lam bao phủ tất cả các bội số sẽ tối đa hóa phạm vi bao phủ mà không có phép gán không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    maxv = max(a)
    freq = [0] * (maxv + 1)
    
    for x in a:
        freq[x] += 1

    used = [False] * (maxv + 1)
    groups = 0

    for x in range(1, maxv + 1):
        if freq[x] > 0 and not used[x]:
            groups += 1
            for multiple in range(x, maxv + 1, x):
                used[multiple] = True

    print(groups)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên nén đầu vào thành một mảng tần số để chúng tôi có thể nhanh chóng kiểm tra xem giá trị có tồn tại hay không. các`used`mảng theo dõi xem một giá trị đã được hấp thụ vào một số nhóm trước đó hay chưa. 

Vòng lặp kết thúc`x`đảm bảo chúng tôi luôn xử lý các trưởng nhóm tiềm năng theo thứ tự tăng dần. Khi chúng tôi chọn một người lãnh đạo mới, chúng tôi ngay lập tức đánh dấu tất cả các bội số của nó, điều này ngăn cản họ hình thành các nhóm dư thừa sau này. 

Một lỗi phổ biến là lặp lại trực tiếp mảng đầu vào thay vì phạm vi giá trị. Điều đó phá vỡ cấu trúc số chia vì việc nhóm phụ thuộc vào mối quan hệ số chứ không phải thứ tự đầu vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[10, 2, 3, 5, 4, 2]`| x | tần số[x] | đã sử dụng[x] | Hành động | Nhóm | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | - | bỏ qua | 0 | 
| 2 | 2 | Sai | nhóm bắt đầu lúc 2, điểm 2,4 | 1 | 
| 3 | 1 | Sai | nhóm bắt đầu lúc 3, điểm 3 | 2 | 
| 4 | 1 | Đúng | đã được sử dụng | 2 | 
| 5 | 1 | Sai | nhóm bắt đầu lúc 5, điểm 5,10 | 3 | 
| 6.. | 0 | - | bỏ qua | 3 | 

Đáp án cuối cùng là 3 nhóm. 

Dấu vết này cho thấy các ước số nhỏ ban đầu hấp thụ bội số lớn hơn như thế nào, ngăn chúng hình thành các nhóm bổ sung sau này. 

### Ví dụ 2 

đầu vào:`[6, 2, 3, 4, 12]`| x | tần số[x] | đã sử dụng[x] | Hành động | Nhóm | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | - | bỏ qua | 0 | 
| 2 | 1 | Sai | nhóm bắt đầu lúc 2, điểm 2,4 | 1 | 
| 3 | 1 | Sai | nhóm bắt đầu lúc 3, điểm 3,6,12 | 2 | 
| 4 | 1 | Đúng | bỏ qua | 2 | 
| 5 | 0 | - | bỏ qua | 2 | 
| 6 | 1 | Đúng | bỏ qua | 2 | 

Đáp án cuối cùng là 2 nhóm. 

Điều này chứng tỏ rằng khi một nhóm ước số được hình thành, nó có thể hấp thụ các bội số sâu rộng, bao gồm các giá trị không phải là bội số ngay lập tức theo thứ tự đầu vào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(maxA log maxA) | mỗi số đánh dấu bội số lên tới 100 | 
| Không gian | O(maxA) | mảng tần số và sổ sách kế toán | 

Giới hạn đủ nhỏ để việc lặp qua bội số cực kỳ nhanh. Ngay cả trong trường hợp xấu nhất, số lượng thao tác vẫn ở dưới mức giới hạn vì giá trị tối đa chỉ là 100. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return io.StringIO()

# corrected run wrapper
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("6\n10 2 3 5 4 2\n") == "3"

# all equal
assert run("4\n2 2 2 2\n") == "1"

# primes only
assert run("3\n7 11 13\n") == "3"

# single chain
assert run("5\n2 4 8 16 32\n") == "1"

# mixed divisibility
assert run("5\n3 6 2 4 9\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | 1 | mỏ neo duy nhất chiếm ưu thế | 
| chỉ số nguyên tố | n | không thể hợp nhất | 
| sức mạnh của hai | 1 | chuỗi chia đầy đủ | 
| giá trị hỗn hợp | 2 | tương tác của các nhóm | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các số là số nguyên tố cùng nhau theo cặp, chẳng hạn như`[7, 11, 13]`. Thuật toán bắt đầu một nhóm mới ở mỗi giá trị vì không có giá trị nào chia nhóm khác, tạo ra ba nhóm, điều này là tối ưu vì không thể nhóm được. 

Một trường hợp khác là chuỗi chia số được lồng hoàn toàn như`[2, 4, 8, 16]`. Thuật toán bắt đầu từ 2, đánh dấu tất cả các bội số và kết thúc bằng một nhóm. Điều này cho thấy việc neo sớm sẽ thu gọn các chuỗi dài thành một nhóm một cách chính xác. 

Trường hợp thứ ba liên quan đến các giá trị lặp lại như`[3, 3, 6, 6, 12]`. 3 cái đầu tiên trở thành mỏ neo và tất cả những cái khác sẽ được hấp thụ ngay lập tức, xác nhận rằng các bản sao không làm tăng số lượng nhóm.
