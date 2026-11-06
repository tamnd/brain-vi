---
title: "CF 1295C - Lấy Chuỗi"
description: "Chúng ta được cho hai chuỗi, s và t. Ban đầu, chúng ta có một chuỗi z rỗng và chúng ta muốn biến z thành t. Hoạt động được phép là nối bất kỳ chuỗi con nào của s vào z. Một dãy con được hình thành bằng cách chọn các ký tự từ s mà không thay đổi thứ tự tương đối của chúng."
date: "2026-06-11T18:36:22+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1295
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 81 (Rated for Div. 2)"
rating: 1600
weight: 1295
solve_time_s: 102
verified: true
draft: false
---

[CF 1295C - Lấy chuỗi](https://codeforces.com/problemset/problem/1295/C) 

**Đánh giá:** 1600 
**Tags:** dp, tham lam, dây 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai chuỗi,`s`Và`t`. Ban đầu, chúng ta có một chuỗi trống`z`, và chúng tôi muốn biến đổi`z`vào trong`t`. Hoạt động được phép là nối thêm bất kỳ chuỗi con nào của`s`ĐẾN`z`. Một dãy con được hình thành bằng cách chọn các ký tự từ`s`mà không thay đổi thứ tự tương đối của chúng. Mỗi lần chúng ta thực hiện thao tác, chúng ta có thể nối thêm bất kỳ số lượng ký tự nào từ`s`theo thứ tự, nhưng chúng tôi không thể sắp xếp lại`s`hoặc sử dụng ký tự mới bên ngoài`s`. 

Nhiệm vụ của chúng ta là xác định số lượng tối thiểu các thao tác như vậy cần thiết để xây dựng`t`. Nếu không thể hình thành`t`bởi vì một số nhân vật trong`t`không xuất hiện trong`s`, chúng ta nên quay lại`-1`. 

Những ràng buộc ngụ ý rằng`s`Và`t`có thể lên đến`10^5`mỗi ký tự và tổng chiều dài trên tất cả các trường hợp thử nghiệm là`2*10^5`. Với giới hạn thời gian 1 giây, bất kỳ giải pháp nào có độ phức tạp kém hơn`O(|s| + |t|)`mỗi trường hợp thử nghiệm có thể hết thời gian chờ. Các vòng lặp lồng nhau trên cả hai chuỗi sẽ mang lại`O(|s|*|t|)`quá chậm. 

Các trường hợp khó khăn mà một giải pháp đơn giản có thể thất bại bao gồm`t`chứa một ký tự không có trong`s`, Ví dụ,`s = "abc"`Và`t = "d"`. Đầu ra đúng là`-1`. Một trường hợp tế nhị khác là khi`t`có thể được hình thành bằng cách tái sử dụng`s`nhiều lần trong các chuỗi con không chồng chéo, chẳng hạn như`s = "ab"`,`t = "ababab"`, đòi hỏi phải đếm cẩn thận số lần`s`phải được thêm vào. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực là bắt đầu ngay từ đầu`t`và liên tục cố gắng tìm dãy con dài nhất của`s`phù hợp với hậu tố hiện tại của`t`. Mỗi lần một dãy con được sử dụng, chúng ta sẽ tiến tới`t`theo độ dài của dãy con phù hợp và lặp lại cho đến hết`t`. Điều này có hiệu quả vì mỗi thao tác đều tăng độ dài của`z`dọc theo`t`. Tuy nhiên, việc tìm ra dãy con một cách ngây thơ đòi hỏi phải quét qua`s`cho mỗi nhân vật trong`t`. Nếu như`s`Và`t`cả hai đều có chiều dài`10^5`, độ phức tạp trong trường hợp xấu nhất trở thành`O(|s|*|t|)`hoặc`10^{10}`hoạt động, rõ ràng là quá chậm. 

Cái nhìn sâu sắc quan trọng là chúng ta có thể xử lý trước`s`để cho phép kết hợp chuỗi con nhanh hơn. Chúng ta có thể ghi lại vị trí của từng nhân vật trong`s`sao cho với mỗi ký tự trong`t`, chúng ta có thể nhanh chóng tìm thấy sự xuất hiện tiếp theo trong`s`sau một chỉ số nhất định. Điều này có thể thực hiện được bằng cách sử dụng từ điển danh sách và tìm kiếm nhị phân. Với quá trình tiền xử lý này, chúng ta có thể duyệt qua`t`tuyến tính trong khi định vị hiệu quả các kết quả phù hợp trong`s`, giảm độ phức tạp xuống`O(|s| + |t| log |s|)`. 

Lực lượng vũ phu hoạt động vì chúng tôi luôn cố gắng để phù hợp`t`sử dụng dãy con của`s`, nhưng nó thất bại khi`t`dài. Sự quan sát đó`s`là tĩnh và có thể được lập chỉ mục cho phép chúng tôi mô phỏng các thao tác nối thêm chuỗi tiếp theo trong quá trình quét tuyến tính với tìm kiếm nhị phân cho ký tự tiếp theo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O( | s | * | 
| Tối ưu | O( | s | + | 

## Hướng dẫn thuật toán 

1. Tiền xử lý`s`để ghi lại chỉ số của từng ký tự. Tạo một từ điển`pos`như vậy`pos[c]`là danh sách được sắp xếp của tất cả các chỉ mục trong đó ký tự`c`xảy ra ở`s`. Điều này cho phép chúng tôi nhanh chóng xác định vị trí xuất hiện tiếp theo của`c`sau một chỉ mục nhất định bằng cách sử dụng tìm kiếm nhị phân. 
2. Khởi tạo bộ đếm`operations = 1`để theo dõi số lượng phần bổ sung tiếp theo mà chúng ta sẽ cần. Bắt đầu quét`t`từ ký tự đầu tiên và giữ một con trỏ`current_index`đến vị trí hiện tại trong`s`nơi chúng tôi đang cố gắng khớp ký tự tiếp theo của`t`. 
3. Đối với mỗi nhân vật`c`TRONG`t`, kiểm tra xem`c`tồn tại ở`s`. Nếu không thì quay lại ngay`-1`bởi vì nó không thể hình thành`t`. 
4. Sử dụng danh sách được xử lý trước`pos[c]`, tìm lần xuất hiện đầu tiên của`c`TRONG`s`cái đó đến sau`current_index`. Nếu xảy ra hiện tượng như vậy, hãy di chuyển`current_index`với chỉ số đó + 1. Điều này mô phỏng việc so khớp`c`ở dãy sau hiện tại. 
5. Nếu không xảy ra hiện tượng`c`sau đó`current_index`, điều này có nghĩa là chúng ta phải bắt đầu nối thêm chuỗi con mới. Tăng`operations`, và đặt`current_index`đến lần xuất hiện đầu tiên của`c`TRONG`s`+ 1. 
6. Tiếp tục quá trình này cho đến khi tất cả các ký tự trong`t`được khớp. 

### Tại sao nó hoạt động 

Tính bất biến đó là`current_index`luôn trỏ đến ký tự có sẵn tiếp theo trong`s`cho phần tiếp theo hiện tại nối thêm. Mỗi lần chúng tôi không thể phù hợp với một nhân vật ngoài`current_index`, chúng ta bắt đầu một dãy con mới. Điều này đảm bảo rằng mỗi phần bổ sung có độ dài tối đa và chúng tôi đếm số lượng thao tác tối thiểu. Không có ký tự nào bị bỏ qua và mọi ký tự trong`t`được khớp với một ký tự tương ứng trong`s`, đảm bảo tính đúng đắn 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline

def min_operations(s, t):
    pos = {}
    for i, c in enumerate(s):
        if c not in pos:
            pos[c] = []
        pos[c].append(i)
    
    operations = 1
    current_index = -1
    
    for c in t:
        if c not in pos:
            return -1
        idx_list = pos[c]
        i = bisect.bisect_right(idx_list, current_index)
        if i == len(idx_list):
            operations += 1
            current_index = idx_list[0]
        else:
            current_index = idx_list[i]
    return operations

T = int(input())
for _ in range(T):
    s = input().strip()
    t = input().strip()
    print(min_operations(s, t))
```Mã tiền xử lý đầu tiên`s`để tạo ra`pos`, một từ điển danh sách. Với mỗi ký tự trong`t`, nó sử dụng`bisect_right`để tìm sự xuất hiện tiếp theo sau`current_index`. Nếu không tìm thấy vị trí hợp lệ, nó sẽ tăng bộ đếm hoạt động và khởi động lại từ lần xuất hiện đầu tiên. Điều này đảm bảo chúng tôi đếm được số lượng tối thiểu các chuỗi tiếp theo được thêm vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`s = "aabce"`,`t = "ace"`| t_char | current_index | vị trí[c] | kết quả chia đôi | hoạt động | 
| --- | --- | --- | --- | --- | 
| một | -1 | [0,1] | 0 | 1 | 
| c | 1 | [3] | 0 | 1 | 
| e | 3 | [4] | 0 | 1 | 

Tất cả các nhân vật của`t`có thể được khớp trong một lần truyền duy nhất, vì vậy đầu ra là`1`. 

### Ví dụ 2 

đầu vào:`s = "abacaba"`,`t = "aax"`| t_char | current_index | vị trí[c] | kết quả chia đôi | hoạt động | 
| --- | --- | --- | --- | --- | 
| một | -1 | [0,2,4,6] | 0 | 1 | 
| một | 0 | [0,2,4,6] | 1 | 1 | 
| x | 2 | [] | - | -1 | 

Tính cách`x`không xuất hiện trong`s`, vậy đầu ra là`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | s | 
| Không gian | O( | s | 

Giải pháp dễ dàng phù hợp trong giới hạn vấn đề. Ngay cả với kích thước đầu vào lớn nhất, tìm kiếm nhị phân và tiền xử lý vẫn giữ cho thuật toán hoạt động hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# Provided samples
assert run("3\naabce\nace\nabacaba\naax\nty\nyyt\n") == "1\n-1\n3", "sample 1"

# Custom tests
assert run("1\na\naaaaa\n") == "5", "repeat single char"
assert run("1\nabc\nabcabc\n") == "2", "s reused twice"
assert run("1\nxyz\nxyz\n") == "1", "all same length"
assert run("1\nabc\ndef\n") == "-1", "impossible case"
assert run("1\nabc\ncab\n") == "2", "reordering requires multiple ops"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "a\naaaaa\n" | 5 | Việc lặp lại cùng một ký tự cần có nhiều phần bổ sung |
